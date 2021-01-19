![DSS](https://github.com/WeBankFinTech/DataSphereStudio/images/en_US/readme/DSS_logo.png)
====

[![License](https://img.shields.io/badge/license-Apache%202-4EB1BA.svg)](https://www.apache.org/licenses/LICENSE-2.0.html)

[English](README.md) | 中文


## 引言

&nbsp; &nbsp; &nbsp; &nbsp;DataSphere Studio（简称DSS）是微众银行自研的一站式数据应用开发管理框架。

&nbsp; &nbsp; &nbsp; &nbsp;在统一的UI下，DataSphere Studio以工作流式的图形化拖拽开发体验，将满足从数据交换、脱敏清洗、分析挖掘、质量检测、可视化展现、定时调度到数据输出应用等，数据应用开发全流程场景需求。

&nbsp; &nbsp; &nbsp; &nbsp;**DSS通过插拔式的集成框架设计，让用户可以根据需要，简单快速替换DSS已集成的各种功能组件，或新增功能组件。**


## Demo试用环境

&nbsp; &nbsp; &nbsp; &nbsp;由于DataSphereStudio支持执行脚本风险较高，WeDataSphere Demo环境的隔离没有做完，考虑到大家都在咨询Demo环境，决定向社区先定向发放邀请码，接受企业和组织的试用申请。

&nbsp; &nbsp; &nbsp; &nbsp;如果您想试用Demo环境，请加入DataSphere Studio社区用户群（**加群方式请翻到本文档末尾处**），联系团队成员获取邀请码。

&nbsp; &nbsp; &nbsp; &nbsp;WeDataSphere Demo环境用户注册页面：https://sandbox.webank.com/wds/dss/#/register

&nbsp; &nbsp; &nbsp; &nbsp;WeDataSphere Demo环境登录页面：https://sandbox.webank.com/wds/dss/

&nbsp; &nbsp; &nbsp; &nbsp;我们会尽快解决环境隔离问题，争取早日向社区完全开放WeDataSphere Demo环境。


## 文档目录

[部署文档]()

[用户手册]()

[常见问题]()

[架构文档]()

[API文档]()

[进阶文档]()

[版本升级]()


## 已集成应用工具

| Application     | Description                                                          | Contributor                                                           | Version    | 是否已发版? |
| --------------- | -------------------------------------------------------------------- | --------------------------------------------------------------------- | ---------- | ------ |
| **ApiService**  | 数据API服务。可快速将脚本发布为一个Restful接口，提供访问能力                                  | WeDataSphere & [brianzhangrong](https://github.com/brianzhangrong)(Linkis Committer) | DSS-1.0.0-RC1 | 否，预计年前 |
| **Airflow**     | 支持将DSS工作流发布到Airflow进行定时调度                                            | [sparklezzz](https://github.com/sparklezzz)                           | DSS-0.10.0 | 否，预计本月 |
| **DataStream**  | 流式应用平台。支持发布Flink Jar 和 Flink SQL ，提供流式应用的管理能力，如：启停、状态监控、checkpoint等。 | [hui zhu](https://github.com/liangqilang)(DSS Committer)           | DSS-0.13.0 | 否，预计2月底 |
| **UserManager** | 自动初始化一个DSS新用户所必须的所有用户环境，包含：创建Linux用户、各种用户路径、目录授权等                    | [Adam Wang](https://github.com/Adamyuanyuan)(DSS Committer)        | DSS-0.12.0 | 否，预计下月 |
| **EventCheck**  | 提供跨业务、跨工程和跨工作流的信号发送能力。 | WeDataSphere | DSS-0.9.3 | 是      |
| **SendEmail**   | 提供数据发送能力，所有其他节点的结果集，都可以通过邮件发送 | WeDataSphere | DSS-0.9.0 | 是      |
| [**Scriptis**](https://github.com/WeBankFinTech/Scriptis)   | 支持在线写SQL、Pyspark、HiveQL等脚本，提交给[Linkis](https://github.com/WeBankFinTech/Linkis)执行的数据分析Web工具。 | WeDataSphere | DSS-0.5.0 | 是      |
| [**Visualis**](https://github.com/WeBankFinTech/Visualis)   | 基于宜信开源项目Davinci二次开发的数据可视化BI工具，为用户在数据安全方面提供金融级数据可视化能力。 | WeDataSphere | DSS-0.5.0 | 是      |
| [**Qualitis**](https://github.com/WeBankFinTech/Qualitis)   | 数据质量校验工具，提供数据完整性、正确性等数据校验能力 | WeDataSphere | DSS-0.5.0 | 是      |
| [**Schedulis**](https://github.com/WeBankFinTech/Schedulis) | 基于Azkaban二次开发的工作流任务调度系统,具备高性能，高可用和多租户资源隔离等金融级特性。 | WeDataSphere | DSS-0.5.0 | 是      |


## 谁在使用DataSphere Studio


## 文档说明

&nbsp; &nbsp; &nbsp; &nbsp;DataSphere Studio统一使用Gitbook进行管理，整个项目会整理成一个Gitbook电子书，方便大家下载和使用。

&nbsp; &nbsp; &nbsp; &nbsp;WeDataSphere后续会提供统一的文档阅读入口，关于Gitbook的使用方式，请参考：[GitBook文档](http://caibaojian.com/gitbook/)。


## 文档贡献

&nbsp; &nbsp; &nbsp; &nbsp;贡献文档前，请先阅读[文档贡献规范]()，了解如何贡献代码，并提交您的贡献。


## 社区交流

![交流](https://github.com/WeBankFinTech/DataSphereStudio/images/zh_CN/readme/communication.png)


## License

&nbsp; &nbsp; &nbsp; &nbsp;DSS is under the Apache 2.0 license. See the [License](LICENSE) file for details.
