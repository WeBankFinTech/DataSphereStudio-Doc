DataSphere Studio
====

[![License](https://img.shields.io/badge/license-Apache%202-4EB1BA.svg)](https://www.apache.org/licenses/LICENSE-2.0.html)

English | [中文](README-ZH.md)

## Introduction

 &nbsp; &nbsp; &nbsp; &nbsp;DataSphere Studio (DSS for short) is WeDataSphere, a big data platform of WeBank, a self-developed one-stop data application development management portal.

 &nbsp; &nbsp; &nbsp; &nbsp;DataSphere Studio is positioned as a data application development framework, and the closed loop covers the entire process of data application development. With a unified UI, the workflow-like graphical drag-and-drop development experience meets the entire lifecycle of data application development from data import, desensitization cleaning, data analysis, data mining, quality inspection, visualization, scheduling to data output applications, etc.

 &nbsp; &nbsp; &nbsp; &nbsp;With a pluggable framework architecture, DSS is designed to allow users to quickly integrate new data application tools, or replace various tools that DSS has integrated.

## Integrated data application components

 &nbsp; &nbsp; &nbsp; &nbsp;DSS has integrated a variety of upper-layer data application systems by implementing multiple AppConns, which can basically meet the data development needs of users.

 &nbsp; &nbsp; &nbsp; &nbsp;**If desired, new data application systems can also be easily integrated to replace or enrich DSS's data application development process.** [Click me to learn how to quickly integrate new application systems](en_US/Development_Documentation/Third-party_System_Access_Development_Guide.md)

| Component | Description | DSS0.X Version Requirements | DSS1.0 Version Requirements | Version Planning |
| --------------- | -------------------------------------------------------------------- | --------- | ---------- | ------ |
| [**DataApiService**](en_US/Using_Document/DataApiService_Usage_Documentation) | Data API service. The SQL script can be quickly published as a Restful interface, providing Rest access capability to the outside world. | Not supported | >=1.0.0 | Released |
| [**Scriptis**](https://github.com/WeBankFinTech/Scriptis) | Support online script writing such as SQL, Pyspark, HiveQL, etc., submit to [Linkis](https://github.com/WeBankFinTech/Linkis ) to perform data analysis web tools. | >=0.5.0 | >=1.0.0 | Released |
| [**Schedulis**](https://github.com/WeBankFinTech/Schedulis) | Workflow task scheduling system based on Azkaban secondary development, with financial-grade features such as high performance, high availability and multi-tenant resource isolation. | >=0.5.0 | >=1.0.0 | Released |
| **EventCheck** | Provides cross-business, cross-engineering, and cross-workflow signaling capabilities. | >=0.5.0 | >=1.0.0 | Released |
| **SendEmail** | Provides the ability to send data, all the result sets of other workflow nodes can be sent by email | >=0.5.0 | >=1.0.0 | Released |
| [**Qualitis**](https://github.com/WeBankFinTech/Qualitis) | Data quality verification tool, providing data verification capabilities such as data integrity and correctness | >=0.5.0 | 1.0.1(Version currently in preparation) | **Expected end of January** |
| [**Streamis**](https://github.com/WeBankFinTech/Streamis) | Streaming application development management tool. It supports the release of Flink Jar and Flink SQL, and provides the development, debugging and production management capabilities of streaming applications, such as: start-stop, status monitoring, checkpoint, etc. | Not supported | 1.0.1(Version currently in preparation) | **Expected end of January** |
| [**Exchangis**](https://github.com/WeBankFinTech/Exchangis) | A data exchange platform that supports data transmission between structured and unstructured heterogeneous data sources, the upcoming Exchangis1. 0, will be connected with DSS workflow | not supported | Planned in 1.0.2 | **In Development** |
| [**Visualis**](https://github.com/WeBankFinTech/Visualis) | A data visualization BI tool based on the second development of Davinci, an open source project of CreditEase, provides users with financial-level data visualization capabilities in terms of data security. | >=0.5.0 | Planned in 1.0.2 | **In Development** |
| [**Prophecis**](https://github.com/WeBankFinTech/Prophecis) | A one-stop machine learning platform that integrates multiple open source machine learning frameworks. Prophecis' MLFlow can be connected to DSS workflow through AppConn. | Not supported | Planned in 1.0.2 | **In Development** |
| **UserManager** | Automatically initialize all user environments necessary for a new DSS user, including: creating Linux users, various user paths, directory authorization, etc. | >=0.9.1 | Planned in 1.0.2 | **In Development** |
| **DolphinScheduler** | Apache DolphinScheduler, a distributed and scalable visual workflow task scheduling platform, supports one-click publishing of DSS workflows to DolphinScheduler. | Not supported | Planned in 1.1.0 | **In Development** |
| **UserGuide**     | It mainly provides help documentation, beginner's guide, Dark mode skinning, etc.      | Not supported | Planning in 1.1.0 | **In Development** |
| **DataModelCenter** | It mainly provides the capabilities of data warehouse planning, data model development and data asset management. Data warehouse planning includes subject domains, data warehouse layers, modifiers, etc.; data model development includes indicators, dimensions, metrics, wizard-based table building, etc.; data assets are connected to Apache Atlas to provide data lineage capabilities. | Not supported | Planning in 1.2.0 | **In Development** |
| **Airflow** | Supports publishing DSS workflows to Airflow for scheduling. | >=0.9.1, not yet merged | Not supported | **No plans yet** |


##  Download

 &nbsp; &nbsp; &nbsp; &nbsp;Please go to the [DSS Releases Page](https://github.com/WeBankFinTech/DataSphereStudio/releases) to download a compiled version or a source code package of DSS.

## Compile and deploy

 &nbsp; &nbsp; &nbsp; &nbsp;Please follow [Compile Guide](en_US/Development_Documentation/Compilation_Documentation.md) to compile DSS from source code.

 &nbsp; &nbsp; &nbsp; &nbsp;Please refer to [Deployment Documents](en_US/Installation_and_Deployment/DSS_Single-Server_Deployment_Documentation.md) to do the deployment.

## Demo Trial environment

 &nbsp; &nbsp; &nbsp; &nbsp;The function of DataSphere Studio supporting script execution has high security risks, and the isolation of the WeDataSphere Demo environment has not been completed. Considering that many users are inquiring about the Demo environment, we decided to first issue invitation codes to the community and accept trial applications from enterprises and organizations.

 &nbsp; &nbsp; &nbsp; &nbsp;If you want to try out the Demo environment, please join the DataSphere Studio community user group (**Please refer to the end of the document**), and contact **WeDataSphere Group Robot** to get an invitation code.

 &nbsp; &nbsp; &nbsp; &nbsp;DataSphereStudio Demo environment user registration page: [click me to enter](https://dss-open.wedatasphere.com/#/register)

 &nbsp; &nbsp; &nbsp; &nbsp;DataSphereStudio Demo environment login page: [click me to enter](https://dss-open.wedatasphere.com/#/login)

## Documents

 &nbsp; &nbsp; &nbsp; &nbsp;For a complete list of documents for DSS1.0, see [DSS-Doc](en_US)

 &nbsp; &nbsp; &nbsp; &nbsp;The following is the installation guide for DSS-related AppConn plugins:

- [Visualis AppConn Plugin Installation Guide](en_US/Installation_and_Deployment/VisualisAppConn_Plugin_Installation_Documentation.md)

- [Schedulis AppConn Plugin Installation Guide](en_US/Installation_and_Deployment/SchedulisAppConn_Plugin_Installation_Documentation.md)

- [Qualitis AppConn Plugin Installation Guide](en_US/Installation_and_deployment/Installation_and_Deployment/QualitisAppConn_Plugin_Installation_Documentation.md)

- [Exchangis AppConn Plugin Installation Guide](en_US/Installation_and_deployment/Installation_and_Deployment/ExchangisAppConn_Plugin_Installation_Documentation.md)

## Who is using DSS

 &nbsp; &nbsp; &nbsp; &nbsp;We opened an issue for users to feedback and record who is using DSS.

 &nbsp; &nbsp; &nbsp; &nbsp;Since the first release of DSS in 2019, it has accumulated more than 700 trial companies and 1000+ sandbox trial users, which involving diverse industries, from finance, banking, tele-communication, to manufactory, internet companies and so on.


## Document statement

 &nbsp; &nbsp; &nbsp; &nbsp;DataSphere Studio uses GitBook for management, and the entire project will be organized into a GitBook e-book for everyone to download and use.

 &nbsp; &nbsp; &nbsp; &nbsp;WeDataSphere will provide a unified document reading entry in the future. For the usage of GitBook, please refer to: [GitBook Documentation](http://caibaojian.com/gitbook/)。

## Contributing

 &nbsp; &nbsp; &nbsp; &nbsp;Contributions are always welcomed, we need more contributors to build DSS together. either code, or doc, or other supports that could help the community.

 &nbsp; &nbsp; &nbsp; &nbsp;For code and documentation contributions, please follow the contribution guide.

## Communication

 &nbsp; &nbsp; &nbsp; &nbsp;For any questions or suggestions, please kindly submit an issue.

 &nbsp; &nbsp; &nbsp; &nbsp;You can scan the QR code below to join our WeChat and QQ group to get more immediate response.


## License

 &nbsp; &nbsp; &nbsp; &nbsp;DSS is under the Apache 2.0 license. See the [License](https://github.com/WeBankFinTech/DataSphereStudio/blob/master/LICENSE) file for details.
