![DSS](https://github.com/WeBankFinTech/DataSphereStudio/images/en_US/readme/DSS_logo.png)
====

[![License](https://img.shields.io/badge/license-Apache%202-4EB1BA.svg)](https://www.apache.org/licenses/LICENSE-2.0.html)

[English](README.md) | 中文


## 引言

&nbsp; &nbsp; &nbsp; &nbsp;DataSphere Studio（简称DSS）是微众银行自研的一站式数据应用开发管理框架。

&nbsp; &nbsp; &nbsp; &nbsp;在统一的UI下，DataSphere Studio以工作流式的图形化拖拽开发体验，将满足从数据交换、脱敏清洗、分析挖掘、质量检测、可视化展现、定时调度到数据输出应用等，数据应用开发全流程场景需求。

&nbsp; &nbsp; &nbsp; &nbsp;**DSS通过插拔式的集成框架设计，让用户可以根据需要，简单快速替换DSS已集成的各种功能组件，或新增功能组件。**

## 已集成应用工具

| 应用工具     | 描述                                                          | DSS0.X 版本要求                                                           | DSS1.0 版本要求    | 版本规划 |
| --------------- | -------------------------------------------------------------------- | --------------------------------------------------------------------- | ---------- | ------ |
| **ApiService**  | 数据API服务。可快速将脚本发布为一个Restful接口，提供访问能力                                  | 不支持 | >=1.0.0 | 已发布 |
| **Airflow**     | 支持将DSS工作流发布到Airflow进行定时调度                                            | >=0.9.1，尚未合并 | on going | 待规划 |
| **Streamis**  | 流式应用平台。支持发布Flink Jar 和 Flink SQL ，提供流式应用的开发调试和生产管理能力，如：启停、状态监控、checkpoint等。 | 不支持 | >=1.0.0 | 即将发布 |
| **UserManager** | 自动初始化一个DSS新用户所必须的所有用户环境，包含：创建Linux用户、各种用户路径、目录授权等                    |  >=0.9.1 | on going | 待规划 |
| **EventCheck**  | 提供跨业务、跨工程和跨工作流的信号发送能力。 | >=0.5.0 | >=1.0.0 | 已发布      |
| **SendEmail**   | 提供数据发送能力，所有其他节点的结果集，都可以通过邮件发送 | >=0.5.0 | >=1.0.0 | 已发布  |
| [**Scriptis**](https://github.com/WeBankFinTech/Scriptis)   | 支持在线写SQL、Pyspark、HiveQL等脚本，提交给[Linkis](https://github.com/WeBankFinTech/Linkis)执行的数据分析Web工具。 | >=0.5.0 | >=1.0.0 | 已发布      |
| [**Visualis**](https://github.com/WeBankFinTech/Visualis)   | 基于宜信开源项目Davinci二次开发的数据可视化BI工具，为用户在数据安全方面提供金融级数据可视化能力。 | >=0.5.0 | >=1.0.0 | 已发布      |
| [**Qualitis**](https://github.com/WeBankFinTech/Qualitis)   | 数据质量校验工具，提供数据完整性、正确性等数据校验能力 | >=0.5.0 | >=1.0.0 |  待发布      |
| [**Schedulis**](https://github.com/WeBankFinTech/Schedulis) | 基于Azkaban二次开发的工作流任务调度系统,具备高性能，高可用和多租户资源隔离等金融级特性。 | >=0.5.0 | >=1.0.0 | 已发布      |
| [**Exchangis**](https://github.com/WeBankFinTech/Exchangis) | 支持对结构化及无结构化的异构数据源之间的数据传输的数据交换平台 | 不支持 | >=1.0.0 | 待发布      |


## 下载

 &nbsp; &nbsp; &nbsp; &nbsp;请前往 [DSS releases](https://github.com/WeBankFinTech/DataSphereStudio/releases) 页面下载 DSS 的已编译版本或源码包。

## 编译和安装部署

请参照 [编译指引](zh_CN/开发文档/DSS编译文档.md) 来编译 DSS 源码。

请参考 [安装部署文档](zh_CN/安装部署/DSS单机部署文档.md) 来部署 DSS。

## Demo试用环境

&nbsp; &nbsp; &nbsp; &nbsp;由于 DataSphereStudio 支持执行脚本风险较高，WeDataSphere Demo环境的隔离没有做完，考虑到大家都在咨询Demo环境，决定向社区先定向发放邀请码，接受企业和组织的试用申请。

&nbsp; &nbsp; &nbsp; &nbsp;如果您想试用Demo环境，请加入DataSphere Studio社区用户群（**加群方式请翻到本文档末尾处**），联系团队成员获取邀请码。

&nbsp; &nbsp; &nbsp; &nbsp;DataSphereStudio Demo环境用户注册页面：[点我进入](https://www.ozone.space/wds/dss/#/register)

&nbsp; &nbsp; &nbsp; &nbsp;DataSphereStudio Demo环境登录页面：[点我进入](https://www.ozone.space/wds/dss/#/login)

&nbsp; &nbsp; &nbsp; &nbsp;**DataSphereStudio1.0 Demo环境将在近期开放，敬请期待**。

## 文档目录

请访问 [中文文档](zh_CN)， 查看DSS的完整文档列表。

以下为 DSS 相关 AppConn 插件的安装指南：

- [DSS的Visualis AppConn插件安装指南](zh_CN/安装部署/VisualisAppConn插件安装文档.md)

- [DSS的Schedulis AppConn插件安装指南](zh_CN/安装部署/SchedulisAppConn插件安装文档.md)

- [DSS的Qualitis AppConn插件安装指南](zh_CN/安装部署/QualitisAppConn插件安装文档.md)

- [DSS的Exchangis AppConn插件安装指南](zh_CN/安装部署/ExchangisAppConn插件安装文档.md)


## 谁在使用DataSphere Studio

&nbsp; &nbsp; &nbsp; &nbsp;我们创建了 [Who is using DSS](https://github.com/WeBankFinTech/DataSphereStudio/issues/1) issue 以便用户反馈和记录谁在使用 DSS，欢迎您注册登记.

&nbsp; &nbsp; &nbsp; &nbsp;DSS 自2019年开源发布以来，累计已有700多家试验企业和1000+沙盒试验用户，涉及金融、电信、制造、互联网等多个行业。


## 文档说明

&nbsp; &nbsp; &nbsp; &nbsp;DataSphere Studio统一使用Gitbook进行管理，整个项目会整理成一个Gitbook电子书，方便大家下载和使用。

&nbsp; &nbsp; &nbsp; &nbsp;WeDataSphere后续会提供统一的文档阅读入口，关于Gitbook的使用方式，请参考：[GitBook文档](http://caibaojian.com/gitbook/)。


## 贡献

&nbsp; &nbsp; &nbsp; &nbsp;我们非常欢迎和期待更多的贡献者参与共建 DSS, 不论是代码、文档，或是其他能够帮助到社区的贡献形式。


## 联系我们

对 DSS 的任何问题和建议，敬请提交issue，以便跟踪处理和经验沉淀共享。

您也可以扫描下面的二维码，加入我们的微信/QQ群，以获得更快速的响应。

![交流](https://github.com/WeBankFinTech/DataSphereStudio/images/zh_CN/readme/communication.png)


## License

&nbsp; &nbsp; &nbsp; &nbsp;DSS is under the Apache 2.0 license. See the [License](https://github.com/WeBankFinTech/DataSphereStudio/LICENSE) file for details.
