# Schedulis Linkis JobType 安装文档

> 本文主要介绍 Schedulis 的 Linkis JobType 自动化部署安装步骤，如果手动安装请参考 Azkaban 的 JobType [安装步骤](https://azkaban.github.io/azkaban/docs/latest/#job-types)。

## 1. 准备工作

1.进入 [release页面](https://github.com/WeBankFinTech/DataSphereStudio/releases)，选择对应的安装包进行下载：

- linkis-jobtype-$version.zip

2.解压安装包

```shell script
unzip linkis-jobtype-$version.zip
```

## 2. 修改配置

1.进入bin目录：

```shell script
cd linkis/bin/
```

2.修改配置：

```shell script
##Linkis gateway url 
LINKIS_GATEWAY_URL=http://127.0.0.1:9001 ## linkis 的 GateWay 地址

##Linkis gateway token default WS-AUTH 
LINKIS_GATEWAY_TOKEN=WS-AUTH  ## Linkis 的代理 Token，该参数可以用默认值

##Azkaban executor host 
AZKABAN_EXECUTOR_HOST=127.0.0.1 ## 如果 Schedulis 是单机安装则该 IP 就是机器 IP，如果是分布式安装为 Schedulis 执行器机器 IP。

### SSH Port 
SSH_PORT=22 ## SSH 端口

##Azkaban executor dir 
AZKABAN_EXECUTOR_DIR=/tmp/Install/AzkabanInstall/executor ## 如果 Schedulis 是单机安装则该目录是 Schedulis 的安装目录，如果是分布式安装为执行器的安装目录，注意：最后不需要带上/

##Azkaban executor plugin reload url 
AZKABAN_EXECUTOR_URL=http://$AZKABAN_EXECUTOR_HOST:12321/executor?action=reloadJobTypePlugins ##这里只需要修改 IP 和端口即可，该地址为 Schedulis 重载插件的地址。
```

## 3. 执行安装脚本

```shell script
sh install.sh
```

如果安装成功最后会打印：```{"status":"success"}```

