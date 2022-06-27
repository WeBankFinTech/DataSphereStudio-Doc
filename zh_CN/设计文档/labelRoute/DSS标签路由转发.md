# DSS基于标签的路由转发

## 引言

标签是DSS1.0新引入的概念，在DSS框架中有很多地方使用了标签，作为转发路由的依据。比如在DSS的多环境支持中，每个请求都是带有环境标签的，如DEV、PROD等。这些可以用来确定当前请求是发送给哪个服务，或者当前应该使用哪一个AppConn Instance。

## 标签的格式

route类型标签

```
labels={"route":"dev"}
```

Get类型请求

```
dssurl?labels=dev
```

上面两种是比较常见的DSS中用于Label的格式

## 基于标签的路由转发

1、服务的转发

服务的转发是指将HTTP请求基于标签的匹配转发给对应的服务实例，当前主要由DSS-GateWay来实现，通过定义DSS-GateWay插件，部署到Linkis-GateWay的lib目录下，就可以实现将DSS的请求转发给DSS的服务实例。

* 直接从请求中获取标签，当前的DSS请求中都包含了类似DEV或者PROD的环境标签，GET类型的请求直接在URl的参数中，POST类型的请求则放在请求的body中，以json的格式进行提交。DSS-GateWay在拦截到发送给DSS的请求后，就会解析请求中带的标签和服务名称。然后根据服务名称和标签，去注册中心如Euraka等查找对应的服务实例，按照匹配规则，当完全匹配服务名时优先发送给该服务实例，有多个同级别的则随机选中一个进行发送。当不能完全匹配时，则发送给匹配相近度最高的服务实例，当完全没有匹配时，则发送给默认的DSS服务实例project。
  
```
object DSSGatewayParser {
    //DSS的URL提取正则匹配规则，可以参考DSS的前端请求URL进行分析
  val DSS_HEADER = normalPath(API_URL_PREFIX) + "rest_[a-zA-Z][a-zA-Z_0-9]*/(v\\d+)/dss/"
  val DSS_URL_REGEX = (DSS_HEADER + "([^/]+)/" + "([^/]+)/.+").r
  val DSS_URL_DEFAULT_REGEX = (DSS_HEADER + "([^/]+).+").r
  
  //该方法主要用于AppConn的应用，如Visualis的url提取
  val APPCONN_HEADER = normalPath(API_URL_PREFIX) + "rest_[a-zA-Z][a-zA-Z_0-9]*/(v\\d+)/([^/]+)/"
  val APPCONN_URL_DEFAULT_REGEX = (APPCONN_HEADER + "([^/]+).+").r

}
```

* RouteLabelParser  在应用中引入Linkis的Label相关的依赖，然后在应用的yaml中定义服务的标签名称，那么在服务启动后，就会带上标签的元数据信息向注册中心注册，这是每个服务在注册中心都是有标签的，也可能存在多个标签的情况。然后在Linkis的GateWay中实现RouteLabelParser，并在GateWay拦截到请求后，在parse中解析出相关的route类型标签列表，再由Linkis-gateway基于标签的转发规则进行请求转发。这个是另外一种基于标签的转发，要求使用Linkis中标签模块。

```
    <dependency>
        <groupId>org.apache.linkis</groupId>
        <artifactId>linkis-instance-label-client</artifactId>
        <version>${linkis.version}</version>
    </dependency>
```

2、AppConn Instance转发

在DSS1.0中使用了AppConn的概念，并替换了原来的AppJoint，以AppConn的方式集成第三方系统。且AppConn最大特点就是能支持多应用实例，每个应用实例都有自己的标签。在数据库中新增一张表dss_appconn_instance的表：

```
CREATE TABLE `dss_appconn_instance` (
  `id` int(20) NOT NULL AUTO_INCREMENT COMMENT '主键',
  `appconn_id` int(20) NOT NULL COMMENT 'appconn的主键',
  `label` varchar(128) NOT NULL COMMENT '实例的标签',
  `url` varchar(128) DEFAULT NULL COMMENT '访问第三方的url',
  `enhance_json` varchar(1024) DEFAULT NULL COMMENT 'json格式的配置',
  `homepage_uri` varchar(255) DEFAULT NULL COMMENT '主页uri',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=66 DEFAULT CHARSET=utf8mb4 COMMENT='dss instance的实例表';
```

在每个AppConn进行部署的时候，就会在DSS中注册它的的AppConn的Instance信息。并使用上面的数据库表记录该实例对应的标签、访问地址、主页地址、配置信息等。

当每个appConn实例有了标签以后，在DSS的框架中就会在每次和AppConn进行接口交互时，都会根据当前请求的标签信息，查找对应的操作实例对象，再向该对象发送具体的请求。

```
 //从执行参数中获取标签内容
 val labels = engineExecutorContext.getProperties.get("labels").toString
    //根据标签内容和加载的AppConn，获取对应的appconn实例
    getAppInstanceByLabels(labels, appConn) match {
      case Some(appInstance) =>
        val developmentIntegrationStandard = appConn.asInstanceOf[OnlyDevelopmentAppConn].getOrCreateDevelopmentStandard
        //根据实例信息，从开发规范中获取对应的执行服务
        val refExecutionService = developmentIntegrationStandard.getRefExecutionService(appInstance)
```

DSS中标签请使用EnvDSSLabel，包含标签的key和Value。继承自linkis中标签体系GenericLabel。

```
//构建一个DSS的标签对象，默认的key为：DSSEnv
DSSLabel envDSSLabel = new EnvDSSLabel(rollbackOrchestratorRequest.getLabels().getRoute());
```

