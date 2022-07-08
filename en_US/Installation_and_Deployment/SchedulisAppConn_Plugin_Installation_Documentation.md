#Schedulis AppConn Installation Instructions

>Schedulis is an open-source workflow scheduling system of WeBank's big data platform room. By installing and configuring the AppConn plug-in in DSS, the workflow developed by DSS can be published to Schedulis for scheduled execution.

# 1. Preparation before installation

- You need to install DSS first, please refer to: [DSS single-machine deployment document](DSS&Linkis_one-click_deployment_document_stand-alone_version.md)&nbsp;&nbsp;&nbsp;&nbsp;2. After installing DSS, you can implement basic DSS interactive analysis and workflow execution tasks. If you need to publish DSS workflows to Scheduled for scheduling execution, you first need to install and run [Schedulis](https ://github.com/WeBankFinTech/Schedulis).

- You need to install and run Schedulis first, you can refer to: [Schedulis deployment documentation](https://github.com/WeBankFinTech/Schedulis/blob/master/docs/schedulis_deploy_cn.md)

# 2. Compile the AppConn plugin package separately
- The user first needs to check whether the schedulis directory exists under the xx/dss_linkis/dss/dss-appconns directory. If the user does not exist, the user needs to download the schedulis plugin
- The user installs Schedulis AppConn by executing the script appconn-install.sh, and only needs to enter the specific IP and Port of the machine where Schedulis WEB is deployed, and the AppConn plug-in installation can be completed. When the script is executed, the init.sql script under the corresponding AppConn will be executed, and the corresponding database information will be inserted into the DSS table
```shell
## Change directory to the installation directory of DSS
cd xx/dss

## Execute the appconn-install installation script, enter the corresponding appconn name, and follow the prompts to enter the IP and PORT corresponding to the scheduleulis WEB service,
## Note that when the services are deployed on one machine, do not enter 127.0.0.1 or localhost for the IP, you must enter the real IP
sh bin/appconn-install.sh
>> schedulis
>> xx.xx.xx.xx
>> 8085
```

#### Configure the url for the "Go to dispatch center" button

- Modify the `${DSS_HOME}/conf/dss-workflow-server.properties` configuration:

```properties
#This path corresponds to the page of the dolphinscheduler operation and maintenance center
wds.dss.workflow.schedulerCenter.url="http://${schedulis_ip}:${schedulis_port}"
```

- Then restart the workflow to make the configuration take effect:

```shell script
sh sbin/dss-daemon.sh restart workflow-server
```


### 3. Scheduled JobType plugin installation

- Users also need to install a JobType plugin for Schedulis: linkis-jobtype, please click [Linkis JobType installation document] (Schedulis_Linkis_JobType installation document.md).

### 4. How to use Schedulelis

- Once the Schedulis service is deployed and the Schedulis AppConn is installed, Schedulis can be used in DSS. In the application component, users can click Schedulis to enter Schedulis, or during workflow development, one-click publishing of DSS workflows to Schedulis for scheduled execution.

### 5. Schedulis AppConn installation principle

- The relevant configuration information of Schedulis will be inserted into the following table. By configuring the following table, you can complete the use and configuration of Schedulis. When installing Schedulis AppConn, the script will replace the init.sql under each AppConn and insert it into the table.
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
