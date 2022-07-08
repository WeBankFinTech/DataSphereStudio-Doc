
# Debug related

> No programmer can write code without any bugs in one go, so many programmers spend a considerable part of their time on debugging. Program debugging is a job that every programmer must face. The following will guide you how to perform dss. Remote debugging (based on DSS1.1.0 version).

## step 1 Prepare DSS source code and compile

```plain
git clone https://github.com/WeBankFinTech/DataSphereStudio.git
cd DataSphereStudio
#If necessary, you can switch to the corresponding branch
#git checkout dev-xxx
mvn -N install 
mvn clean Install
```

## step 2 Deploy the DSS service on the server
If DSS has not been deployed, please refer to the deployment document: [DSS single-machine deployment document](DSS&Linkis_one-click_deployment_document_stand-alone_version.md)

## step 3 Open debug port

First, you need to identify the service where the package to be debugged is located, and determine the service to which it belongs according to the location of the code to be debugged.

Then enter the ${DSS_HOME}/sbin/ext directory under the dss deployment directory, modify the startup script file of the corresponding service to be debugged, and open the remote call port: (take the workflow-server service as an example)

```shell
cd ${DSS_HOME}/sbin/ext

vim dss-workflow-server
```
Find the DEBUG_PORT keyword in the script, and then enter the port number that needs to be opened **(to be able to connect from the local)**

![img.png](../Images/Installation_and_Deployment/DSS_Debug_Documentation/img.png)

Then you need to restart the corresponding service to make it take effect:

```
sh sbin/dss-daemon.sh restart workflow-server
```
Note: If you are not sure about the service name, you can query it in the ${DSS_HOME}/sbin/common.sh script,
Just enter the keyword of the service to start the corresponding service:

![img_1.png](../Images/Install_and_Deploy/DSS_Debug_Documentation/img_1.png)


## step 4 IDEA Compiler configuration remote debugging
Open the window as shown below and configure remote debugging ports, services, and modules:  

![img_2.png](../Images/Install_and_Deploy/DSS_Debug_Documentation/img_2.png)

## step 5 start debugging

Click the debug button in the upper right corner of the idea to start debugging:

![img_3.png](../Images/Install_and_Deploy/DSS_Debug_Documentation/img_3.png)

## step 6 Replace jar package

After modifying the code locally, you can package the jar package of the corresponding module, and then replace the jar of the corresponding lib on the server. Just restart the service.

```shell
cd ${DSS_HOME}

## Upload the jar package to the server
rz -bye

## Replace the uploaded jar package with the original jar package
cp ${your_jar_name} lib/${jar_path}
```

Note: If you don't know which service libs the jar exists in, you can search all the locations of the jar with the following command:
```shell
cd ${DSS_HOME}

## Search all dss-orchestrator-common-*.jar packages in the lib directory
find lib/ -name "*dss-orchestrator-common*"
```

![img_4.png](../Images/Install_and_Deploy/DSS_Debug_Documentation/img_4.png)