# DSS label-based routing forwarding

## introduction

Labels are a newly introduced concept in DSS1.0. Labels are used in many places in the DSS framework as the basis for forwarding routes. For example, in the multi-environment support of DSS, each request is labeled with an environment, such as DEV, PROD, etc. These can be used to determine which service the current request is being sent to, or which AppConn Instance should currently be used.

## Format of the label

route type label

```
labels={"route":"dev"}
```

Get type request

```
dssurl?labels=dev
```

The above two are the more common formats used for Label in DSS

## Label-based routing forwarding

1. Service forwarding

Service forwarding refers to forwarding HTTP requests based on tag matching to the corresponding service instance. Currently, it is mainly implemented by DSS-GateWay. By defining the DSS-GateWay plug-in and deploying it to the lib directory of Linkis-GateWay, DSS can be implemented. The request is forwarded to the DSS service instance.

* Get the tag directly from the request. The current DSS request contains an environment tag like DEV or PROD. The GET type request is directly in the URl parameter, and the POST type request is placed in the body of the request, with json format to submit. After DSS-GateWay intercepts the request sent to DSS, it will parse the label and service name in the request. Then, according to the service name and label, go to the registration center such as Euraka to find the corresponding service instance. According to the matching rules, when the service name is completely matched, it will be sent to the service instance first. When there is no complete match, it will be sent to the service instance with the highest matching similarity, and if there is no match at all, it will be sent to the default DSS service instance project.

```
object DSSGatewayParser {
    //The URL extraction regular matching rules of DSS can refer to the front-end request URL of DSS for analysis
  val DSS_HEADER = normalPath(API_URL_PREFIX) + "rest_[a-zA-Z][a-zA-Z_0-9]*/(v\\d+)/dss/"
  val DSS_URL_REGEX = (DSS_HEADER + "([^/]+)/" + "([^/]+)/.+").r
  val DSS_URL_DEFAULT_REGEX = (DSS_HEADER + "([^/]+).+").r
  
  //This method is mainly used in AppConn applications, such as Visualis url extraction
  val APPCONN_HEADER = normalPath(API_URL_PREFIX) + "rest_[a-zA-Z][a-zA-Z_0-9]*/(v\\d+)/([^/]+)/"
  val APPCONN_URL_DEFAULT_REGEX = (APPCONN_HEADER + "([^/]+).+").r

}
```

* RouteLabelParser introduces the Label-related dependencies of Linkis into the application, and then defines the label name of the service in the yaml of the application. After the service is started, the metadata information of the label will be registered with the registration center. Registries are all tagged, and there may be multiple tags. Then implement RouteLabelParser in Linkis's GateWay, and after GateWay intercepts the request, parse out the relevant route type label list in parse, and then forward the request by Linkis-gateway's label-based forwarding rules. This is another tag-based forwarding that requires the use of the tag module in Linkis.

```
    <dependency>
        <groupId>org.apache.linkis</groupId>
        <artifactId>linkis-instance-label-client</artifactId>
        <version>${linkis.version}</version>
    </dependency>
```

2、AppConn Instance Forward

In DSS1.0, the concept of AppConn is used, and the original AppJoint is replaced, and the third-party system is integrated in the way of AppConn. And the biggest feature of AppConn is that it can support multiple application instances, and each application instance has its own label. Add a new table dss_appconn_instance to the database:

```
CREATE TABLE `dss_appconn_instance` (
  `id` int(20) NOT NULL AUTO_INCREMENT COMMENT 'primary key',
  `appconn_id` int(20) NOT NULL COMMENT 'appconn primary key',
  `label` varchar(128) NOT NULL COMMENT 'instance label',
  `url` varchar(128) DEFAULT NULL COMMENT 'access third-party urls',
  `enhance_json` varchar(1024) DEFAULT NULL COMMENT 'json format configuration',
  `homepage_uri` varchar(255) DEFAULT NULL COMMENT 'homepage uri',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=66 DEFAULT CHARSET=utf8mb4 COMMENT='Instance table of dss instance';
```

When each AppConn is deployed, its Instance information of AppConn will be registered in DSS. And use the above database table to record the label, access address, homepage address, configuration information, etc. corresponding to the instance.

After each appConn instance has a label, in the DSS framework, each time it interacts with AppConn, it will search for the corresponding operation instance object according to the label information of the current request, and then send a specific request to the object. .

```
 //Get label content from execution parameters
 val labels = engineExecutorContext.getProperties.get("labels").toString
    //Obtain the corresponding appconn instance based on the tag content and the loaded AppConn
    getAppInstanceByLabels(labels, appConn) match {
      case Some(appInstance) =>
        val developmentIntegrationStandard = appConn.asInstanceOf[OnlyDevelopmentAppConn].getOrCreateDevelopmentStandard
        //Obtain the corresponding execution service from the development specification according to the instance information
        val refExecutionService = developmentIntegrationStandard.getRefExecutionService(appInstance)
```

Please use EnvDSSLabel for the label in DSS, including the key and value of the label. Inherited from the label system GenericLabel in linkis.

```
//Build a DSS label object, the default key is: DSSEnv
DSSLabel envDSSLabel = new EnvDSSLabel(rollbackOrchestratorRequest.getLabels().getRoute());
```

