#Schedulis AppConn Installation Instructions
>Schedulis is an open-source workflow scheduling system of WeBank's big data platform room. By installing and configuring the AppConn plug-in in DSS, the workflow developed by DSS can be published to Schedulis for scheduled execution.

# 1. Preparation before installation
&nbsp;&nbsp;&nbsp;&nbsp;1. First, you need to install DSS, through one-click installation deployment script or separate installation, you can refer to the DSS installation documentation.
&nbsp;&nbsp;&nbsp;&nbsp;2. After installing DSS, you can implement basic DSS interactive analysis and workflow execution tasks. If you need to publish DSS workflows to Scheduled for scheduling execution, you first need to install and run [Schedulis](https ://github.com/WeBankFinTech/Schedulis).
# 2. Compile the AppConn plugin package separately
&nbsp;&nbsp;&nbsp;&nbsp;After executing DSS compilation, packaging and deployment, the related AppConn packages will be compiled at the same time. If you want to compile Schedules AppConn separately, you need to refer to the following steps:
```bash 
cd {DSS_SOURCE_CODE_PROJECT}/dss-appconn/appconns/dss-schedulis-appconn
mvn clean install
```
# 3. Install Scheduler AppConn
&nbsp;&nbsp;&nbsp;&nbsp;In the DSS installation directory, the appconns directory already contains AppConn jar packages for third-party application tools connected to DSS, including the Schedulis AppConn jar package described in this document.
&nbsp;&nbsp;&nbsp;&nbsp;When installing Schedulis, by executing the following script, you only need to simply enter the IP of the Schedulis deployment machine and the service Port. You can configure and complete the installation of the corresponding AppConn plug-in. When the script is executed, the init.sql script under the corresponding AppConn will be executed, and the corresponding database information will be inserted into the DSS table.
````sh
sh ${DSS_HOME}bin/appconn-install.sh

# After executing the appcon-install installation script, enter the corresponding appconn name
# Follow the prompts to enter the IP corresponding to the schedulelis service, and the PORT
>> schedulis
>> 127.0.0.1
>> 8089
````

# 4. How to use Schedulis
&nbsp;&nbsp;&nbsp;&nbsp;Schedulis service is deployed and Scheduled AppConn is installed, you can use Schedulis in DSS. In the app store, click Schedules to enter Schedules. During workflow development, you can publish DSS workflows to Schedules for scheduled execution with one click.

# 5. Schedulis AppConn installation principle
&nbsp;&nbsp;&nbsp;&nbsp;Schedulis related configuration information will be inserted into the following table, by configuring the following table, you can complete the use and configuration of Schedulis, when installing the Schedulis AppConn, the script will replace the init.sql under each AppConn, and Insert into the table.

| Table Name      | Table function   | Remark                                   |
|-----------------|----------------|----------------------------------------|
| dss_application       | Application table, mainly to insert the basic information of schedule application | must                                   |
| dss_menu     | Menu table, which stores the content displayed to the outside world, such as icons, names, etc. | must                                   |
| dss_onestop_menu_application | The association table of menu and application for joint search |                    must                |
| dss_appconn      | Basic information of appconn, used to load appconn  | must                                   |
| dss_appconn_instance  | Information about the instance of AppConn, including its own url information | must         |
| dss_workflow_node  | Schedules as workflow nodes need to insert information | If using a visualization node, you must         |

&nbsp;&nbsp;&nbsp;&nbsp;Schedulis, as a scheduling framework, implements the first-level specification and the second-level specification. The microservices that need to use Schedulelis AppConn are as follows.

| Microservice name | Function done using AppConn | Remarks                               |
|-----------------|----------------|----------------------------------------|
| dss-framework-project-server | Use schedule-appconn to complete the project and unify the organization | must |
| dss-workflow-server | Use scheduling AppConn to realize workflow publishing and status acquisition | must                                   |
