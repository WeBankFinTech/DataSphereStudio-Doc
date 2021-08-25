# VisualisAppConn安装文档

本文主要介绍在DSS(DataSphere Studio)1.0中VisualisAppConn的部署、配置以及使用。



## 1.部署VisualisAppConn的准备工作

您在部署VisualisAppConn之前，必须要先将Visualis部署启动并保证基本功能可用。



## 2.VisualisAppConn物料的编译

我们提供了VisualisAppConn的物料包，如果您已经下载了，您可以跳过此步骤。如果您想自己编译VisualisAppConn，具体编译步骤如下:
1. clone DataSphere Studio的代码
2. 单独编译visualis-appconn
```bash 
cd {DSS_CODE_HOME}/dss-appconn/appconns/dss-visualis-appconn
mvn clean install
```

## 3.VisualisAppConn物料的部署安装


1. 从步骤2中的target目录获取visualis-appconn.zip物料包
2. 放置到如下目录并进行解压
```
{DSS_HOME}/dss/dss-appconns
```



## 4.VisualisAppConn的配置

1. Visualis集成进入dss需要在以下数据库表中设置相应的内容

| 表名      | 表作用   | 备注                                   |
|-----------------|----------------|----------------------------------------|
| dss_application       | 应用表,主要是插入visualis应用的基本信息    | 必须                                   |
| dss_menu     | 菜单表，存储对外展示的内容，如图标、名称等 | 必须                                   |
| dss_onestop_menu_application | menu和application的关联表，用于联合查找 |                    必须                |
| dss_appconn      | appconn的基本信息，用于加载appconn  | 必须                                   |
| dss_appconn_instance  | visualis实例的信息，包括visualis自身的url信息   | 必须         |
| dss_workflow_node  | Visualis作为工作流节点需要插入的信息   | 如果想使用可视化节点，则必须         |


2. 表配置方式
``` bash 
cd {DSS_INSTALL_HOME}/dss/bin
sh install-appconn.sh
```
脚本是交互式的方案，您需要按照指示输入appconn的名称(visualis), visualis的ip和端口，输入完毕之后，就可以完成自动安装

## 5.使用VisualisAppConn的微服务

DSS中有以下的微服务会通过VisualisAppConn与Visualis进行交互，完成指定的功能。

| 微服务名      | 使用AppConn完成的功能   | 备注                                   |
|-----------------|----------------|----------------------------------------|
| dss-framework-project-server       | 使用visualis-appconn完成工程以及组织的统一    | 必须                                   |
| dss-workflow-server     | 使用第三级规范完成节点的创建、编辑和导入导出等| 必须                                   |
| appconn-engine | 使用第三级规范，完成visualis节点的创建 |                    必须                |
为了保证所有的微服务都能加载到此插件，我们需要重启下dss服务，具体方式是
```bash 
cd {DSS_INSTALL_HOME}/dss/bin
sh stop-all.sh
sh start-all.sh
```



## 5.使用方式
您可以进入DSS的前端首页,然后进入Visualis应用的首页,如图

![Visualis嵌入DSS](../Images/安装部署/VisualisAppConn部署/DSS-Visualis.png)

您也可以通过使用工作流的可视化节点来使用Visualis的可视化能力，如图

![工作流使用可视化节点](../Images/安装部署/VisualisAppConn部署/Workflow-Visualis.png)
