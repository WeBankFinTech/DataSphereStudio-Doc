# Schedulis Linkis JobType installation documentation

> This article mainly introduces the automatic deployment and installation steps of Schedulis' Linkis JobType. If you install it manually, please refer to Azkaban's JobType [installation steps](https://azkaban.github.io/azkaban/docs/latest/#job-types)

1. Go to the Schedules directory

````
##Users first need to go to the installation directory of Schedulelis. The specific operation commands are as follows:
cd xx/schedulis_0.7.0_exec/plugins/jobtypes/linkis/bin
```

2. Modify config.sh configuration

```
## Linkis gateway url 
LINKIS_GATEWAY_URL=http://127.0.0.1:9001 ## GateWay address for linkis

## Linkis gateway token default WS-AUTH 
LINKIS_GATEWAY_TOKEN=WS-AUTH  ## The proxy token of Linkis, this parameter can use the default value

## Azkaban executor host 
AZKABAN_EXECUTOR_HOST=127.0.0.1 ## This IP is the machine IP if Schedulis is a stand-alone installation, or the Schedulis executor machine IP if it is a distributed installation

## SSH Port 
SSH_PORT=22 ## SSH port

## Azkaban executor dir 
AZKABAN_EXECUTOR_DIR=xx/schedulis_0.7.0_exec ## If Schedulis is a stand-alone installation, this directory is the installation directory of Schedulis. If it is a distributed installation, it is the installation directory of the executor. Note: You do not need to bring / at the end.

## Azkaban executor plugin reload url 
AZKABAN_EXECUTOR_URL=http://$AZKABAN_EXECUTOR_HOST:12321/executor?action=reloadJobTypePlugins ## You only need to modify the IP and port here.
```

3. Execute the installation script

```
sh install.sh
```




