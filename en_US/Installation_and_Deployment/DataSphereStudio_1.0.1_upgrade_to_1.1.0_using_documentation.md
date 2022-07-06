# DataSphere Studio 1.0.1 upgrade to 1.1.0 using documentation

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

```roomsql
source ${your_path}/from_v101_to_v110.sql
```
It can be executed successfully under normal circumstances.

#### 3. Replace the dss deployment directory with the new version package

First back up the deployment directory of the old version of dss, take this directory as an example: /appcom/Install/DSSInstall
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
###dolphinscheduler system admin user token, if you do not need to install dolphinscheduler-appconn, you do not need to configure this item
wds.dss.appconn.ds.admin.token=
```
2. Add in the configuration file dss-framework-project-server.properties:
```properties
###Or appconn does not implement the desired development specification (node update, delete, copy, import, export operations), it needs to be added to the configuration to ignore the check
wds.dss.appconn.checker.development.ignore.list=workflow,sendemail
###Or appconn does not implement all engineering specifications (add, delete, modify, check), it needs to be added to the configuration to ignore the check
wds.dss.appconn.checker.project.ignore.list=mlss
```

#### 5. Service startup
OK, now you can start the new version of dss service, run the command in the **dss deployment directory** to start all services:

```shell
sh sbin/dss-start-all.sh 
```




