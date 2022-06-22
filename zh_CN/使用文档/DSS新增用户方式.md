# DataSphere Studio 新增用户的方式

> DSS默认只提供了管理员账号，用户登录鉴权完全依托于Linkis的用户登录认证体系。本文将详细介绍Linkis目前已经支持的用户登录认证方式，以及如何补充和完善用户环境信息

## 一、基本介绍
1. DSS管理员的用户名为部署用户名，如部署用户为hadoop，则管理员的用户名为hadoop，密码因DSS版本不同而发生变化，具体用户可在[DSS单机部署文档](../安装部署/DSS单机部署文档.md)查看


2. DSS的用户登录鉴权，依托于Linkis的用户登录认证体系。除管理员以外，Linkis还支持以下用户登录认证方式： 
  - 接入LDAP登录
  - Token登录方式
  - 代理用户模式

  不管您接入哪种用户登录认证方式，您都需为所有Linkis用户（除部署用户以外）完善用户环境信息。第五部分详细介绍了如何补充和完善用户环境信息。
  
## 二、接入LDAP登录
- 若用户采用[一键安装部署DSS&Linkis](../安装部署/DSS单机部署文档.md)，那么就需要修改安装包conf目录的config.sh以增加您的LDAP配置，具体如下
```
## 进入到conf目录
cd xx/dss_linkis/conf

## 打开config.sh
vi config.sh

## 修改下面几个参数
LDAP_URL=ldap://localhost:1389/
LDAP_BASEDN=dc=webank,dc=com
LDAP_USER_NAME_FORMAT=cn=%s@xxx.com,OU=xxx,DC=xxx,DC=com
```

## 三、Token登录方式

待补充

## 四、代理用户模式

待补充

## 五、如何为用户完善环境信息

1. 由于DSS & Linkis做了自上而下的多租户隔离，为了使登录的用户可正常使用DSS，还需在linux服务器上面创建对应的Linux用户，具体步骤如下：
- 在所有Linkis & DSS 服务器上创建对应Linux用户。
- 在Hadoop的NameNode创建对应Linux用户。
- 保证Linkis & DSS 服务器上的Linux用户，可正常使用hdfs dfs -ls /等命令，同时该用户需要能正常使用Spark和hive任务， 如：通过spark-sql命令可以启动一个spark application，通过hive命令可以启动一个hive客户端。
- 由于每个用户的工作空间严格隔离，您还需为该用户创建工作空间和HDFS目录，如下：
```shell
## 创建用户工作空间目录和授权
mkdir $WORKSPACE_USER_ROOT_PATH/${NEW_USER}
chmod 750 $WORKSPACE_USER_ROOT_PATH/${NEW_USER}

## 创建用户HDFS目录和授权
hdfs dfs -mkdir $HDFS_USER_ROOT_PATH/${NEW_USER}
hdfs dfs -chown ${NEW_USER}:${NEW_USER} $HDFS_USER_ROOT_PATH/${NEW_USER}
hdfs dfs -chmod 750 $HDFS_USER_ROOT_PATH/${NEW_USER}
```
WORKSPACE_USER_ROOT_PATH和HDFS_USER_ROOT_PATH是您一键安装DSS时，设置的工作空间和HDFS根路径。

如果您没有设置，则默认为：
```shell
WORKSPACE_USER_ROOT_PATH=file:///tmp/linkis
HDFS_USER_ROOT_PATH=hdfs:///tmp/linkis
```
