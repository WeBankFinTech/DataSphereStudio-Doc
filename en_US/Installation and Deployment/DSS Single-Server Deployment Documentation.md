# DataSphere Studio one-click installation documentation

## 1. Environment preparation before use

### a. Basic software installation

Command tools required by Linkis (before the official installation, the script will automatically detect whether these commands are available. If they do not exist, they will try to install them automatically. If the installation fails, the user needs to manually install the following basic shell command tools):

-   telnet
-   tar
-   sed
-   dos2unix
-   mysql
-   yum
-   java
-   unzip
-   expect

Software to be installed:

- MySQL (5.5+)

- JDK (1.8.0_141 and above)

- Python (both 2.x and 3.x are supported)

- Nginx


The following services must be accessible from this machine:

- Hadoop (**2.7.2, other versions of Hadoop need to compile Linkis**), the installed machine must support the execution of the ``` hdfs dfs -ls / ``` command

- Hive (**2.3.3, other versions of Hive need to compile Linkis**), the installed machine must support the execution of the ``` hive -e "show databases" ``` command

- Spark (**supports all versions above 2.0**), the installed machine must support the execution of the ```spark-sql -e "show databases" ``` command

Tips:

If you are installing Hadoop for the first time, you can refer to: [Hadoop single-machine deployment](https://hadoop.apache.org/docs/r2.7.2/hadoop-project-dist/hadoop-common/SingleCluster.html ); for distributed deployment of Hadoop, please refer to: [Hadoop Distributed Deployment](https://hadoop.apache.org/docs/r2.7.2/hadoop-project-dist/hadoop-common/ClusterSetup.html).

If you are installing Hive for the first time, please refer to: [Hive Quick Installation and Deployment](https://cwiki.apache.org/confluence/display/Hive/GettingStarted).

If you are installing Spark for the first time, you can refer to the On Yarn mode: [Spark on Yarn deployment](http://spark.apache.org/docs/2.4.3/running-on-yarn.html).

### b. Create User

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;For example: **The deployment user is the hadoop account** (it may not be the hadoop user, but it is recommended to use the Hadoop super user for deployment, here is just an example)

2. Create a deployment user on all machines that need to be deployed for installation

```bash
sudo useradd hadoop
```

3. Because the Linkis service uses sudo -u ${linux-user} to switch engines to execute jobs, the deployment user needs to have sudo privileges and is password-free.

```bash
vi /etc/sudoers
````

````properties
     hadoop ALL=(ALL) NOPASSWD: NOPASSWD: ALL
````

4. Make sure that the server where DSS and Linkis are deployed can execute commands such as hdfs , hive -e and spark-sql -e normally. In the one-click install script, the components are checked.

5. **If your Pyspark wants to have the drawing function, you also need to install the drawing module on all installation nodes**. The command is as follows:
    

```bash
python -m pip install matplotlib
```

### c.Installation preparation

Compile by yourself or go to the component release page to download the installation package:

2. Download the installation package

-   [wedatasphere-linkis-x.x.x-dist.tar.gz](https://github.com/WeBankFinTech/Linkis/releases)
-   [wedatasphere-dss-x.x.x-dist.tar.gz](https://github.com/WeBankFinTech/DataSphereStudio/releases)
-   [wedatasphere-dss-web-x.x.x-dist.zip](https://github.com/WeBankFinTech/DataSphereStudio/releases)

3. Download the DSS & LINKIS one-click installation deployment package, and unzip it. The following is the hierarchical directory structure of the one-click installation deployment package:

````text
├── dss_linkis # One-click deployment home directory
  ├── bin # for one-click installation, and one-click to start DSS + Linkis
  ├── conf # Parameter configuration directory for one-click deployment
  ├── wedatasphere-dss-x.x.x-dist.tar.gz # DSS background installation package
  ├── wedatasphere-dss-web-x.x.x-dist.zip # DSS front-end installation package
  ├── wedatasphere-linkis-x.x.x-dist.tar.gz # Linkis installation package
```

### d. Modify the configuration

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Open conf/config.sh and modify the relevant configuration parameters as needed:

```bash
vi conf/config.sh
````

The parameter description is as follows:

```properties
#################### Basic configuration for one-click installation and deployment ####################

# Deployment user, the default is the current logged-in user, it is not recommended to modify it if it is not necessary
# deployUser=hadoop

# Not required and not recommended to modify
# LINKIS_VERSION=1.0.2

### DSS Web, no modification required for native installation
#DSS_NGINX_IP=127.0.0.1
#DSS_WEB_PORT=8088

# Not required and not recommended to modify
#DSS_VERSION=1.0.0

## The stack size of the Java application. If the memory of the deployment machine is less than 8G, 128M is recommended; when it reaches 16G, at least 256M is recommended; if you want to have a very good user experience, it is recommended that the memory of the deployment machine be at least 32G.
export SERVER_HEAP_SIZE="128M"

################################################## ##########
##################### Linkis configuration starts #####################
########## Non-commented parameters must be configured, and commented out parameters can be modified as needed ##########
################################################## ##########

### DSS workspace directory
WORKSPACE_USER_ROOT_PATH=file:///tmp/linkis/
### User HDFS root path
HDFS_USER_ROOT_PATH=hdfs:///tmp/linkis
### result set path: file or hdfs path
RESULT_SET_ROOT_PATH=hdfs:///tmp/linkis

### Path to store started engines and engine logs, must be local
ENGINECONN_ROOT_PATH=/appcom/tmp

#ENTRANCE_CONFIG_LOG_PATH=hdfs:///tmp/linkis/

### HADOOP configuration file path, must be configured
HADOOP_CONF_DIR=/appcom/config/hadoop-config
### HIVE CONF DIR
HIVE_CONF_DIR=/appcom/config/hive-config
### SPARK CONF DIR
SPARK_CONF_DIR=/appcom/config/spark-config

# for install
#LINKIS_PUBLIC_MODULE=lib/linkis-commons/public-module


## YARN REST URL
YARN_RESTFUL_URL=http://127.0.0.1:8088

## Engine version configuration, if not configured, the default configuration is used
#SPARK_VERSION
#SPARK_VERSION=2.4.3
##HIVE_VERSION
#HIVE_VERSION=1.2.1
#PYTHON_VERSION=python2

## LDAP is for enterprise authorization, if you just want to have a try, ignore it.
#LDAP_URL=ldap://localhost:1389/
#LDAP_BASEDN=dc=webank,dc=com
#LDAP_USER_NAME_FORMAT=cn=%s@xxx.com,OU=xxx,DC=xxx,DC=com

# Microservices Service Registration Discovery Center
#LINKIS_EUREKA_INSTALL_IP=127.0.0.1         
#LINKIS_EUREKA_PORT=20303
#LINKIS_EUREKA_PREFER_IP=true

###  Gateway install information
#LINKIS_GATEWAY_PORT =127.0.0.1
#LINKIS_GATEWAY_PORT=9001

### ApplicationManager
#LINKIS_MANAGER_INSTALL_IP=127.0.0.1
#LINKIS_MANAGER_PORT=9101

### EngineManager
#LINKIS_ENGINECONNMANAGER_INSTALL_IP=127.0.0.1
#LINKIS_ENGINECONNMANAGER_PORT=9102

### EnginePluginServer
#LINKIS_ENGINECONN_PLUGIN_SERVER_INSTALL_IP=127.0.0.1
#LINKIS_ENGINECONN_PLUGIN_SERVER_PORT=9103

### LinkisEntrance
#LINKIS_ENTRANCE_INSTALL_IP=127.0.0.1
#LINKIS_ENTRANCE_PORT=9104

###  publicservice
#LINKIS_PUBLICSERVICE_INSTALL_IP=127.0.0.1
#LINKIS_PUBLICSERVICE_PORT=9105

### cs
#LINKIS_CS_INSTALL_IP=127.0.0.1
#LINKIS_CS_PORT=9108

##################### Linkis configuration completed #####################

################################################## ##########
####################### DSS configuration starts #######################
########## Non-commented parameters must be configured, and commented out parameters can be modified as needed ##########
################################################## ##########

# Used to store temporary ZIP package files published to Schedules
WDS_SCHEDULER_PATH=file:///appcom/tmp/wds/scheduler

### This service is used to provide dss-framework-project-server capability.
#DSS_FRAMEWORK_PROJECT_SERVER_INSTALL_IP=127.0.0.1
#DSS_FRAMEWORK_PROJECT_SERVER_PORT=9002

### This service is used to provide dss-framework-orchestrator-server capability.
#DSS_FRAMEWORK_ORCHESTRATOR_SERVER_INSTALL_IP=127.0.0.1
#DSS_FRAMEWORK_ORCHESTRATOR_SERVER_PORT=9003

### This service is used to provide dss-apiservice-server capability.
#DSS_APISERVICE_SERVER_INSTALL_IP=127.0.0.1
#DSS_APISERVICE_SERVER_PORT=9004

### This service is used to provide dss-workflow-server capability.
#DSS_WORKFLOW_SERVER_INSTALL_IP=127.0.0.1
#DSS_WORKFLOW_SERVER_PORT=9005

### dss-flow-Execution-Entrance
### This service is used to provide flow execution capability.
#DSS_FLOW_EXECUTION_SERVER_INSTALL_IP=127.0.0.1
#DSS_FLOW_EXECUTION_SERVER_PORT=9006

### This service is used to provide dss-datapipe-server capability.
#DSS_DATAPIPE_SERVER_INSTALL_IP=127.0.0.1
#DSS_DATAPIPE_SERVER_PORT=9008

##sendemail configuration, only affects the sending email function in DSS workflow
EMAIL_HOST=smtp.163.com
EMAIL_PORT=25
EMAIL_USERNAME=xxx@163.com
EMAIL_PASSWORD=xxxxx
EMAIL_PROTOCOL=smtp
####################### DSS configuration ends #######################
```

### f. Modify database configuration

Please make sure that the configured database and the installation machine can be accessed normally, otherwise the DDL and DML import failure error will occur.

```bash
vi conf/db.sh 
```

```properties
### Configure DSS database
MYSQL_HOST=127.0.0.1
MYSQL_PORT=3306
MYSQL_DB=dss
MYSQL_USER=xxx
MYSQL_PASSWORD=xxx

## Hive metastore database configuration, used for Linkis to access Hive metadata information
HIVE_HOST=127.0.0.1
HIVE_PORT=3306
HIVE_DB=xxx
HIVE_USER=xxx
HIVE_PASSWORD=xxx
```

## Second, installation and use

### 1. Execute the installation script:

```bash
sh bin/install.sh
````

### 2. Installation steps

- The installation script will check various integrated environment commands. If not, please follow the prompts to install. The following commands are required:

  _yum java mysql unzip expect telnet tar sed dos2unix nginx_

- When installing, the script will ask if you need to initialize the database and import metadata, both Linkis and DSS will ask.


&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**First time installation** must be Yes.

### 3. Whether the installation is successful:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Check whether the installation is successful by viewing the log information printed on the console.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;If there is an error message, you can check the specific error reason.

### 4. Start the service

#### (1) Start the service:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Execute the following command in the installation directory to start all services:

```shell
sh bin/start-all.sh
````

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;If the startup generates an error message, you can check the specific error reason. After startup, each microservice will perform **communication detection**, and if there is an abnormality, it can help users locate the abnormal log and cause.

#### (2) Check if the startup is successful

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;You can view the startup status of Linkis & DSS background microservices on the Eureka interface.
![Eureka](../Images/Install_and_Deploy/Standalone_Deployment_Documentation/eureka.png)

#### (3) Google Chrome access:

Please use **Google Chrome** to visit the following frontend address:

`http://DSS_NGINX_IP:DSS_WEB_PORT` **The startup log will print this access address**. When logging in, the administrator's username and password are both the deployment username. If the deployment user is hadoop, the administrator's username/password is: hadoop/hadoop.

#### (4) Stop the service:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Execute the following command in the installation directory to stop all services:

```bash
sh bin/stop-all.sh
```
