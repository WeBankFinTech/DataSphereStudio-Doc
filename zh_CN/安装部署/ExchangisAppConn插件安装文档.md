# ExchangisAppConn安装文档

本文主要介绍在DSS(DataSphere Studio)1.0中ExchangisAppConn的部署、配置以及使用。



## 1.部署ExchangisAppConn的准备工作

您在部署ExchangisAppConn之前，必须要先将Exchangis部署启动并保证基本功能可用。并且Exchangis需要引入以下的maven依赖，这个maven依赖可以实现统一的SSO能力
```xml
<dependency>
    <groupId>com.webank.wedatasphere.dss</groupId>
    <artifactId>spring-origin-sso-integration-plugin</artifactId>
    <version>${dss.version}</version>
</dependency>
```
在DSS1.0.0版本中，Exchangis可以做到DSS第一级规范中的SSO规范，在首页能够免密登录到Exchangis。


## 2.ExchangisAppConn物料的编译

DSS1.0.0版本，此步骤可以跳过，ExchangisAppConn使用的是默认SSO实现

## 3.ExchangisAppConn物料的部署安装

DSS1.0.0版本，此步骤可以跳过



## 4.ExchangisAppConn的使用
您可以进入DSS的前端首页,然后进入Exchangis应用的首页,如图

![Exchangis嵌入DSS](../Images/安装部署/ExchangisAppConn部署/DSS-Exchangis.png)


## 5.ExchangisAppConn插件的工作原理

1. 使用ExchangisAppConn的微服务

DSS中有以下的微服务会通过ExchangisAppConn与Exchangis进行交互，完成指定的功能。

| 微服务名      | 使用AppConn完成的功能   | 备注                                   |
|-----------------|----------------|----------------------------------------|
| dss-framework-project-server       | 统一鉴权，实现SSO   | 必须                                   |




2. Exchangis集成进入dss需要在以下数据库表中设置相应的内容

| 表名      | 表作用   | 备注                                   |
|-----------------|----------------|----------------------------------------|
| dss_application       | 应用表,主要是插入Exchangis应用的基本信息    | 必须                                   |
| dss_menu     | 菜单表，存储对外展示的内容，如图标、名称等 | 必须                                   |
| dss_onestop_menu_application | menu和application的关联表，用于联合查找 |                    必须                |



3.后续开发项

3.1 ExchangisAppConn支持一级规范中的工程规范

3.2 ExchangisAppConn支持第三级规范中的执行规范，支持执行Exchangis类型的任务

3.3 ExchangisAppConn支持第三级规范中的导入导出规范
