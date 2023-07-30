# DataSphere Studio 1.1.1 升级到 1.1.2 使用文档

### 升级步骤主要分为：
- 服务停止
- 执行数据库升级脚本
- 替换相关jar包
- 替换前端包
- 服务启动

### 1. 服务停止
进入到dss的部署目录，在目录下执行命令停止dss的所有服务：
```shell
cd ${DSS_DEPLOY_PATH}

sh sbin/dss-stop-all.sh
```
### 2. 执行数据库升级sql脚本

升级sql脚本获取方式：

- dss1.1.2的安装包解压后的目录：db/version_update/from_v111_to_v112.sql

然后登陆dss数据库执行source命令：

```shell
source ${your_path}/from_v111_to_v112.sql
```
正常情况下可以执行成功。

### 3. 替换相关jar包和修改配置文件
#### 3.1 替换DSS的jar包
- 用户可以自行编译DSS1.1.2后端代码或参考文档 [DSS部署文档单机版](DSS部署文档单机版.md) 来获取DSS1.1.2的全量包
- 使用DSS1.1.2的lib去全量替换掉原有1.1.2 xxx/dss/目录下的的lib
#### 3.1 修改DSS的配置文件
- 新增配置文件dss-server.properties，将v1.1.1中dss-framework-project-server.properties, dss-framework-orchestrator-server.properties, dss-workflow-server.properties和dss-flow-execution-server.properties中配置合并到dss-server.properties中
- 新增配置文件dss-apps-server.properties，将v1.1.1中dss-scriptis-server.properties, dss-guide-server.properties和dss-apiservice-server.properties中配置合并到dss-apps-server.properties中

### 4. 替换前端包
- 用户可自行编译前端包或者从 [DSS部署文档单机版](DSS部署文档单机版.md) 来获取DSS1.1.2的全量包并将前端包拿出，替换掉`${DSS_HOME}/web/`下的dist即可

### 5. 服务启动
- 重启DSS服务，在**dss部署目录下**执行命令启动所有服务：

```shell
sh sbin/dss-start-all.sh 
```
- 重启nginx



