# DataSphere Studio 单机一键部署文档

### 零、部署前注意事项（重要！！！）
- 确保安装的系统为CentOS为6,7,8

- 服务器存在多网卡问题。首先通过命令`ifconfig`命令查看服务器激活状态的网卡，若激活状态的网卡数大于1，那么用户就需要通过命令`ifconfig [NIC_NAME] down`(`[NIC_NAME]`为网卡名称)来关闭多余的网卡，以确保激活的网卡数只有1个

- 网卡多IP问题。在确保服务器只存在一个网卡是激活状态的情况下，通过命令`echo $(hostname -I)`查看网卡对应的IP数，若大于1，那么就需要去掉网卡中指定的IP，采用动态获取IP的方式，具体命令如下：
    ```shell
      ip addr flush dev [NIC_NAME]
      ifdown [NIC_NAME]
      ifup [NIC_NAME]
    ```

- hostname配置。在安装前用户需要配置hostname到ip的映射

- 若未进行上述设置，安装脚本会默认获取第一个网卡ip

### 一、基础软件安装

- 需要的命令工具（在正式安装前，脚本会自动检测这些命令是否可用，如果不存在会尝试自动安装，安装失败则需用户手动安装以下基础shell命令工具）：

  *telnet; tar; sed; dos2unix; mysql; yum; java; unzip; zip; expect*

- 需要安装的软件：

  MySQL (5.5+); JDK (1.8.0_141以上); Nginx

- **Tips: 请确保已安装Linkis**

### 二、创建用户

1. 假设**部署用户是hadoop账号**（可以不是hadoop用户，但是推荐使用Hadoop的超级用户进行部署，这里只是一个示例）


2. 在所有需要部署的机器上创建部署用户，用于安装 ，如下命令创建部署用户hadoop

   ```shell
   sudo useradd hadoop
   ```

3. 改部署用户权限

   编辑/etc/sudoers文件：

   ```shell
   vi /etc/sudoers
   ```

   在/etc/sudoers文件中添加下面内容：

   ```
   hadoop  ALL=(ALL)  NOPASSWD: NOPASSWD: ALL
   ```

### 三、准备安装包

- 用户可以自行编译或者去 release 页面下载安装包：[DSS Release-1.1.2](https://github.com/WeBankFinTech/DataSphereStudio/releases/tag/1.1.2)

- DSS 一键安装部署包的层级目录结构如下：

    ```text
    ├── dss_install # 一键部署主目录
      ├── bin # 用于一键安装，以及一键启动 DSS
      ├── conf # 一键部署的参数配置目录
      ├── wedatasphere-dss-x.x.x-dist.tar.gz # DSS后端安装包
      ├── wedatasphere-dss-web-x.x.x-dist.zip # DSS前端安装包
    ```

- 如果用户选择采用下载安装包直接部署的形式，可直接跳转到 [修改配置](#1)


- 如果用户选择自行编译DSS，请确保编译的是DSS `master` 分支的最新代码，编译方式可以参考:  
  [DSS后端编译文档](../开发文档/DSS编译文档.md)  
  [DSS前端编译文档](../开发文档/前端编译文档.md)


        1. 针对后端安装包可直接将上面的 DSS 后端安装包替换成编译后安装包即可。

        2. 针对前端安装包，则需要特别注意，整个前端安装包目录结构如下：
        ```
        ├── wedatasphere-dss-web-x.x.x-dist # DSS前端安装包
          ├── config.sh # 参数配置脚本
          ├── install.sh # 前端部署脚本
          ├── dist # DSS前端包
        ```

        3. DSS前端包可直接替换成用户编译后的相关安装包。
   
        4. 用户在打包wedatasphere-dss-web-x.x.x-dist.zip的时候需要特别注意，不要在父级目录对其直接压缩，应全选目录下面的所有文件然后压缩。


### <a id = "1">四、修改配置</a>

- 用户需要对 `xx/dss_install/conf` 目录下的 `config.sh` 和 `db.sh` 进行修改。


- 打开 `config.sh`，按需修改相关配置参数，参数说明如下：

```properties
### deploy user
deployUser=hadoop

## max memory for services
SERVER_HEAP_SIZE=512M

### The install home path of DSS，Must provided
LINKIS_DSS_HOME=/data/Install/dss_install

DSS_VERSION=1.1.2

DSS_FILE_NAME=dss-1.1.2

DSS_WEB_PORT=8085

###  Linkis EUREKA information.  # Microservices Service Registration Discovery Center
EUREKA_INSTALL_IP=127.0.0.1
EUREKA_PORT=20303
### If EUREKA  has safety verification, please fill in username and password
#EUREKA_USERNAME=
#EUREKA_PASSWORD=

### Linkis Gateway information
GATEWAY_INSTALL_IP=127.0.0.1
GATEWAY_PORT=9001

### Linkis BML Token，需要从Linkis表linkis_mg_gateway_auth_token中获取以BML开头的token_name
BML_AUTH=BML-14ddab34fbd640afa523dd8d1496c3c3

################### The install Configuration of all Micro-Services start #####################
#
#    NOTICE:
#       1. If you just wanna try, the following micro-service configuration can be set without any settings.
#            These services will be installed by default on this machine.
#       2. In order to get the most complete enterprise-level features, we strongly recommend that you install
#          the following microservice parameters
#

### DSS_SERVER
### This service is used to provide dss-server capability.
### dss-server
DSS_SERVER_INSTALL_IP=127.0.0.1
DSS_SERVER_PORT=9043

### dss-apps-server
DSS_APPS_SERVER_INSTALL_IP=127.0.0.1
DSS_APPS_SERVER_PORT=9044
################### The install Configuration of all Micro-Services end #####################


############## ############## dss_appconn_instance configuration start ############## ##############
####eventchecker表的地址，一般就是dss数据库
EVENTCHECKER_JDBC_URL=jdbc:mysql://172.16.16.16:3305/xxx?characterEncoding=UTF-8
EVENTCHECKER_JDBC_USERNAME=xxx
EVENTCHECKER_JDBC_PASSWORD=xxx

#### hive地址
DATACHECKER_JOB_JDBC_URL=jdbc:mysql://172.16.16.10:3306/xxx?useUnicode=true
DATACHECKER_JOB_JDBC_USERNAME=xxx
DATACHECKER_JOB_JDBC_PASSWORD=xxx
#### 元数据库，可配置成和DATACHECKER_JOB的一致
DATACHECKER_BDP_JDBC_URL=jdbc:mysql://172.16.16.10:3306/xxx?useUnicode=true
DATACHECKER_BDP_JDBC_USERNAME=xxx
DATACHECKER_BDP_JDBC_PASSWORD=xxx

### 邮件节点配置
EMAIL_HOST=smtp.163.com
EMAIL_PORT=25
EMAIL_USERNAME=xxx@163.com
EMAIL_PASSWORD=xxxxx
EMAIL_PROTOCOL=smtp
############## ############## dss_appconn_instance configuration end ############## ##############
```

### 五、安装和使用

1. #### 停止机器上所有DSS服务

- 若从未安装过DSS服务，忽略此步骤

2. #### 将当前目录切换到bin目录
    ```shell
    cd xx/dss_install/bin
    ```
3. #### 执行安装脚本
    ```shell
    sh install.sh
    ```
- 该安装脚本会检查各项集成环境命令，如果没有请按照提示进行安装，以下命令为必须项：

  *yum; java; mysql; unzip; expect; telnet; tar; sed; dos2unix; nginx*

- 安装时，脚本会询问您是否需要初始化数据库并导入元数据，**第一次安装必须选是**

- 通过查看控制台打印的日志信息查看是否安装成功，如果有错误信息，可以查看具体报错原因
- *除非用户想重新安装整个应用，否则该命令执行一次即可*

4. #### 启动服务
- 在xx/dss_install/dss/sbin目录下执行启动服务脚本

    ```shell
    sh dss-start-all.sh
    ```
- 如果启动产生了错误信息，可以查看具体报错原因。启动后，各项微服务都会进行**通信检测**，如果有异常则可以帮助用户定位异常日志和原因
- 通过命令`sudo nginx`启动nginx，若nginx已在启动中，则通过命令`sudo nginx -s reload`重启nginx

5. #### 安装默认Appconn

   ```shell
   # 切换目录到dss，正常情况下dss目录就在xx/dss_install目录下，
   cd xx/dss_install/dss/bin
   
   # 执行启动默认Appconn脚本
   sh install-default-appconn.sh
   ```

- *该命令执行一次即可，除非用户想重新安装整个应用*

6. #### 查看验证是否成功

- 用户可以在Linkis的Eureka界面查看 DSS 后台各微服务的启动情况，默认情况下DSS有2个微服务

- 用户可以使用**谷歌浏览器**访问以下前端地址：`http://DSS_NGINX_IP:DSS_WEB_PORT` **启动日志会打印此访问地址（在xx/dss_install/conf/config.sh中也配置了此地址）**。登陆时默认管理员的用户名和密码均为部署用户为hadoop（用户若做了修改，可以查看xx/linkis/conf/linkis-mg-gateway.properties 文件中的 wds.linkis.admin.password 参数)

7. #### 停止服务
    ```shell
    sh dss-stop-all.sh
    ```
- 若用户需要停止所有服务可执行该命令`sh stop-all.sh`，重新启动所有服务就执行`sh start-all.sh`，这两条命令均在xx/dss_install/bin目录下执行

8. #### 在linkis中配置appconn engine plugin
- 将下载的dss-enginplugin-appconn.zip包放到linkis的xxx/lib/linkis-engineconn-plugins目录，解压并将文件夹重命名为appconn
- 若是自行编译的DSS后端，那么可以在xxx/DataSphereStudio/dss-appconn/linkis-appconn-engineplugin/target获取到该zip包
- 通过重启linkis cg-engineplugin 服务刷新引擎

### 六、补充说明
- DSS默认未安装调度系统，用户可以选择安装 Schedulis 或者 DolphinScheduler，具体安装方式见下面表格
- DSS默认仅安装DateChecker, EventSender, EventReceiver AppConn，用户可参考文档安装其他AppConn，如Visualis, Exchangis, Qualitis, Prophecis, Streamis。调度系统可使用Schedulis或DolphinScheduler

  | 组件名      | 组件版本要求   | 组件部署链接                                   | AppConn部署链接 |
    |-----------------|----------------|----------------------------------------|-------------------|
  | Schedulis | Schedulis0.7.0 | [Schedulis部署](https://github.com/WeBankFinTech/Schedulis/blob/master/docs/schedulis_deploy_cn.md) | [Schedulis AppConn安装](SchedulisAppConn插件安装文档.md)|
  | Visualis | Visualis1.0.0  | [Visualis部署](https://github.com/WeBankFinTech/Visualis/blob/master/visualis_docs/zh_CN/Visualis_deploy_doc_cn.md) |[Visualis AppConn安装](https://github.com/WeBankFinTech/Visualis/blob/master/visualis_docs/zh_CN/Visualis_appconn_install_cn.md)|
  | Exchangis | Exchangis1.0.0 | [Exchangis部署](https://github.com/WeBankFinTech/Exchangis/blob/master/docs/zh_CN/ch1/exchangis_deploy_cn.md) | [Exchangis AppConn安装](https://github.com/WeBankFinTech/Exchangis/blob/master/docs/zh_CN/ch1/exchangis_appconn_deploy_cn.md) |
  | Qualitis |Qualitis0.9.2 | [Qualitis部署](https://github.com/WeBankFinTech/Qualitis/blob/master/docs/zh_CN/ch1/%E5%BF%AB%E9%80%9F%E6%90%AD%E5%BB%BA%E6%89%8B%E5%86%8C%E2%80%94%E2%80%94%E5%8D%95%E6%9C%BA%E7%89%88.md) |[Qualitis AppConn安装](https://github.com/WeBankFinTech/Qualitis/blob/master/docs/zh_CN/ch1/%E6%8E%A5%E5%85%A5%E5%B7%A5%E4%BD%9C%E6%B5%81%E6%8C%87%E5%8D%97.md) |
  | Prophecis  | Prophecis0.3.2 | [Prophecis部署](https://github.com/WeBankFinTech/Prophecis/blob/master/docs/zh_CN/QuickStartGuide.md) | [Prophecis AppConn安装](https://github.com/WeBankFinTech/Prophecis/blob/master/docs/zh_CN/Deployment_Documents/Prophecis%20Appconn%E5%AE%89%E8%A3%85%E6%96%87%E6%A1%A3.md) |
  | Streamis  | Streamis0.2.0 | [Streamis部署](https://github.com/WeBankFinTech/Streamis/blob/main/docs/zh_CN/0.2.0/Streamis%E5%AE%89%E8%A3%85%E6%96%87%E6%A1%A3.md) | [Streamis AppConn安装](https://github.com/WeBankFinTech/Streamis/blob/main/docs/zh_CN/0.2.0/development/StreamisAppConn%E5%AE%89%E8%A3%85%E6%96%87%E6%A1%A3.md) |
  | DolphinScheduler | DolphinScheduler1.3.x | [DolphinScheduler部署](https://dolphinscheduler.apache.org/zh-cn/docs/1.3.8/user_doc/standalone-deployment.html) | [DolphinScheduler AppConn安装](DolphinScheduler插件安装文档.md) | 
