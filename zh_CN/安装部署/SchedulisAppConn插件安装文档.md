# Schedulis AppConn 插件安装

> Schedulis 通过在 DSS 中安装配置 AppConn 插件，可以实现DSS开发的工作流发布到 Schedulis 定时调度执行。

### 一、安装前准备

- 需先安装DSS，可参考: [DSS&Linkis单机一键部署文档](DSS&Linkis单机一键部署文档.md)

- 需先安装并运行 Schedulis，可参考: [Schedulis部署文档](https://github.com/WeBankFinTech/Schedulis/blob/master/docs/schedulis_deploy_cn.md)

### 二、安装 Schedulis AppConn

- 安装Schedulis时，通过执行以下脚本，只需输入 Schedulis 部署机器的 IP，和 Port，就能配置完成对应的 AppConn 插件的安装，在执行脚本时，会执行对应 AppConn 下 init.sql 脚本，把对应的数据库信息插入到 DSS 表中
```
## 切换目录到DSS的安装目录
cd xx/dss

## 执行 appconn-install 安装脚本，输入对应的appconn名称，按照提示输入对应schedulis exec服务对应的IP和PORT
sh bin/appconn-install.sh
>> schedulis
>> 127.0.0.1
>> 8085
```


### 三、Schedulis JobType 插件安装

- 用户还需为 Schedulis 安装一个 JobType 插件： linkis-jobtype，请点击[Linkis JobType安装文档](Schedulis_Linkis_JobType安装文档.md)。

### 四、用户 token 配置

- 请在 DSS-SERVER 服务 conf 目录的 token.properties 文件中，配置用户名和密码信息，关联 DSS 和 Schedulis 用户，因为用户通过 DSS 创建工程后，要发布到 Schedulis，用户必须保持一致。示例：

```shell script
cd ${DSS_HOME}/conf
vim token.properties
```



```
## token.properties 添加用户
 用户名=密码
```

- 说明：由于每个公司都有各自的登录认证系统，这里只提供简单实现，用户可以实现 ```SchedulerSecurityService``` 定义自己的登录认证方法。

- Schedulis 用户管理可参考：[Azkaban-3.x 用户管理](https://cloud.tencent.com/developer/article/1492734) 或 [官网](https://azkaban.readthedocs.io/en/latest/userManager.html)

### 五、Schedulis 使用方式

- Schedulis 服务部署完成和 Schedulis AppConn 安装完成后，即可在 DSS 中使用 Schedulis。可以在应用组件中，点击 Schedulis 进入 Schedulis，在工作流开发时，可以一键发布 DSS 工作流到 Schedulis 进行调度执行。

### 六、Schedulis AppConn 安装原理

- Schedulis 的相关配置信息会插入到以下表中，通过配置下表，可以完成 Schedulis 的使用配置，安装 Schedulis AppConn 时，脚本会替换每个 AppConn 下的 init.sql，并插入到表中。

| 表名      | 表作用   | 备注                                   |
|-----------------|----------------|----------------------------------------|
| dss_application       | 应用表,主要是插入 schedulis 应用的基本信息 | 必须                                   |
| dss_menu     | 菜单表，存储对外展示的内容，如图标、名称等 | 必须                                   |
| dss_onestop_menu_application | menu 和 application 的关联表，用于联合查找 |                    必须                |
| dss_appconn      | appconn 的基本信息，用于加载 appconn  | 必须                                   |
| dss_appconn_instance  | AppConn 的实例的信息，包括自身的url信息 | 必须         |
| dss_workflow_node  | schedulis 作为工作流节点需要插入的信息 | 如果使用可视化节点，则必须         |

- Schedulis 作为调度框架，实现了一级规范和二级规范，需要使用 Schedulis AppConn 的微服务如下表。

| 微服务名      | 使用AppConn完成的功能   | 备注                                   |
|-----------------|----------------|----------------------------------------|
| dss-framework-project-server       | 使用 schedulis-appconn 完成工程以及组织的统一    | 必须                                   |
| dss-workflow-server       | 借用调度 AppConn 实现工作流发布，状态等获取    | 必须                                   |
