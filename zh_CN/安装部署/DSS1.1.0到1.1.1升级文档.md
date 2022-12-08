# DataSphere Studio 1.1.0 升级到 1.1.1 使用文档

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

- dss1.1.1的安装包解压后的目录：db/version_update/from_v110_to_v111.sql

然后登陆dss数据库执行source命令：

```shell
source ${your_path}/from_v110_to_v111.sql
```
正常情况下可以执行成功。

### 3. 替换相关jar包

#### 3.1 替换DSS的jar包
- 用户可以自行编译DSS1.1.1后端代码或参考文档 [DSS&Linkis一键部署文档单机版](DSS&Linkis一键部署文档单机版.md) 来获取DSS1.1.1的全量包
- 使用DSS1.1.1的lib去全量替换掉原有1.1.0 xxx/dss/目录下的的lib

#### 3.2 DolphinScheduler涉及到的jar包替换
- `DolphinSchedulerAppConn` 插件安装包替换，可从此处下载：[点我下载插件安装包](https://osp-1257653870.cos.ap-guangzhou.myqcloud.com/WeDatasphere/DolphinScheduler/DSS1.1.1_dolphinscheduler/dolphinscheduler-appconn.zip) 解压该zip包后用将其中的lib和db全量替换原有的`${DSS_HOME}/dss-appconns/dss-dolphinscheduler/`目录下的lib和db
- 下载 dss-dolphinscheduler-token-1.1.1.jar [点我下载](https://osp-1257653870.cos.ap-guangzhou.myqcloud.com/WeDatasphere/DolphinScheduler/DSS1.1.1_dolphinscheduler/dss-dolphinscheduler-token-1.1.1.jar) 将该jar包放到`${DSS_HOME}/lib/dss-framework/dss-framework-project-server/`目录下
- 下载`dss-dolphinscheduler-client` 插件安装包，可从此处下载：[点我下载插件安装包](https://osp-1257653870.cos.ap-guangzhou.myqcloud.com/WeDatasphere/DolphinScheduler/DSS1.1.1_dolphinscheduler/dss-dolphinscheduler-client.zip) 解压该zip包后用将其中的lib全量替换原有的dss-dolphinscheduler-client安装目录下的lib
- 下载 dolphinscheduler-prod-metrics-with-dependencies.jar [点我下载](https://osp-1257653870.cos.ap-guangzhou.myqcloud.com/WeDatasphere/DolphinScheduler/DSS1.1.1_dolphinscheduler/dolphinscheduler-prod-metrics-1.1.1-jar-with-dependencies.jar) 将该jar包放到dolphinscheduler安装目录的lib目录下，删除之前的 dolphinscheduler-prod-metrics-1.1.0.jar

### 4. 替换前端包
- 用户可自行编译前端包或者从 [DSS&Linkis一键部署文档单机版](DSS&Linkis一键部署文档单机版.md) 来获取DSS1.1.1的全量包并将前端包拿出，替换掉`${DSS_HOME}/web/`下的dist即可
### 5. 服务启动
- 重启DSS服务，在**dss部署目录下**执行命令启动所有服务：

```shell
sh sbin/dss-start-all.sh 
```
- 重启dolphinschduler服务
- 重启nginx



