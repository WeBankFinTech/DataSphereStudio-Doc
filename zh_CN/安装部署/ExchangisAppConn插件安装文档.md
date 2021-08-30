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

DSS1.0.0版本,ExchangisAppConn物料安装可以跳过，但是需要在数据库中插入相应的数据，sql如下
```roomsql
SET @EXCHANGIS_INSTALL_IP_PORT='127.0.0.1:9003';
SET @URL = replace('http://EXCHANGIS_IP_PORT', 'EXCHANGIS_IP_PORT', @EXCHANGIS_INSTALL_IP_PORT);
SET @HOMEPAGE_URL = replace('http://EXCHANGIS_IP_PORT', 'EXCHANGIS_IP_PORT', @EXCHANGIS_INSTALL_IP_PORT);
SET @PROJECT_URL = replace('http://EXCHANGIS_IP_PORT', 'EXCHANGIS_IP_PORT', @EXCHANGIS_INSTALL_IP_PORT);
SET @REDIRECT_URL = replace('http://EXCHANGIS_IP_PORT/udes/auth', 'EXCHANGIS_IP_PORT', @EXCHANGIS_INSTALL_IP_PORT);

delete from `dss_application` WHERE `name` = 'Exchangis';
INSERT INTO `dss_application`(`name`,`url`,`is_user_need_init`,`level`,`user_init_url`,`exists_project_service`,`project_url`,`enhance_json`,`if_iframe`,`homepage_url`,`redirect_url`) VALUES ('Exchangis', @URL, 0, 1, NULL, 0, @PROJECT_URL, '', 1, @HOMEPAGE_URL, @REDIRECT_URL);

select @dss_exchangis_applicationId:=id from `dss_application` WHERE `name` = 'Exchangis';

delete from `dss_stop_menu` WHERE `name` = '数据交换';
INSERT INTO `dss_stop_menu`(`name`, `title_en`, `title_cn`, `description`, `is_active`) values ('数据交换','data exchange','数据交换','数据交换',1);

select @dss_onestop_menu_id:=id from `dss_onestop_menu` where `name` = '数据交换';

delete from `dss_onestop_menu_application` WHERE title_en = 'Exchangis';
INSERT INTO `dss_onestop_menu_application` (`application_id`, `onestop_menu_id`, `title_en`, `title_cn`, `desc_en`, `desc_cn`, `labels_en`, `labels_cn`, `is_active`, `access_button_en`, `access_button_cn`, `manual_button_en`, `manual_button_cn`, `manual_button_url`, `icon`, `order`, `create_by`, `create_time`, `last_update_time`, `last_update_user`, `image`) 
VALUES(@dss_exchangis_applicationId, @dss_onestop_menu_id, 'Exchangis','数据交换','Exchangis is a lightweight, high scalability, data exchange platform, support for structured and unstructured data transmission between heterogeneous data sources','Exchangis是一个轻量级的、高扩展性的数据交换平台，支持对结构化及无结构化的异构数据源之间的数据传输，在应用层上具有数据权限管控、节点服务高可用和多租户资源隔离等业务特性，而在数据层上又具有传输架构多样化、模块插件化和组件低耦合等架构特点。','Data Exchange','数据交换','1','Enter Exchangis','进入Exchangis','user manual','用户手册','http://127.0.0.1:8088/wiki/scriptis/manual/workspace_cn.html','shujujiaohuan-logo',NULL,NULL,NULL,NULL,NULL,'shujujiaohuan-icon');
```



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
