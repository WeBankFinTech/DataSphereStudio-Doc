# DolphinSchedulerAppConn 安装文档


## 1. 准备工作

您在部署 `DolphinSchedulerAppConn` 之前，必须先将 `DolphinScheduler` 部署启动并保证 `DolphinScheduler` 可用。

关于 `DolphinSchedulerAppConn` 的安装部署，请参考：[`DolphinSchedulerAppConn` 的安装部署文档](https://dolphinscheduler.apache.org/zh-cn/docs/latest/user_doc/guide/installation/standalone.html)

## 2. 下载和编译

`DolphinSchedulerAppConn` 插件安装包，可从此处下载：[点我下载`DolphinSchedulerAppConn`插件安装包]()。

如果您想自己编译 `DolphinSchedulerAppConn`，具体编译步骤如下:

1. clone DataSphereStudio 的代码
2. 单独编译 dolphinscheduler-appconn

```shell script 
cd ${DSS_HOME}/dss-appconn/appconns/dss-dolphinscheduler-appconn
mvn clean install
```

## 3. 配置和部署

- 将 `dolphinscheduler-appconn.zip` 插件安装包，放置到如下目录并进行解压

```shell script 
cd ${DSS_HOME}/dss/dss-appconns
unzip dolphinscheduler-appconn.zip
```

- 配置参数

请按需修改 `appconn.properties` 的配置参数

```shell script
cd ${DSS_HOME}/dss/dss-appconns/dolphinscheduler
vim appconn.properties
```

```properties
# 【必填】指定 DolphinScheduler 的管理员用户
wds.dss.appconn.ds.admin.user=admin
# 【必填】指定 DolphinScheduler 管理员用户的 token
wds.dss.appconn.ds.admin.token=

# 【请参考】目前只适配了 DolphinScheduler 1.3.X.
wds.dss.appconn.ds.version=1.3.9

# 用于配置 dss-dolphinscheduler-client 的 home 路径，可为具体路径，具体请参考第 4 大步骤
wds.dss.appconn.ds.client.home=${DSS_DOLPHINSCHEDULER_CLIENT_HOME}

# this property is used to add url prefix, if you add a proxy for dolphinscheduler url.
# for example: the normal dolphinscheduler url is http://ip:port/users/create, if you set
# this property, the real url will be http://ip:port/${wds.dss.appconn.ds.url.prefix}/users/create
#wds.dss.appconn.ds.url.prefix=
```

- 加载 `DolphinScheduler` 插件

``` bash 
cd ${DSS_HOME}/bin
sh install-appconn.sh
# 该脚本为交互式的安装方案，您只需要按照指示，输入字符串 dolphinscheduler 以及 服务的 ip 和端口，即可以完成安装
```

## 4. 部署 dss-dolphinscheduler-client

要想 `DolphinScheduler` 能正常调度起 DataSphereStudio 的工作流节点，您还需安装 dss-dolphinscheduler-client 插件，该插件用于执行 DSS 工作流节点作业。

### 4.1 安装包准备

`dss-dolphinscheduler-client` 插件安装包，可从此处下载：[点我下载`dss-dolphinscheduler-client`插件安装包]()。

如果您想自己编译 `dss-dolphinscheduler-client`，具体编译步骤如下:

1. clone DataSphereStudio 的代码
2. 单独编译 dss-dolphinscheduler-client

```shell script 
cd ${DSS_HOME}/plugins/dolphinscheduler/dss-dolphinscheduler-client
mvn clean install
```

### 4.2 安装部署

请先在 `/home/${USER}/.bash_rc` 配置环境变量 `DSS_DOLPHINSCHEDULER_CLIENT_HOME`（如果您在 `appconn.properties` 中指定的是绝对路径而不是该环境变量，则该环境变量也可以不用配置）。

将 `DSS_DOLPHINSCHEDULER_CLIENT_HOME` 配置为实际的 `dss-dolphinscheduler-client` 根路径。

在 `dss-dolphinscheduler-client` 的根路径进行解压安装，如下：

```shell script 
cd ${DSS_DOLPHINSCHEDULER_CLIENT_HOME}
unzip dss-dolphinscheduler-client.zip
```

解压即可完成 `dss-dolphinscheduler-client` 的安装。

## 4. DolphinSchedulerAppConn 的使用

### 4.1 免密跳转

进入 DSS 的工作空间首页，然后在顶部菜单栏点击跳转到 DolphinScheduler

![DolphinScheduler免密跳转]()

### 4.2 发布 DSS 工作流到 DolphinScheduler

点击工作流的发布按钮，可将 DSS 工作流一键发布到 DolphinScheduler

![DSS工作流一键发布到DolphinScheduler]()