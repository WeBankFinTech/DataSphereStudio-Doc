# Schedulis Linkis JobType 安装文档

> 本文主要介绍 Schedulis 的 Linkis JobType 自动化部署安装步骤，如果手动安装请参考 Azkaban 的 JobType [安装步骤](https://azkaban.github.io/azkaban/docs/latest/#job-types)

1. 进入到Schedulis目录

```
##用户首先需要到Schedulis的安装目录，具体操作命令如下：
cd xx/schedulis_0.7.0_exec/plugins/jobtypes/linkis/bin
```

2. 修改config.sh配置

```
##Linkis gateway url 
LINKIS_GATEWAY_URL=http://127.0.0.1:9001 ## linkis 的 GateWay 地址

##Linkis gateway token default WS-AUTH 
LINKIS_GATEWAY_TOKEN=WS-AUTH  ## Linkis 的代理 Token，该参数可以用默认值

##Azkaban executor host 
AZKABAN_EXECUTOR_HOST=127.0.0.1 ## 如果 Schedulis 是单机安装则该 IP 就是机器 IP，如果是分布式安装为 Schedulis 执行器机器 IP

### SSH Port 
SSH_PORT=22 ## SSH 端口

##Azkaban executor dir 
AZKABAN_EXECUTOR_DIR=xx/schedulis_0.7.0_exec ## 如果 Schedulis 是单机安装则该目录是 Schedulis 的安装目录，如果是分布式安装为执行器的安装目录，注意：最后不需要带上/

##Azkaban executor plugin reload url 
AZKABAN_EXECUTOR_URL=http://$AZKABAN_EXECUTOR_HOST:12321/executor?action=reloadJobTypePlugins ##这里只需要修改 IP 和端口即可，该地址为 Schedulis 重载插件的地址。
```

3. 执行安装脚本

```
sh install.sh
```




