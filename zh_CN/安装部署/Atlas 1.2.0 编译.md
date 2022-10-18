> **ZK**

```
server.1=ecs-f0cf-0001:2888:3888
server.2=ecs-f0cf-0002:2888:3888
server.3=ecs-f0cf-0003:2888:3888
```





| 主机 | IP         | 节点进程                               |
| ---- | ---------- | -------------------------------------- |
| hd1  | 172.17.0.1 | Master、Zookeeper                      |
| hd2  | 172.17.0.2 | Master-backup、RegionServer、Zookeeper |
| hd3  | 172.17.0.3 | RegionServer、Zookeeper                |
| hd4  | 172.17.0.4 | RegionServer                           |



ES：5.6.4



# Atlas 1.2.0 编译

---

> **基础设施**

```shell
_> export MAVEN_OPTS="-Xms2g -Xmx2g"

_> vi pom.xml
    <hadoop.version>2.7.2</hadoop.version>
    <hbase.version>1.1.2</hbase.version>
    <solr.version>5.5.1</solr.version>
    <kafka.version>1.0.0</kafka.version>
    <elasticsearch.version>5.6.4</elasticsearch.version>
    <kafka.scala.binary.version>2.11</kafka.scala.binary.version>
    <zookeeper.version>3.6.2</zookeeper.version>

_> wget -P /software https://github.com/sass/node-sass/releases/download/v4.14.1/linux-x64-57_binding.node
_> export SASS_BINARY_PATH=/software/linux-x64-57_binding.node
```

> **安装 NodeJS**

```shell
_> curl --silent --location https://rpm.nodesource.com/setup_10.x | bash -
_> yum install -y nodejs
_> npm install -g cnpm --registry=https://registry.npm.taobao.org
```

> **编译**

```shell
_> mvn clean -DskipTests package -Pdist
_> mv $ATLAS_SOURCE_HOME/distro/target/apache-atlas-1.2.0-bin/apache-atlas-1.2.0 $ATLAS_HOME
_> cd $ATLAS_HOME/apache-atlas-1.2.0
```

> **配置环境变量**

```shell
_> vi $ATLAS_HOME/conf/atlas-env.sh
export JAVA_HOME=/usr/local/java
export MANAGE_LOCAL_HBASE=false
export HBASE_CONF_DIR=/usr/local/hbase/conf/
export MANAGE_LOCAL_SOLR=false
export MANAGE_EMBEDDED_CASSANDRA=false
export MANAGE_LOCAL_ELASTICSEARCH=false
```

> **修改 Atlas 配置**

```shell
_> vi $ATLAS_HOME/conf/atlas-application.properties
atlas.cluster.name=primary

atlas.graph.storage.backend=hbase
atlas.graph.storage.hbase.table=apache_atlas_janus

atlas.graph.storage.hostname=ecs-f0cf-0001:2181,ecs-f0cf-0002:2181,ecs-f0cf-0003:2181
atlas.graph.storage.hbase.regions-per-server=1
atlas.graph.storage.lock.wait-time=10000


atlas.EntityAuditRepository.impl=org.apache.atlas.repository.audit.HBaseBasedAuditRepository

atlas.graph.index.search.backend=elasticsearch
atlas.graph.index.search.index-name=janusgraph

#atlas.graph.index.hostname=192.168.0.98:9200
atlas.graph.index.search.hostname=http://192.168.0.98:9200
atlas.graph.index.search.elasticsearch.client-only=true
atlas.graph.index.search.max-result-set-size=150

atlas.hook.hive.synchronous=false
atlas.hook.hive.numRetries=5
atlas.hook.hive.queueSize=10000

#########  Notification Configs  #########
atlas.notification.embedded=false
#atlas.kafka.data=${sys:atlas.home}/data/kafka
atlas.kafka.zookeeper.connect=ecs-f0cf-0001:2181,ecs-f0cf-0002:2181,ecs-f0cf-0003:2181
atlas.kafka.bootstrap.servers=ecs-f0cf-0009:9092
atlas.kafka.zookeeper.session.timeout.ms=60000
atlas.kafka.zookeeper.connection.timeout.ms=30000
atlas.kafka.zookeeper.sync.time.ms=20
atlas.kafka.auto.commit.interval.ms=1000
atlas.kafka.hook.group.id=atlas

atlas.kafka.enable.auto.commit=true
atlas.kafka.auto.offset.reset=earliest
atlas.kafka.session.timeout.ms=30000
atlas.kafka.offsets.topic.replication.factor=1
atlas.kafka.poll.timeout.ms=1000

atlas.notification.create.topics=true
atlas.notification.replicas=1
atlas.notification.topics=ATLAS_HOOK,ATLAS_ENTITIES
atlas.notification.log.failed.messages=true
atlas.notification.consumer.retry.interval=500
atlas.notification.hook.retry.interval=1000

## Server port configuration
atlas.server.http.port=21000

# SSL config
atlas.enableTLS=false

# Authentication config

atlas.authentication.method.kerberos=false
atlas.authentication.method.file=true
atlas.authentication.method.ldap.type=none

#### user credentials file
atlas.authentication.method.file.filename=${sys:atlas.home}/conf/users-credentials.properties


#########  Server Properties  #########
atlas.rest.address=http://192.168.0.98:21000


#########  Entity Audit Configs  #########
atlas.audit.hbase.tablename=atlas_entity_audit
atlas.audit.zookeeper.session.timeout.ms=1000
atlas.audit.hbase.zookeeper.quorum=ecs-f0cf-0001:2181,ecs-f0cf-0002:2181,ecs-f0cf-0003:2181

#########  High Availability Configuration ########
atlas.server.ha.enabled=false

######### Atlas Authorization #########
atlas.authorizer.impl=simple
atlas.authorizer.simple.authz.policy.file=atlas-simple-authz-policy.json

#########  CSRF Configs  #########
atlas.rest-csrf.enabled=true
atlas.rest-csrf.browser-useragents-regex=^Mozilla.*,^Opera.*,^Chrome.*
atlas.rest-csrf.methods-to-ignore=GET,OPTIONS,HEAD,TRACE
atlas.rest-csrf.custom-header=X-XSRF-HEADER

############ Atlas Metric/Stats configs ################
atlas.metric.query.cache.ttlInSecs=900

#########  Gremlin Search Configuration  #########
atlas.search.gremlin.enable=false

```



































