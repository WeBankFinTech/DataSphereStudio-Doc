# DataSphere Studio 1.0.1 upgrade to 1.1.0 using documentation(apply to 1.01 to 1.1.1)

### The upgrade steps are mainly divided into:
- service stopped
- Execute database upgrade scripts
- Replace the dss deployment directory with the new version package
- Configuration file addition, modification
- service start

#### 1. Service stopped
Go to the deployment directory of dss, and execute the command in the directory to stop all services of dss:
```shell
cd ${DSS_DEPLOY_PATH}

sh sbin/dss-stop-all.sh
````
#### 2. Execute the database upgrade sql script

How to get the upgrade sql script:

- The decompressed directory of the installation package of dss1.1.0: db/version_update/from_v101_to_v110.sql
- Download from the github page, the address is: (to be uploaded and added)

Then log in to the dss database and execute the source command:

```shell
source ${your_path}/from_v101_to_v110.sql
```
It can be executed successfully under normal circumstances.

#### 3. Replace the dss deployment directory with the new version package

**(Important: it is best to back up the database of the old version of dss first)**

- Back up the following tables with structural changes through the mysqldump command:

dss_appconn、dss_appconn_instance、dss_workflow_node、dss_onestop_user_favorites、dss_component_role、dss_onestop_menu_application



These tables just modify the table name, and can also be backed up:

dss_dictionary
dss_role
dss_admin_dept
dss_download_audit
dss_flow_relation
dss_flow_edit_lock
dss_onestop_menu
dss_menu_role

- Back up the deployment directory of the old version of dss, take this directory as an example:/appcom/Install/DSSInstall
```shell
mv /appcom/Install/DSSInstall /appcom/Install/DSSInstall-bak
```

Put the installation package of dss1.1.0 in the temporary directory and decompress it:
```shell
mkdir /tmp/dss-pkg
mv wedatasphere-dss-1.1.0-dist.tar.gz /tmp/dss-pkg/
cd /tmp/dss-pkg
tar zxvf wedatasphere-dss-1.1.0-dist.tar.gz
```
The directory structure after decompression is as follows:
![img.png](../Images/Install_and_Deploy/DolphinschedulerAppConn_deployment/img.png)

Then copy all the files in the dss-1.1.0 directory to the installation directory of dss1.1.0:
```shell
cd dss-1.1.0
cp -r lib dss-appconns sbin /appcom/Install/DSSInstall/
```

Copy the configuration file from the previous version:
```shell
cp -r /appcom/Install/DSSInstall-bak/conf /appcom/Install/DSSInstall/
```

#### 4. Add and modify configuration

New configuration added in the new version: dss-scriptis-server.properties, dss-guide-server.properties,
Copy directly from the dss1.1.0/conf directory:

```shell
cp -r conf/dss-scriptis-server.properties /appcom/Install/DSSInstall/conf/
cp -r conf/dss-guide-server.properties /appcom/Install/DSSInstall/conf/
```

Configuration modification:

1. Add to the dss.properties configuration file:
```properties
###If appconn does not implement all development specifications (node update, delete, copy, import, export operations), it needs to be added to the configuration to ignore the check
wds.dss.appconn.checker.development.ignore.list=workflow,sendemail
###If appconn does not implement all project specifications (add, delete, modify, check), it needs to be added to the configuration to ignore the check
wds.dss.appconn.checker.project.ignore.list=
```

#### 5. service start
OK, now you can start the new version of dss service, run the command in the **dss deployment directory** to start all services:

```shell
sh sbin/dss-start-all.sh 
```




