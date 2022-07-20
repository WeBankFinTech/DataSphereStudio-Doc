# DSS工作流内容从0.X版本迁移1.0版本说明文档

> 由于 DSS1.0 的工作流存储方式和 DSS0.X 相比存在一些结构差异，因此不能通过简单的替换jar包再复用0.X 的工作流数据内容的方式完成升级。需要先对原有的0.X项目内容进行导出，后在1.0进行导入的时候，迁移服务会自动解析导出包内容并重新在1.0进行工作流的创建。

> 迁移服务主要完成0.X的项目导出包解析、工作流信息提取、结构修改和1.0编排导入。用户只需要拿到0.X的项目导出包，并调用1.0的导入接口，就可以完成工作流内容的迁移。

## 主要变化点

- 新增编排和编排版本信息表，每一个编排版本会关联一个工作流ID，编排的内容与原来工作流的信息基本一致

- 去掉工作流的版本信息表，把工作流的版本放到编排的版本，每个工作流会存储bml的版本信息

- 项目信息表也有一些变化，去掉一些在1.0没有用到的字段

## 迁移步骤

### Step 1 导出 DSS0.X 的项目包

> 0.X导出可以直接复用现有的项目导出，在已有的发布操作中也会将项目进行导出。

#### 导出流程
1. 进入0.X环境
2. 选择一个工程的工作流，点击发布
3. 进入服务器0.X的部署目录
4. 接着进入dss-server的日志目录，通过查看日志，获取到本次解压的路径。路径是用户配置的调度导出路径。
5. 下载路径下zip包文件
6. 然后通过postman测试 /dss/framework/release/importOldDSSFlow

### Step 2 导入 DSS1.0 环境

导入接口: `http://ip:port/api/rest_j/v1/dss/framework/release/importOldDSSProject`

接口类型: POST

java导入示例:
```java
httpClient = HttpClients.custom().build();
MultipartEntityBuilder entityBuilder =  MultipartEntityBuilder.create();
entityBuilder.addBinaryBody("file",file);  //导出项目文件的File对象
entityBuilder.addTextBody("dssLabels", "{\"route\": \"dev\"}");  //导入环境的标签,可以省略
httpPost.setEntity(entityBuilder.build());
response = httpClient.execute(httpPost);
```


导入处理流程：

- 把 DSS0.X 导出的zip包上传到 DSS1.0 后，DSS1.0 会自动创建项目、导入编排信息、导入工作流信息，无需用户操作

- 用户若只关心导入是否成功，那么可直跳转到 校验导入内容

导入处理过程中后台主要实现步骤，用户使用时无需关注：

1. 下载导入的0.X项目zip包到本地

2. 对zip包的内容进行解析，获取项目信息、工作流信息、工作流关系等

3. 同步项目信息，如果项目不存在，则新建项目，如果已经存在，则支持重复导入，覆盖上次的导入内容。 由于在1.0的类定义发生了变化，需要替换项目元数据信息中存储的内容，如类的名称，替换成功后，使用修改后的元数据信息，创建项目

```java
  replaceFileStr(metaFilePath, "com.webank.wedatasphere.dss.common.entity.project.DWSProject",
                "com.webank.wedatasphere.dss.framework.project.entity.DSSProjectDO");
  replaceFileStr(metaFilePath, "com.webank.wedatasphere.dss.common.entity.flow.DWSFlow",
                "com.webank.wedatasphere.dss.workflow.common.entity.DSSFlow");
  replaceFileStr(metaFilePath, "com.webank.wedatasphere.dss.server.entity.DWSFlowRelation",
                "com.webank.wedatasphere.dss.workflow.common.entity.DSSFlowRelation");
```

4. 丰富相关内容，适配1.0的架构，补全编排信息

- 读取工作流导入文件中的工作流元数据信息meta.txt文件，以及工作流关系信息dss_flow_relation，建立工作流的依赖关系，根据所有的rootflow创建新的编排信息，并且写入到新的meta.txt, 因为原来导出的存储结构中没有包含编排的内容，需要依据工作流信息创建编排

- 调整工作流打包结构，按照1.0的规范是以编排为单位进行导入的，需要把0.x里面同一个父级工作流下的所有子工作流内容，放入到一个编排的导入包中。把原有以工作流为单位的导出内容
拷贝到以编排为单位的目录结构下，工作流的导出结构和1.0没有变化，可以直接复用

5. 将适配之后的目录打包成新的zip包

6. 上传到bml，将新打包好的编排的导入包上传到BML服务器，拿到ResourceId、Version

7. 通过resourceId 和 version 导入到 dev的 orchestrator-server，复用1.0已有的编排导入流程

### Step 3 校验导入内容

* 检查工作流数量是否有缺失
* 检查工作流节点是否有缺失
* 检查节点的内容是否正确
* 实时执行是否可以正常执行
* 工作流的资源文件都有同步

## 说明：
- 迁移只包含迁移项目导出的内容，主要是工作流的json，资源文件、节点脚本，节点关联第三方组件的定义。


- 不包括迁移项目的用户权限、脚本中关联的UDF函数、第三方组件除了节点导出以外的内容，如数据源、规则定义等。
    