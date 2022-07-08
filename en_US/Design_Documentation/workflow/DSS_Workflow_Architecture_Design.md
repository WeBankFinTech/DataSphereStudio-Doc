## DSS workflow  architecture design

Workflow : refers to "the automation of part or the whole of the business process in the computer application environment". It is an abstract and general description of the business rules between the workflow and its operation steps. Data development work can be greatly facilitated by using workflow.

### 1. Workflow Architecture

![workflow](images/workflow.png)



### 2. the introduction of the second module

##### dss-workflow-server

The workflow core module includes the workflow CRUD function, the workflow publishing function, the CRUD function of the workflow appconn node, the workflow cs service function, and the PPC external service function.

| Core service | Core function |
| --------- | -------- |
| DSSFlowService | Contains workflow and sub-workflow CRUD, as well as workflow version management methods |
| WorkflowNodeService | Contains workflow node CRUD, copy, import, export and other methods |
| PublishService | Provides functions related to workflow publishing |
| DSSWorkflowReceiver | PRC Task Call Receiver |
| DSSWorkflowChooser | PRC Task Invocation Selector |

##### dss-flow-execution-server

The workflow execution module includes functions related to workflow execution, including real-time workflow execution, selected execution, rerun on failure, and workflow kill.

##### dss-workflow-sdk

The workflow toolkit module provides external workflow resource file parsing functions.

| Core Class | Core Function                               |
| ---------------- | -------------------------------------------- |
| FlowToJsonParser | Used to parse a flow into a DSSJsonFlow that DSS can use |
| json2flow        | Used to parse the workflow json into the required workflow object       |

##### dss-workflow-common

Workflow basic public module, which abstracts the public entity class and saves it in this module.

##### dss-linkis-node-execution

dss calls linkis to execute the node module, which implements the task execution related interfaces provided by linkis.

| Core Class | Core Function                                                  |
| ----------------------- | ------------------------------------------------------------ |
| LinkisNodeExecution     | Contains core methods such as task running, task status, task results, task cancellation, and task logs |
| LinkisExecutionListener | Monitor task execution                                             |
| DefaultRetryHandler     | Provide a retry mechanism                                                 |

##### dss-workflow-conversion-standard

The workflow transformation specification module defines the specification for transforming the DSS workflow into a workflow that can be used by other external applications.

| Core Class | Core Function       |
| ------------------------------------- | -------------- |
| ProjectConversionIntegrationStandard  | Engineering Conversion Specification   |
| WorkflowConversionIntegrationStandard | Workflow Transformation Specification |

