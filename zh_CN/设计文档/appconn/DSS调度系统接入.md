# DSS接入调度系统

## 背景

目前应用于大数据领域的批量定时调度系统有很多，如Azkaban、Dophinscheduler、Airflow等，DataSphereStudio(DSS)支持将用户设计好的工作流，发布到不同的调度系统进行调度，当前默认支持了发布到Azkaban。用户在DSS中完成了工作流的DAG设计，包含工作流资源文件加载、工作流参数设置，数据导入、数据质量检查、节点代码编写，可视化报表设计、邮件输出、节点资源文件上传和运行参数设置等操作后，可在DSS中调试执行，验证所有节点的可执行正确后，发布到调度系统 ，由调度系统根据定时任务的配置，定时调度执行。

### 发布方式

DSS集成新的调度系统需要使用AppConn的方式接入，用户需要根据不同调度系统定义对应的XXXSchedulerAppConn, 在SchedulerAppConn中定义了转换集成规范和结构化集成规范。转换集成规范包含DSS工程级别内容和DSS工作流级别内容到第三方调度系统的转换。DSS同调度系统的接入可以分为以下两种：

1、工程级别发布

是指将工程内的所有工作流进行转换，并把转换后的内容统一打包上传给调度系统。主要有ProjectPreConversionRel接口，定义了工程内需要转换的工作流。

2、工作流级别发布

是指按照工作流的粒度进行转换，只打包工作流的内容上传给调度系统。当前DSS的工作流定义都是以Json的方式存储在BML文件中，工作流的元数据信息则存储在数据库中。


## 主要步骤

### Parser

JsonToFlowParser   用于将工作流的Json转换成Workflow,Workflow是DSS中操作工作流的标准格式，包含了工作流的节点信息，工作流的边信息、父工作流、子工作流、工作流资源文件、工作流属性文件、工作流创建时间、更新时间、工作流用户、工作流代理用户、工作流的元数据信息如名称、ID、描述、类型、是否根工作流等。这些都是根据Json的内容进行解析，转成DSS可操作的Workflow对象，如AzkabanWorkflow、DolphinSchedulerWorkflow。

### Converter

把DSS的Workflow转成接入调度系统可以识别的工作流, 每个调度系统对于工作流都有自己的定义。如果把DSS工作流的节点转成Azkaban的job格式文件，或者转成DolphinScheduler的task, 也可以反过来转换，将调度系统的工作流转成DSS可以加载和展示的工作流，把工作流的依赖关系，节点的连线转成对应调度系统的依赖。还可在Converter中检查该项目下的工作流节点是否存在重名的节点，如在Azkaban的调度系统中是不允许使用重名节点的。

WorkflowConVerter 定义工作流转换输出目录结构，包括工作流的存储目录、工作流资源文件存储、子工作流存储目录建立等。如Azkaban在工程级别的转换操作中还包括建立项目转换的目录，并根据工程内的工作流情况建立工作流的转换目录。在convertToRel中实现把Workflow转成dolphinSchedulerWorkflow或者SchedulisWorkFlow

NodeConverter 定义节点转换输出内容：如Azkaban的ConvertNode,会把工作流的节点内容转成对应的Job文件内容。包括转换节点的名称、类型、依赖关系、节点的执行命令（依赖linkis-jobtype解析）、节点的配置参数、节点的标签等内容。最终按照Job文件定义的格式进行存储。DolphinScheduler的Converter将DSS中节点转为 DolphinScheduler 中 task，并构建Shell类型Task的执行脚本，将DSS的节点内容转成自定义的dss-dolphinscheduler-client.sh脚本执行所需要的参数。

```--java
            addLine.accept("LINKIS_TYPE", dssNode.getNodeType());   //工作流节点类型
            addLine.accept("PROXY_USER", dssNode.getUserProxy());   //代理用户
            addObjectLine.accept("JOB_COMMAND", dssNode.getJobContent()); //执行命令
            addObjectLine.accept("JOB_PARAMS", dssNode.getParams());      //节点执行参数
            addObjectLine.accept("JOB_RESOURCES", dssNode.getResources()); //节点执行资源文件
            addObjectLine.accept("JOB_SOURCE", sourceMap);                 //节点的source信息
            addLine.accept("CONTEXT_ID", workflow.getContextID());         //上下文ID
            addLine.accept("LINKIS_GATEWAY_URL", Configuration.getGateWayURL());  //linkis的gateway地址
            addLine.accept("RUN_DATE", "${system.biz.date}");                    //运行日期变量
```

### Tunning

用于完成工程发布前的整体调整操作，在Azkaban的实现 中主要完成了工程的路径设置和工作流的存储路径设置。因为这个时候是可以操作工程=》工作流=》子工作流，便于进行从外到里的设置操作。比如工作流的存储依赖于工程的存储位置，子工作流存储依赖于父工作流的位置。在FlowTuning中完成了子节点计算，自动添加末尾节点。

## 调度AppConn实现

### AbstractSchedulerAppConn

调度AppConn的抽象类，新的调度系统AppConn接入可以直接继承该抽象类，它实现了SchedulerAppConn接口，并继承了AbstractOnlySSOAppConn，打通DSS与调度系统的SSO登录。比如已经集成的DolphinSchedulerAppConn和SchedulisAppConn都是继承了该抽象类。

该抽象类中包含了两种类型的Standard

第一个是ConversionIntegrationStandard，用于支持将DSS编排转换为调度系统的工作流

第二个是SchedulerStructureIntegrationStandard，用于DSS和调度系统的结构化集成规范

### ConversionIntegrationStandard

用于调度系统的转换集成规范，包含用于将DSS编排转成调度系统工作流的DSSToRelConversionService。也预留了接口支持把调度系统的工作流转成DSS的编排

### AbstractSchedulerStructureIntegrationStandard

调度系统组织结构集成规范，专门用于调度系统的组织结构管理，主要包含工程服务和编排服务。

### ProjectService

* 实现了工程的统一创建、更新、删除和查重操作。
* 用于打通 DSS 工程与接入的第三方应用工具的工程体系，实现工程的协同管理。
* 如调度系统需要同DSS打通工程体系就需要在结构化集成规范中实现工程服务的所有接口。

### OrchestrationService

编排服务用于调度系统统一编排规范，具有如下作用：

* 统一编排规范，专门用于打通 DSS 与 SchedulerAppConn（调度系统）的编排体系。
* 例如：打通 DSS 工作流 与 Schedulis 工作流。
* 请注意，如果对接的 SchedulerAppConn 系统本身不支持管理工作流，则无需实现该接口。
