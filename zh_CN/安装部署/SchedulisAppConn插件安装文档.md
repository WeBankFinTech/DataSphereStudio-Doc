# Schedulis AppConn安装说明
> Schedulis是微众银行大数据平台室开源的工作流调度系统，通过在DSS中安装配置AppConn插件，可以实现DSS开发的工作流发布到Schedulis定时调度执行。

# 1. 安装前准备
&nbsp;&nbsp;&nbsp;&nbsp;1. 首先需要安装DSS，通过一键安装部署脚本或单独安装，可以参考DSS安装文档。  
&nbsp;&nbsp;&nbsp;&nbsp;2. 安装完DSS后，可以实现基本的DSS交互式分析和工作流执行任务，如果需要DSS工作流发布到Schedulis调度执行，首先需要安装并运行[Schedulis](https://github.com/WeBankFinTech/Schedulis)。
# 2. 单独编译AppConn插件包
&nbsp;&nbsp;&nbsp;&nbsp;在执行DSS编译打包部署后，同时会编译相关AppConn包，如果想单独编译Schedulis AppConn，需参照以下步骤:
```bash 
cd {DSS_SOURCE_CODE_PROJECT}/dss-appconn/appconns/dss-schedulis-appconn
mvn clean install
```
# 3. 安装Schedulis AppConn
&nbsp;&nbsp;&nbsp;&nbsp;在DSS的安装目录下，appconns目录中已经有对接到DSS的第三方应用工具的AppConn jar包，包括本文档中介绍的Schedulis AppConn的jar包。  
&nbsp;&nbsp;&nbsp;&nbsp;安装Schedulis时，通过执行以下脚本，只需简单的输入Schedulis部署机器的IP，和服务Port。就能配置完成对应的AppConn插件的安装，在执行脚本时，会执行对应AppConn下init.sql脚本，把对应的数据库信息插入到DSS表中。
```sh
sh ${DSS_HOME}bin/appconn-install.sh

# 执行appcon-install安装脚本后，输入对应的appconn名称
# 按照提示输入对应schedulis服务对应的IP，和PORT
>> schedulis
>> 127.0.0.1
>> 8089
```
# 4. Schedulis AppConn使用原理
&nbsp;&nbsp;&nbsp;&nbsp;Schedulis的相关配置信息会插入到以下表中，通过配置下表，可以完成Schedulis的使用配置，安装Schedulis AppConn时，脚本会替换每个AppConn下的init.sql，并插入到表中。
| 表名      | 表作用   | 备注                                   |
|-----------------|----------------|----------------------------------------|
| dss_application       | 应用表,主要是插入schedulis应用的基本信息 | 必须                                   |
| dss_menu     | 菜单表，存储对外展示的内容，如图标、名称等 | 必须                                   |
| dss_onestop_menu_application | menu和application的关联表，用于联合查找 |                    必须                |
| dss_appconn      | appconn的基本信息，用于加载appconn  | 必须                                   |
| dss_appconn_instance  | AppConn的实例的信息，包括自身的url信息 | 必须         |
| dss_workflow_node  | qualitis作为工作流节点需要插入的信息 | 如果使用可视化节点，则必须         |

&nbsp;&nbsp;&nbsp;&nbsp;Schedulis作为调度框架，实现了一级规范和二级规范，需要使用Schedulis AppConn的微服务如下表。

| 微服务名      | 使用AppConn完成的功能   | 备注                                   |
|-----------------|----------------|----------------------------------------|
| dss-framework-project-server       | 使用schedulis-appconn完成工程以及组织的统一    | 必须                                   |
|-----------------|----------------|----------------------------------------|
| dss-workflow-server       | 借用调度AppConn实现工作流发布，状态等获取    | 必须                                   |
# 5. Schedulis使用方式
&nbsp;&nbsp;&nbsp;&nbsp;Schedulis服务部署完成，Schedulis AppConn安装完成后，即可在DSS中使用Schedulis。可以在应用商店中，点击Schedulis进入Schedulis，在工作流开发时，可以一键发布DSS工作流到Schedulis进行调度执行。