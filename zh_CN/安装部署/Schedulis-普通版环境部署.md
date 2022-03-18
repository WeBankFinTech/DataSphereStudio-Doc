## ## Schedulis 普通版环境部署准备

### 一）、使用前置

1. 请基于 Linux 操作系统操作（建议 CentOS）
2. 创建新用户 hadoop， 并为该用户赋予 root 权限,用于部署schedulis
3. 准备好 MySQL（版本5.5+） 的客户端和服务端
4. 请确保已安装并且正确配置 JDK（版本1.8+）
5. 配置集群各节点之间的免密码登录
6. 请准备一台已经正确安装和配置 Maven（版本3.3+） 和 Git 的机器，用来编译代码
7. ==安装dss所需的安装包，以及pstree（shutdown时会用到）==

### 二）、获取项目文件并编译打包

1. 
2. 使用 Git 下载 Schedulis 项目文件 git clone https://github.com/WeBankFinTech/Schedulis.git
3. 下载jobtype插件的依赖和配置，链接: 链接：https://pan.baidu.com/s/1FuSBdgdTAHL1PxUXnfbLBw 提取码：0cpo；（由于文件大小较大，所以放在网盘进行管理）
4. 进入项目文件的根目录下，将第二步中下载的jobtypes文件解压后，将整个jobtypes文件夹放入项目module（azkaban-jobtyope）的根目录，然后使用 Maven 来编译打包整个项目 `mvn clean install -Dmaven.test.skip=true`
   待整个项目编译打包成功后，用户可以在这两个服务(azkaban-web-server 和 azkaban-exec-server)各自的 target 目录下找到相应的 .ZIP 安装包(schedulis_****web.zip 和 schedulis\****_exec.zip)。这里需要注意：打包完成后一定要确认安装包内是否有plugins目录，如发现安装包没有plugins，或者plugins为空，则分别进入 WebServer 和 ExecServer 目录，为它们单独再次编译即可,如果没有打包进来则无法使用插件。
5. 将以下文件复制到需要部署的 Executor 或者 WebServer 服务器:
   - Executor 或者 WebServer 安装包
   - 项目文件根目录下的 bin/construct 目录中的数据库初始化脚本 hdp_wtss_deploy_script.sql
   - 项目文件根目录下的 bin 目录中的环境检测脚本 checkEnv.sh
6. 将安装包解压到合适的安装目录下，譬如：/appcom/Install/AzkabanInstall， 并将安装的根目录 /appcom 以及其下子目录的属主转换为 hadoop 用户， 且赋予 775 权限（/appcom/Install/AzkabnaInstall/ 为默认安装目录，建议创建该路径并将其作为安装路径，可避免一些路径的修改）
7. 在开始下一步操作之前，为需要部署的机器运行 bin 目录下的环境检测脚本 checkEnv.sh，确认基础环境已经准备完成。若是报错，请用户参考"使用前置"章节为部署节点准备好基础环境
8. 如果自行编译schedulis，那么编译出的linkis插件是不可用的，需要自行编译linkis1.0或者直接使用第二步中已经编译好的插件包（jobtypes）放入安装目录的plugins目录下，使用release包没有此问题

==先编译：az-webank-system-manager==

==再编译：schedulis==

### 三）、修改配置

### 1. 修改 host.properties 文件

此配置文件存放的路径请参考或者修改 ExecServer 安装包下的 bin/internal/internal-start-executor.sh 文件中的 KEY 值 hostConf
该文件记录的是 Executor 端的所有执行节点 Hostname 和 ServerId， 需保持每台执行机器上的该文件内容一致

示例：

```
vi /appcom/config/wtss-config/host.properties
```

文件内容如下：

```
executor1_hostname=1
executor2_hostname=2
executor3_hostname=3
```

其中executor1_hostname，executor2_hostname，executor3_hostname 为Executor节点所在机器的真实主机名。

### 2. 初始化数据库

在 MySQL 中相应的 database 中（也可新建一个），将前面复制过来的数据库初始化脚本导入数据库

```
#连接 MySQL 服务端
#eg: mysql -uroot -p12345，其中，username ： root, password: 12345

mysql -uUserName -pPassword -hIP --default-character-set=utf8
#创建一个 Database(按需执行)

mysql> create database schedulis;
mysql> use schedulis; 
# 初始化 Database
#eg: source hdp_wtss_deploy_script.sql

mysql> source 脚本存放目录/hdp_wtss_deploy_script.sql
```

### 3. Executor Server 配置修改

#### 执行包修改

项目文件根目录下的 bin/construct 目录中任务执行依赖的包 execute-as-user ，复制到azkaban-exec-server的lib下，并且更新权限

```
sudo chown root execute-as-user
sudo chmod 6050 execute-as-user
```

#### plugins/jobtypes/commonprivate.properties

此配置文件存放于 ExecServer 安装包下的 plugins/jobtypes 目录下
此配置文件主要设置程序启动所需要加载的一些 lib 和 classpath

```
#以下四项配置指向对应组件的安装目录，请将它们修改成相应的组件安装目录
hadoop.home=/appcom/Install/hadoop
hadoop.conf.dir=/appcom/config/hadoop-config
hive.home=/appcom/Install/hive
spark.home=/appcom/Install/spark

#azkaban.native.lib 请修改成ExecServer 安装目录下 lib 的所在绝对路径
execute.as.user=true
azkaban.native.lib=/appcom/Install/AzkabanInstall/wtss-exec/lib
```

#### plugins/jobtypes/common.properties

此配置文件存放于 ExecServer 安装包下的 plugins/jobtypes 目录下
此配置文件主要是设置 DataChecker 和 EventChecker 插件的数据库地址，如不需要这两个插件可不用配置

```
#配置集群 Hive 的元数据库（密码用 base64 加密）
job.datachecker.jdo.option.name="job"
job.datachecker.jdo.option.url=jdbc:mysql://host:3306/db_name?useUnicode=true&amp;characterEncoding=UTF-8
job.datachecker.jdo.option.username=username
job.datachecker.jdo.option.password=password

#配置 Schedulis 的数据库地址（密码用 base64 加密）
msg.eventchecker.jdo.option.name="msg"
msg.eventchecker.jdo.option.url=jdbc:mysql://host:3306/db_name?useUnicode=true&characterEncoding=UTF-8
msg.eventchecker.jdo.option.username=username
msg.eventchecker.jdo.option.password=password


#此部分依赖于第三方脱敏服务mask，暂未开源，将配置写为和job类型一样即可（密码用 base64 加密） 

bdp.datachecker.jdo.option.name="bdp"
bdp.datachecker.jdo.option.url=jdbc:mysql://host:3306/db_name?useUnicode=true&amp;characterEncoding=UTF-8
bdp.datachecker.jdo.option.username=username
bdp.datachecker.jdo.option.password=password
```

#### conf/azkaban.properties

此配置文件是 ExecServer 的核心配置文件， 该配置文件存放在 ExecServer 安装包下的 conf 目录下。

==executor.global.properties必须是绝对路径，否则会报azkaban-executor启动时出现conf/global.properties (No such file or directory)的错误==

```



# Azkaban
default.timezone.id=Asia/Shanghai

# Azkaban JobTypes Plugins
azkaban.jobtype.plugin.dir=plugins/jobtypes

# Loader for projects
executor.global.properties=/appcom/Install/AzkabanInstall/schedulis_0.5.0_exec/conf/global.properties
azkaban.project.dir=projects


#项目 MySQL 服务端地址（密码用 base64 加密）
database.type=mysql
mysql.port=3306
mysql.host=
mysql.database=
mysql.user=
mysql.password=
mysql.numconnections=100

#Executor 线程相关配置
executor.maxThreads=60
executor.port=12321
executor.flow.threads=30
jetty.headerBufferSize=65536
flow.num.job.threads=30
#此 server id 请参考1的 host.properties，改配置会在服务启动的时候自动从host.properties中拉取
executor.server.id=8
checkers.num.threads=200

#Web Sever url相关配置， eg: http://localhost:18081
azkaban.webserver.url=http://webserver_ip:webserver_port
```

#### plugins/alerter/WeBankIMS/conf/plugin.properties

此配置文件存放在 ExecServer 安装包下的 plugins/alerter/WeBankIMS/conf 目录下
该配置文件主要是设置 Executor 告警插件地址， 请用户基于自己公司的告警系统来设置
此部分依赖于第三方告警服务，如不需要可跳过配置

```
# webank alerter settings
alert.type=WeBankAlerter
alarm.server=ims_ip
alarm.port=10812
alarm.subSystemID=5003
alarm.alertTitle=WTSS Aleter Message
alarm.alerterWay=1,2,3
alarm.reciver=root
alarm.toEcc=0
```

#### conf/global.properties

该配置文件存放在 ExecServer 安装包下的 conf 目录下，该配置文件主要存放一些 Executor 的全局属性

```
#azkaban.native.lib，执行项目的 lib 目录，请修改成本机解压后的 ExecServer 安装包下 lib 的所在路径
execute.as.user=true
azkaban.native.lib=/appcom/Install/AzkabanInstall/wtss-exec/lib
```

#### plugins/jobtypes/linkis/private.properties

该配置文件存放在 ExecServer 安装包下的 plugins/jobtypes/linkis 目录下，主要是设置 jobtype 所需的 lib 所在位置

```
#将该值修改为 ExecServer 安装包目录下的 /plugins/jobtypes/linkis/extlib
jobtype.lib.dir=/appcom/Install/AzkabanInstall/wtss-exec/plugins/jobtypes/linkis/extlib
```

#### plugins/jobtypes/linkis/plugin.properties （按需修改）

若用户安装了 Linkis，则修改此配置文件来对接 Linkis，该配置文件存放在 ExecServer 安装包下的 plugins/jobtypes/linkis 目录下

```
#将该值修改为 Linkis 的gateway地址
wds.linkis.gateway.url=
```

### 4. Web Server 配置文件

#### conf/azkaban.properties

此配置文件是 WebServer 的核心配置文件， 该配置文件存放在 WebServer 安装包下的 conf 目录下

```
#项目 MySQL 配置（密码用 base64 加密）
database.type=mysql
mysql.port=
mysql.host=
mysql.database=
mysql.user=
mysql.password=
mysql.numconnections=100

#Azkaban jetty server properties
jetty.port=18081

#Executor 选择策略配置
azkaban.use.multiple.executors=true
azkaban.executorselector.filters=StaticRemainingFlowSize
azkaban.queueprocessing.enabled=true
azkaban.webserver.queue.size=100000
azkaban.activeexecutor.refresh.milisecinterval=50000
azkaban.activeexecutor.refresh.flowinterval=5
azkaban.executorinfo.refresh.maxThreads=5
azkaban.executorselector.comparator.Memory=3
#azkaban.executorselector.comparator.CpuUsage=2
azkaban.executorselector.comparator.LastDispatched=1
azkaban.executorselector.comparator.NumberOfAssignedFlowComparator=1

# LDAP 地址配置
ladp.ip=ldap_ip
ladp.port=ldap_port
```

```

# 如下地址改为绝对地址，如果设置错误登录界面会变形，所有前端请求路径会发生错误
web.resource.dir=/appcom/Install/AzkabanInstall/schedulis_0.5.0_web/web/

```



### 5. 修改日志存放目录（按需修改）

Schedulis 项目的日志默认存放路径为 /appcom/logs/azkaban, 目录下存放的就是 Executor 和 Web 两个服务相关的日志
若选择使用默认存放路径，则需要按要求将所需路径提前创建出来， 并将文件属主转换为 hadoop，赋予 775 权限；若要使用自定义的日志存放路径，则需要创建好自定义路径，并修改 ExecServer 和 WebServer 安装包的以下文件：

1. Executor 下的 bin/internal/internal-start-executor.sh 和 Web 下的 bin/internal/internal-start-web.sh 文件中的 KEY 值 logFile， 设为自定义日志存放路径, 以及在两个文件中关于 “Set the log4j configuration file” 中的 -Dlog4j.log.dir 也修改为自定义的日志路径
2. 两个服务中的 bin/internal/util.sh 文件中的 KEY 值 shutdownFile，改为自定义日志路径

### 6.密码加密问题

```
azkaban.base64Pwd
Base 64 加密后：amVyZWgxMjM=
Base 64 解密后：abcd123

sun.misc.BASE64 加密后：amVyZWgxMjM=
sun.misc.BASE64 解密后：abcd123
```



```
package azkaban;

import sun.misc.BASE64Decoder;
import sun.misc.BASE64Encoder;

import java.util.Base64;


public class base64Pwd {
    public static void main(String[] args) throws Exception {
        String str = "abcd123";

        base64(str);

        enAndDeCode(str);

    }


    /**
     * Base64
     *
     */
    public static void base64(String str) {
        byte[] bytes = str.getBytes();

        //Base64 加密
        String encoded = Base64.getEncoder().encodeToString(bytes);
        System.out.println("Base 64 加密后：" + encoded);

        //Base64 解密
        byte[] decoded = Base64.getDecoder().decode(encoded);

        String decodeStr = new String(decoded);
        System.out.println("Base 64 解密后：" + decodeStr);

        System.out.println();


    }

    /**
     * BASE64加密解密
     */
    public static void enAndDeCode(String str) throws Exception {
        String data = encryptBASE64(str.getBytes());
        System.out.println("sun.misc.BASE64 加密后：" + data);

        byte[] byteArray = decryptBASE64(data);
        System.out.println("sun.misc.BASE64 解密后：" + new String(byteArray));
    }

    /**
     * BASE64解密
     * @throws Exception
     */
    public static byte[] decryptBASE64(String key) throws Exception {
        return (new BASE64Decoder()).decodeBuffer(key);
    }

    /**
     * BASE64加密
     */
    public static String encryptBASE64(byte[] key) throws Exception {
        return (new BASE64Encoder()).encodeBuffer(key);
    }

}

```

## 五、启动

对数据库进行初始化完毕，以及修改完以上的配置文件后，就可以启动了

1. 进入 ExecutorServer 安装包路径，注意不要进到 bin 目录下

```
./bin/start-exec.sh
```

1. 进入 WebServer 安装包路径，注意不要进到 bin 目录下

```
./bin/start-web.sh
```

此时若得到提示信息说启动成功，则可以进入验证环节了；若是出错，请查看日志文件，并按需先查看 QA 章节

## 六、测试

1. 若是单 WebServer 部署模式，则在浏览器中输入 http://webserver_ip:webserver_port
2. 若是多 WebServer 部署模式，则在浏览器中输入 http://nginx_ip:nginx_port
3. 在跳出的登陆界面输入默认的用户名和密码
   username : superadmin
   pwd : Abcd1234
4. 成功登陆后，请参考用户使用手册，自己创建一个项目并上传测试运行
5. 运行成功，恭喜 Schedulis 成功安装了



修改web访问端口

```
vim /appcom/Install/AzkabanInstall/schedulis_0.5.0_web/conf/azkaban.properties

# Azkaban Jetty server properties.
jetty.use.ssl=false
jetty.maxThreads=25
jetty.ssl.port=8443
jetty.port=18080
jetty.send.server.version=false
jetty.keystore=keystore/azkabanjetty.keystore
jetty.password=hadoop
jetty.keypassword=hadoop
jetty.truststore=keystore/azkabanjetty.keystore
jetty.trustpassword=hadoop

```

修改executor的配置

```
vim /appcom/Install/AzkabanInstall/schedulis_0.5.0_exec/conf/azkaban.properties

# Web Server
zakaban.webserver.url=http://localhost:18080
```

![image-20220318190904490](https://s2.loli.net/2022/03/18/JwjfEId6NTC4KPy.png)

![image-20220318190927410](https://s2.loli.net/2022/03/18/3XJ87GtQbKRYOqw.png)



## 七、help

1. [azkaban-executor启动时出现conf/global.properties (No such file or directory)的问题解决](https://www.cnblogs.com/zlslch/p/7124229.html)

2. [maven跳过单元测试-maven.test.skip和skipTests的区别](https://www.cnblogs.com/javabg/p/8026881.html)

3. [^M问题的原因与解决](https://blog.csdn.net/ygy162/article/details/105364096?spm=1001.2101.3001.6650.5&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-5.pc_relevant_antiscanv2&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-5.pc_relevant_antiscanv2&utm_relevant_index=9)

4. 登录异常，前端调试资源路径不对

   需要修改web项目中的azkaban.properties中的web.resource.dir为绝对地址

5. 登录异常，前端登录无响应，控制态报opendj 的jar包错误

   需要确保 opendj-core-3.0.0.jar 和 abcd-grizzly-3.0.0.jar 正确，版本不能变。



参考官方部署文档：[Schedulis/schedulis_deploy_cn.md at master · WeBankFinTech/Schedulis (github.com)](https://github.com/WeBankFinTech/Schedulis/blob/master/docs/schedulis_deploy_cn.md)

参考官方使用教程：[Schedulis/schedulis_user_manual_cn.md at master · WeBankFinTech/Schedulis (github.com)](https://github.com/WeBankFinTech/Schedulis/blob/master/docs/schedulis_user_manual_cn.md)