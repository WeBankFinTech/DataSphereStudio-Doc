## 引言

DataSphere Studio（简称 DSS）是微众银行自研的数据应用开发管理集成框架。

在统一的UI下，DataSphere Studio 以工作流式的图形化拖拽开发体验，将满足从数据交换、脱敏清洗、分析挖掘、质量检测、可视化展现、定时调度到数据输出应用等，数据应用开发全流程场景需求。

## 文档列表

* [安装部署文档](安装部署)
    * [DSS 单机部署文档](安装部署/DSS&Linkis一键部署文档单机版.md)
    * [Exchangis AppConn 插件安装文档](https://github.com/WeBankFinTech/Exchangis/blob/master/docs/zh_CN/ch1/exchangis_appconn_deploy_cn.md)
    * [Qualitis AppConn 插件安装文档](https://github.com/WeBankFinTech/Qualitis/blob/master/docs/zh_CN/ch1/%E6%8E%A5%E5%85%A5%E5%B7%A5%E4%BD%9C%E6%B5%81%E6%8C%87%E5%8D%97.md)
    * [Schedulis AppConn 插件安装文档](安装部署/SchedulisAppConn插件安装文档.md)
    * [Visualis AppConn 插件安装文档](https://github.com/WeBankFinTech/Visualis/blob/master/visualis_docs/zh_CN/Visualis_appconn_install_cn.md)
    * [Streamis AppConn 插件安装文档](https://github.com/WeBankFinTech/Streamis/blob/main/docs/zh_CN/0.2.0/development/StreamisAppConn%E5%AE%89%E8%A3%85%E6%96%87%E6%A1%A3.md)
    * [Prophecis AppConn 插件安装文档](https://github.com/WeBankFinTech/Prophecis/blob/master/docs/zh_CN/Deployment_Documents/Prophecis%20Appconn%E5%AE%89%E8%A3%85%E6%96%87%E6%A1%A3.md)
    * [DolphinScheduler AppConn 插件安装文档](安装部署/DolphinScheduler插件安装文档.md)


* [使用文档](使用文档)
    * [产品简介](产品简介)
        * [什么是DataSphere Studio](产品简介/什么是DataSphere%20Studio.md)
        * [基本功能](产品简介/基本功能.md)
        * [使用场景](产品简介/使用场景.md)
    * [快速使用指南](使用文档/快速使用指南)
        * [管理台资源参数配置](使用文档/快速使用指南/管理台资源参数配置.md)
        * [DSS快速入门](使用文档/快速使用指南/DSS快速入门.md)
        * [数据分析快速入门](使用文档/快速使用指南/数据分析快速入门.md)
        * [数据开发快速入门](使用文档/快速使用指南/数据开发快速入门.md)
        * [开发语言使用案例](使用文档/快速使用指南/开发语言使用案例/开发语言使用文档目录.md)
            * [Hive脚本案例](使用文档/快速使用指南/开发语言使用案例/Hive脚本案例.md)
            * [Scala脚本案例](使用文档/快速使用指南/开发语言使用案例/Scala脚本案例.md)
            * [SparkSql脚本案例](使用文档/快速使用指南/开发语言使用案例/SparkSql脚本案例.md)
            * [Python脚本案例](使用文档/快速使用指南/开发语言使用案例/Python脚本案例.md)
            * [PySpark脚本案例](使用文档/快速使用指南/开发语言使用案例/PySpark脚本案例.md)
            * [shell脚本案例](使用文档/快速使用指南/开发语言使用案例/shell脚本案例.md)
            * [Mllib使用案例](使用文档/快速使用指南/开发语言使用案例/Mllib使用案例.md)
            * [Trino脚本案例](使用文档/快速使用指南/开发语言使用案例/Trino脚本案例.md)
    * [产品使用指南](使用文档/产品使用指南)
        * [平台基本概念](使用文档/产品使用指南/DSS平台基本概念.md)
        * 函数与变量管理
            * [UDF函数](使用文档/产品使用指南/UDF函数.md)
            * [方法函数](使用文档/产品使用指南/Scriptis/方法函数.md)
            * [变量管理](使用文档/产品使用指南/Scriptis/变量管理.md)
        * [Scriptis数据探索分析](使用文档/产品使用指南/Scriptis)
            * [Scriptis基本功能](使用文档/产品使用指南/Scriptis/Scriptis基本功能.md)
            * [脚本管理与开发](使用文档/产品使用指南/Scriptis/脚本管理与开发.md)
            * [脚本编辑器功能概述](使用文档/产品使用指南/Scriptis/脚本编辑器功能概述.md)
            * [数据库/表管理](使用文档/产品使用指南/Scriptis/数据库_表管理.md)
            * [hdfs文件管理](使用文档/产品使用指南/Scriptis/hdfs文件管理.md)
            * [udf函数管理](使用文档/产品使用指南/Scriptis/udf函数管理.md)
            * [方法函数管理](使用文档/产品使用指南/Scriptis/方法函数.md)
            * [变量管理](使用文档/产品使用指南/Scriptis/变量管理.md)
            * [数据导入](使用文档/产品使用指南/Scriptis/数据导入.md)
            * [数据导出](使用文档/产品使用指南/Scriptis/数据导出.md)
            * [进阶使用案例](使用文档/产品使用指南/Scriptis/show_matplotlib使用指引.md)
            * [数据服务发布](使用文档/产品使用指南/Scriptis/DataApiService使用文档.md)
        * [工作流任务开发](使用文档/产品使用指南/workflow)
            * [工作流开发流程](使用文档/产品使用指南/workflow/工作流开发操作步骤.md)
            * [工作流支持节点类型](使用文档/产品使用指南/workflow/工作流支持节点类型.md)
            * [项目管理](使用文档/产品使用指南/workflow/项目管理.md)
            * [工作流基本管理](使用文档/产品使用指南/workflow/工作流基本管理.md)
            * [工作流开发](使用文档/产品使用指南/workflow/工作流开发.md)
            * [工作流协同开发](使用文档/产品使用指南/workflow/工作流协同开发.md)
            * [工作流导入与导出](使用文档/产品使用指南/workflow/工作流导入与导出.md)
            * [工作流调度中心](使用文档/产品使用指南/workflow/工作流调度中心.md)
        * 数据服务
            * [API发布](使用文档/产品使用指南/Scriptis/DataApiService使用文档.md)
    * DSS 用户手册汇总版 
        * [DSS 接口汇总](使用文档/DSS接口汇总.md)
        * [DSS 用户登录认证体系](使用文档/用户登录认证体系.md)
        * [DSS 工作空间使用手册](使用文档/工作空间使用手册.md)
        * [DSS 如何新增用户](用户手册/DSS新增用户方式.md)
        * [超级管理员使用介绍](用户手册/超级管理员功能.md)
        * [Linkis 管理台使用介绍](https://linkis.apache.org/zh-CN/docs/latest/user-guide/control-panel/overview)
        * [调度中心使用介绍](用户手册/调度中心使用文档.md)


* [开发文档](开发文档)
    * [DSS 后端编译文档](开发文档/DSS编译文档.md)
    * [DSS 前端编译文档](开发文档/前端编译文档.md)
    * [第三方系统接入 DSS 开发指南](开发文档/第三方系统接入DSS开发指南.md)
    * [AppConn 开发指南](开发文档/AppConn开发指南.md)
    * [DSS 新增工作流节点](开发文档/DSS工作流如何新增工作流节点.md)
    * [Scripits 新增脚本类型](开发文档/Scriptis如何新增脚本类型.md)


* [设计文档](设计文档)
  * [AppConn 设计文档](设计文档/appconn/appconn.md)
  * [DSS Workspace 设计文档](设计文档/Workspace/README.md)
  * [DSS Project 设计文档](设计文档/project/DSS工程模块设计文档.md)
  * [DSS Orchestrator 设计文档](设计文档/Orchestrator/README.md)
  * [DSS 工作流微模块设计文档](设计文档/workflow/DSS工作流架构设计.md)
  * [DSS 工作流执行设计文档](设计文档/FlowExecution/README.md)
  * [DSS 工作流发布到调度系统设计文档](设计文档/publish/工作流发布设计文档.md)
    
更多文档持续更新中，敬请期待……