# DSS access scheduling system

## background

There are many batch timing scheduling systems currently used in the field of big data, such as Azkaban, Dophinscheduler, Airflow, etc. DataSphere Studio (DSS) supports publishing workflows designed by users to different scheduling systems for scheduling. Currently, the default support for publishing to Azkaban. The user has completed the DAG design of the workflow in DSS, including workflow resource file loading, workflow parameter setting, data import, data quality check, node code writing, visual report design, email output, node resource file upload and running parameter setting After the operation, it can be debugged and executed in DSS. After verifying that the execution of all nodes is correct, it will be released to the scheduling system, and the scheduling system will schedule and execute regularly according to the configuration of the scheduled task.

### release method

The DSS integration new scheduling system needs to be accessed by AppConn. Users need to define the corresponding XXXSchedulerAppConn according to different scheduling systems, and the transformation integration specification and the structured integration specification are defined in the SchedulerAppConn. The transformation integration specification contains the transformation of DSS project-level content and DSS workflow-level content to third-party scheduling systems. The access of DSS to the dispatching system can be divided into the following two types:

1. Engineering level release

It refers to converting all the workflows in the project, and uniformly packaging and uploading the converted content to the scheduling system. There are mainly the ProjectPreConversionRel interface, which defines the workflow that needs to be converted in the project.

2. Workflow level release

It refers to the conversion according to the granularity of the workflow, and only the content of the workflow is packaged and uploaded to the scheduling system. The current workflow definition of DSS is stored in the BML file in the form of Json, and the metadata information of the workflow is stored in the database.

## The main steps

### Parser

JsonToFlowParser is used to convert the Json of the workflow into Workflow. Workflow is the standard format for operating workflow in DSS. It includes the node information of the workflow, the edge information of the workflow, the parent workflow, the child workflow, the workflow resource file, Workflow property file, workflow creation time, update time, workflow user, workflow proxy user, workflow metadata information such as name, ID, description, type, whether it is a root workflow, etc. These are parsed according to the content of Json and converted into DSS operable Workflow objects, such as AzkabanWorkflow and DolphinSchedulerWorkflow.

### Converter

Turn the Workflow of DSS into a workflow that can be recognized by the access scheduling system. Each scheduling system has its own definition for the workflow. If you convert the nodes of the DSS workflow into Azkaban's job format files, or into DolphinScheduler's tasks, you can also convert them in reverse, and convert the workflow of the scheduling system into a workflow that can be loaded and displayed by DSS, and the dependencies of the workflow. , the connection of the node is converted into the dependency of the corresponding scheduling system. You can also check whether the workflow nodes under the project have nodes with the same name in Converter. For example, the use of nodes with the same name is not allowed in Azkaban's scheduling system.

WorkflowConVerter defines the workflow conversion output directory structure, including workflow storage directory, workflow resource file storage, and establishment of sub-workflow storage directory. For example, Azkaban also includes creating a project conversion directory in the project-level conversion operation, and establishing a workflow conversion directory according to the workflow situation in the project. Convert Workflow to dolphinSchedulerWorkflow or ScheduleWorkFlow in convertToRel

NodeConverter defines the output content of node conversion: such as Azkaban's ConvertNode, it will convert the node content of the workflow into the corresponding Job file content. Including the name, type, dependency of the conversion node, the execution command of the node (depending on linkis-jobtype parsing), the configuration parameters of the node, the label of the node, etc. Finally, it is stored in the format defined by the Job file. The Converter of DolphinScheduler converts the nodes in DSS into tasks in DolphinScheduler, and builds the execution script of Shell type Task, and converts the node content of DSS into the parameters required for the execution of the custom dss-dolphinscheduler-client.sh script.

```--java
            addLine.accept("LINKIS_TYPE", dssNode.getNodeType());   //Workflow Node Type
            addLine.accept("PROXY_USER", dssNode.getUserProxy());   //proxy user
            addObjectLine.accept("JOB_COMMAND", dssNode.getJobContent()); //Excuting an order
            addObjectLine.accept("JOB_PARAMS", dssNode.getParams());      //Node execution parameters
            addObjectLine.accept("JOB_RESOURCES", dssNode.getResources()); //Node execution resource file
            addObjectLine.accept("JOB_SOURCE", sourceMap);                 //Node's source information
            addLine.accept("CONTEXT_ID", workflow.getContextID());         //context ID
            addLine.accept("LINKIS_GATEWAY_URL", Configuration.getGateWayURL());  //linkis gateway address
            addLine.accept("RUN_DATE", "${system.biz.date}");                    //run date variable
```

### Tunning

It is used to complete the overall adjustment operation before the project is released. In the implementation of Azkaban, the path setting of the project and the storage path setting of the workflow are mainly completed.Because at this time it is possible to operate the project=》workflow=》sub-workflow，It is convenient for setting operation from outside to inside. For example, the storage of workflow depends on the storage location of the project, and the storage of child workflow depends on the location of the parent workflow. The child node calculation is completed in FlowTuning, and the end node is automatically added.

## Scheduling the AppConn implementation

### AbstractSchedulerAppConn

The abstract class of scheduling AppConn, the new scheduling system AppConn access can directly inherit this abstract class, it implements the SchedulerAppConn interface, and inherits AbstractOnlySSOAppConn, to get through the SSO login between DSS and the scheduling system. For example, the already integrated DolphinSchedulerAppConn and SchedulisAppConn both inherit this abstract class.

This abstract class contains two types of Standard

The first is ConversionIntegrationStandard to support workflows that transform DSS orchestration into a scheduling system

The second is SchedulerStructureIntegrationStandard, a structured integration specification for DSS and scheduling systems

### ConversionIntegrationStandard

Conversion integration specification for scheduling systems, including DSSToRelConversionService for converting DSS orchestration into scheduling system workflows. An interface is also reserved to support converting the workflow of the scheduling system into the orchestration of DSS

### AbstractSchedulerStructureIntegrationStandard

The scheduling system organizational structure integration specification is specially used for the organizational structure management of the scheduling system, mainly including engineering services and orchestration services.

### ProjectService

* The unified creation, update, deletion and duplicate checking operations of the project are realized.
* It is used to open up the engineering system of DSS engineering and access third-party application tools, and realize the collaborative management of engineering.
* If the scheduling system needs to open up the engineering system with DSS, it needs to implement all the interfaces of engineering services in the structured integration specification.

### OrchestrationService

The orchestration service is used for the unified orchestration specification of the scheduling system, and has the following functions:

* Unified orchestration specification, specially used to open up the orchestration system of DSS and SchedulerAppConn (scheduling system).
* For example: connect DSS workflow and Schedules workflow.
* Please note that if the docking SchedulerAppConn system does not support management workflow itself, you do not need to implement this interface.
