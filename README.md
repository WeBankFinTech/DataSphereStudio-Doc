DataSphereStudio
====

[![License](https://img.shields.io/badge/license-Apache%202-4EB1BA.svg)](https://www.apache.org/licenses/LICENSE-2.0.html)

English | [中文](README-ZH.md)


## Introduce

&nbsp; &nbsp; &nbsp; &nbsp;DataSphere Studio (DSS for short) is a one-stop data application development and management framework developed by WeBank.

&nbsp; &nbsp; &nbsp; &nbsp;Under a unified UI, DataSphere Studio provides a workflow-based graphical drag-and-drop development experience, which will satisfy the requirements from data exchange, desensitization cleaning, analysis and mining, quality inspection, visual display, timing scheduling to Data output applications, etc., data application development full-process scenario requirements.

&nbsp; &nbsp; &nbsp; &nbsp;**DSS is designed with a pluggable integration framework, allowing users to easily and quickly replace various functional components that DSS has integrated, or add new functional components according to their needs. **
## Integrated application tools

&nbsp; &nbsp; &nbsp; &nbsp;DSS has integrated a variety of upper-layer data application systems by implementing multiple AppConns, which can basically meet the data development needs of users.

 &nbsp; &nbsp; &nbsp; &nbsp;**If desired, new data application systems can also be easily integrated to replace or enrich DSS's data application development process.** [Click me to learn how to quickly integrate new application systems](en_US/Development_Documentation/Third-party_System_Access_Development_Guide.md)

|Utility | Description | DSS0.X compatible version (DSS0.9.1 recommended) | DSS1.0 compatible version (DSS1.1.0 recommended) |
| --------------- | -------------------------------------------------------------------- | --------- | ---------- |
| [**Linkis**](https://github.com/apache/incubator-linkis) | Computing middleware Apache Linkis, by providing standard interfaces such as REST/WebSocket/JDBC/SDK, upper-layer applications can easily connect and access underlying engines such as MySQL/Spark/Hive/Presto/Flink. | Linkis0.11.0 is recommended (**Released* *) | >= Linkis1.1.1 (**Released**) |
| [**DataApiService**](https://github.com/WeBankFinTech/DataSphereStudio-Doc/blob/main/en_US/Using_Document/DataApiService_Usage_Documentation.md)  | (DSS has built-in third-party application tools) data API service. The SQL script can be quickly published as a Restful interface, providing Rest access capability to the outside world. | Not supported | DSS1.1.0 recommended (**Released**)|
| [**Scriptis**](https://github.com/WeBankFinTech/DataSphereStudio) | (DSS has built-in third-party application tools) support online writing of SQL, Pyspark, HiveQL and other scripts, and submit to [Linkis](https ://github.com/WeBankFinTech/Linkis) data analysis web tool. | Recommended DSS0.9.1 (**Released**) | Recommended DSS1.1.0 (**Released**) |
| [**Schedulis**](https://github.com/WeBankFinTech/Schedulis) | Workflow task scheduling system based on Azkaban secondary development, with financial-grade features such as high performance, high availability and multi-tenant resource isolation. | Recommended Schedulis0.6.1 (**released**) | >= Schedulis0.7.0 (**Released**) |
| **EventCheck** | (a third-party application tool built into DSS) provides signal communication capabilities across business, engineering, and workflow. | Recommended DSS0.9.1 (**Released**) | Recommended DSS1.1.0 (**Released**) |
| **SendEmail** | (DSS has built-in third-party application tools) provides the ability to send data, all the result sets of other workflow nodes can be sent by email | DSS0.9.1 is recommended (**released**) | Recommended DSS1.1.0 (**Released**) |
| [**Qualitis**](https://github.com/WeBankFinTech/Qualitis) | Data quality verification tool, providing data verification capabilities such as data integrity and correctness | Qualitis0.8.0 is recommended (**Released **) | >= Qualitis0.9.2 (**Released**) |
| [**Streamis**](https://github.com/WeBankFinTech/Streamis) | Streaming application development management tool. It supports the release of Flink Jar and Flink SQL, and provides the development, debugging and production management capabilities of streaming applications, such as: start-stop, status monitoring, checkpoint, etc. | Not supported | >= Streamis0.2.0 (**Released**) |
| [**Prophecis**](https://github.com/WeBankFinTech/Prophecis) | A one-stop machine learning platform that integrates multiple open source machine learning frameworks. Prophecis' MLFlow can be connected to DSS workflow through AppConn. | Not supported | >= Prophecis 0.3.2 (**Released**) |
| [**Exchangis**](https://github.com/WeBankFinTech/Exchangis) | A data exchange platform that supports data transmission between structured and unstructured heterogeneous data sources, the upcoming Exchangis1. 0, will work with DSS workflow | not supported | = Exchangis1.0.0 (**Released**) |
| [**Visualis**](https://github.com/WeBankFinTech/Visualis) | A data visualization BI tool based on the secondary development of Davinci, an open source project of CreditEase, provides users with financial-level data visualization capabilities in terms of data security. | Recommended Visualis0.5.0 |= Visualis1.0.0 (**Released**) |
| [**DolphinScheduler**](https://github.com/apache/dolphinscheduler) | Apache DolphinScheduler, a distributed and easily scalable visual workflow task scheduling platform, supports one-click publishing of DSS workflows to DolphinScheduler. | Not supported | DolphinScheduler1.3.X (**Released**) |
| **UserGuide** | (DSS will be built-in third-party application tools) contains help documents, beginner's guide, Dark mode skinning, etc. | Not supported | >= DSS1.1.0 (**Released**) |
| **DataModelCenter** | (the third-party application tool that DSS will build) mainly provides data warehouse planning, data model development and data asset management capabilities. Data warehouse planning includes subject domains, data warehouse hierarchies, modifiers, etc.; data model development includes indicators, dimensions, metrics, wizard-based table building, etc.; data assets are connected to Apache Atlas to provide data lineage capabilities . | Not supported | Planned in DSS1.2.0 (**under development**) |
| **UserManager** | (DSS has built-in third-party application tools) automatically initialize all user environments necessary for a new DSS user, including: creating Linux users, various user paths, directory authorization, etc. | Recommended DSS0.9.1 (**Released**) | Planning |
| [**Airflow**](https://github.com/apache/airflow) | Supports publishing DSS workflows to Apache Airflow for scheduled scheduling. | PR not yet merged | Not supported |

&nbsp; &nbsp; &nbsp; &nbsp;Due to the high risk of script execution supported by DataSphere Studio, the isolation of the WeDataSphere Demo environment has not been completed. Considering that everyone is consulting the Demo environment, it is decided to first issue invitation codes to the community and accept trial applications from enterprises and organizations.

&nbsp; &nbsp; &nbsp; &nbsp;If you want to try the Demo environment, please join the DataSphere Studio community user group [Join the group to jump](#1), and contact the team members to get the invitation code.

&nbsp; &nbsp; &nbsp; &nbsp;DataSphereStudio Demo environment user registration page: [Click me to enter](https://dss-open.wedatasphere.com/#/register)

&nbsp; &nbsp; &nbsp; &nbsp;DataSphereStudio Demo environment login page: [Click me to enter](https://dss-open.wedatasphere.com/#/login)


## Documentation directory

&nbsp; &nbsp; &nbsp; &nbsp;Please visit [English Documentation](en_US) for a complete list of DSS documents.

&nbsp; &nbsp; &nbsp; &nbsp;The following is the installation guide for DSS-related AppConn plugins:

- [Visualis AppConn Plugin Installation Guide for DSS](https://github.com/WeBankFinTech/Visualis/blob/master/visualis_docs/en_US/Visualis_appconn_install_en.md)

- [Schedulis AppConn Plugin Installation Guide for DSS](en_US/Installation_and_Deployment/SchedulisAppConn_Plugin_Installation_Documentation.md)

- [Qualitis AppConn Plugin Installation Guide for DSS](https://github.com/WeBankFinTech/Qualitis/blob/master/docs/zh_CN/ch1/%E6%8E%A5%E5%85%A5%E5%B7%A5%E4%BD%9C%E6%B5%81%E6%8C%87%E5%8D%97.md)

- [Exchangis AppConn Plugin Installation Guide for DSS](https://github.com/WeDataSphere/Exchangis/blob/master/docs/en_US/ch1/exchangis_appconn_deploy_en.md)

- [Streamis AppConn Plugin Installation Guide for DSS](https://github.com/WeBankFinTech/Streamis/blob/main/docs/en_US/0.2.0/development/StreamisAppConnInstallationDocument.md)

- [Prophecis AppConn Plugin Installation Guide for DSS](https://github.com/WeBankFinTech/Prophecis/blob/master/docs/zh_CN/Deployment_Documents/Prophecis%20Appconn%E5%AE%89%E8%A3%85%E6%96%87%E6%A1%A3.md)


## Who is using DataSphere Studio

&nbsp; &nbsp; &nbsp; &nbsp;We have created a [Who is using DSS](https://github.com/WeBankFinTech/DataSphereStudio/issues/1) issue for user feedback and documentation of who is using DSS, you are welcome to register.

&nbsp; &nbsp; &nbsp; &nbsp;DSS Since the open source release in 2019, there have been more than 700 test companies and 1,000+ sandbox test users, covering industries such as finance, telecommunications, manufacturing, and the Internet.


## Documentation

&nbsp; &nbsp; &nbsp; &nbsp;DataSphere Studio uses GitBook for management, and the entire project will be organized into a GitBook e-book for everyone to download and use.

&nbsp; &nbsp; &nbsp; &nbsp;WeDataSphere A unified document reading entry will be provided in the future. For the usage of GitBook, please refer to: [GitBook Documentation](http://caibaojian.com/gitbook/)


## contribute

&nbsp; &nbsp; &nbsp; &nbsp;We welcome and look forward to more contributors to build DSS, whether it is code, documentation, or other forms of contribution that can help the community.


## <a id = "1">Contact Us</a>

&nbsp; &nbsp; &nbsp; &nbsp;Any questions and suggestions about DSS, please submit an issue for tracking processing and experience sharing.

&nbsp; &nbsp; &nbsp; &nbsp;You can also scan the QR code below to join our WeChat/QQ group for faster response.

![comminicate](https://github.com/WeBankFinTech/DataSphereStudio/blob/master/images/zh_CN/readme/communication.png)


## License

&nbsp; &nbsp; &nbsp; &nbsp;DSS is under the Apache 2.0 license. See the [License](https://github.com/WeBankFinTech/DataSphereStudio/LICENSE) file for details.
