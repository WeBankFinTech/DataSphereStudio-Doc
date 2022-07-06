### DSS Interface Summary

#### FlowEntranceRestfulApi

> Base Info

##### Interface Name：Workflow execution

**Interface Path：**`POST` /api/rest_j/v1/dss/flow/entrance/execute

> Request Param

**Body:**

````json
{
  "isReExecute":""	//must Added parameter, this parameter needs to be added when re-execution fails 
}
````

> Return Parameter

```json
{
  "status":"",    //code status
  "message":"",	  //info
  "data":{
    "taskID":"",  
    "execID":""   
  }
}
```



> Base Info

##### Interface Name：Get workflow status

**Interface Path：**`POST` /api/rest_j/v1/flow/entrance/{id}/status

> Request Param

**Body:**

````json
{
  "id":"", //must
  "taskID":""  
}
````

> Return Parameter

````json
{
  "status":"",   //code status
  "message":"",  //info
  "data":{
    "taskID":"",  
    "execID":""   
  }
}
````



> Base Info

##### Interface Name：end workflow

**Interface Path：**`GET` /api/rest_j/v1/dss/flow/entrance/{id}/killWorkflow

> Request Param

**Body:**

````json
{
  "taskID":""  //must 
}
````

> response parameter

````json
{
  "massage":"",
  "data":{
    "result":"",
    "errorInfo":""
  }
}
````

---

#### FremeworkReleaseRestful

> Base Info

##### Interface Name：Get task progress

**Interface Path：**`GET`/api/rest_j/v1/dss/framework/release/getReleaseStatus

> Request Param

**Body:**

````json
{
  "dssLabel":"",	//must dssLabel
  "releaseTaskId":""	//must Query's release id
}
````

> Response Parameter

````json
{
  "massage":"",
  "data":{
    "status":"",
    "errorMsg":""
  }
}
````



> Base Info

##### Interface Name：Submit a release task

**Interface Path：**`POST` /api/rest_j/v1/dss/framework/release/releaseOrchestrator

> Request Param

**Body:**

```json
{
  "orchestratorId":"",	//must Orchestration mode id
  "orchestratorVersionId":"",	//must Orchestration mode version id
  "comment":"",	//must Post a note
  "dssLabel":""	//must dssLabel
}
```

> Response Parameter

````json
{
  "massage":"",
  "data":""
}
````

---

#### ApiServiceTokenResfulApi

> Base Info

##### Interface Name：token query

**Interface Path：**`GET` /api/rest_j/v1/dss/apiservice/tokenQuery

> Request Param

**Body:**

```json
{
  "apiId":"",
  "user":"",
  "status":"",
  "startDate":"",
  "endDate":"",
  "currentPage":1,
  "pageSize":10
}
```

> Response Parameter

```json
{
  "massage":"",
  "data":{
    "queryList":[
      "id":"",
      "apiId":"",
      "apiVersionId":"",
      "publisher":"",
      "user":"",
      "applyTime":"",
      "duration":"",
      "reason":"",
      "ipWhitelist":"",
      "status":"",
      "caller":"",
      "accessLimit":"",
      "token":"",
      "applySource":""
    ]
  }
}
```



> Base Info

##### Interface Name：Refresh the application form

**Interface Path：**`GET` /api/rest_j/v1/dss/apiservice/approvalRefresh

> Request Param

**Body:**

```json
{
  "approvalNo":"" 
}
```

> Response Parameter

```json
{
  "massage":"",
  "data":{
    "approvalStatus":""
  }
}
```

---

#### ContextServiceRestful

> Base Info

##### Interface Name：table query

**Interface Path：**`POST`/api/rest_j/v1/dss/workflow/tables

> Request Param

**Body:**

```json
{
  "contextID":"",	 //ID
  "nodeName":"",	//node name
  "labels":{
    "route":""
  }
}
```

> Response Parameter

```json
{
  "massage":"",
  "data":{
    "tables":[]
  }
}
```



> Base Info

##### Interface Name：column query

**Interface Path：**`POST`/api/rest_j/v1/dss/workflow/columns

> Request Param

**Body:**

```json
{
  "contextID":"",	 
  "nodeName":"",
  "contextKey":"",
  "labels":{
    "route":""
  }
}
```

> Response Parameter

```json
{
  "massage":"",
  "data":{
    "columns":[]
  }
}
```

---

#### DSSDictionaryRestful

> Base Info

##### Interface Name：data dictionary - get by key

**Interface Path：**`POST`/api/rest_j/v1/dss/framework/workspace/getDicList

> Request Param

**Body:**

```json
{
  "workspaceId":"",	 //must workspace id
  "dicKey":"",	
  "parentKey":""	//parent key 
}
```

> Response Parameter

```json
{
  "massage":"",
  "data":{
    "list":[{
      "id":0,
      "workspaceId":0, //workspace id
      "parentKey":"", //parent key
      "dicName":"", //dictionary name
      "dicNameEn":"", //dictionary english name
      "dicKey":"", //key Equivalent to encoding, the space starts with w_, and the project is p_
      "dicValue":"", //the value corresponding to the key
      "dicValueEn":"", //The value corresponding to the key (English)
      "title":"", //title
      "titleEn":"", //title(english)
      "url":"",
      "urlType":1, //url type: 0-internal system, 1-external system; default is internal
      "icon":"", //icon
      "orderNum":1, //order
      "remark":"", 
      "createUser":"", 
      "createTime":"", 
      "updateUser":"", 
      "updateTime":"" 
    }]
  }
}
```



> Base Info

##### Interface Name：get data dictionary

**Interface Path：**`POST`/api/rest_j/v1/dss/framework/workspace/getDicSecondList

> Request Param

**Body:**

```json
{
  "workspaceId":"",	 //must 
  "dicKey":"",	//key Equivalent to encoding, the space starts with w_, and the project is p_
  "parentKey":""	
}
```

> Response Parameter

```json
{
  "massage":"",
  "data":{
    "list":[],
    "mapList":{}
  }
}
```

---

#### DSSFrameworkOrchestratorRestful

> Base Info

##### Interface Name：Equivalent to encoding, the space starts with w_, and the project is p_

**Interface Path：**`POST`/api/rest_j/v1/dss/framework/orchestrator/createOrchestrator

> Request Param

**Body:**

```json
{
  "orchestratorName":"",	 //must Orchestration name
  "orchestratorMode":"",	//Orchestration patterns such as workflow, composition orchestration, etc.
  "orchestratorWays":[],	//must Orchestration way
  "orchestratorLevel":"",	//Orchestration Importance Levels
  "dssLabels":[],	
  "uses":"",	//Orchestration use
  "description":"",	//must 
  "projectName":"", 
  "workspaceName":"" 
}
```

> Response Parameter

```json
{
  "massage":"",
  "data":{
    "orchestratorId":1
  }
}
```



> Base Info

##### Interface Name：Query all orchestration schemas

**Interface Path：**`POST`/api/rest_j/v1/dss/framework/orchestrator/getAllOrchestrator

> Request Param

**Body:**

```json
{
  "workspaceId":"",	 //must
  "projectId":"",	//must
  "orchestratorMode":"",	//orchestrator type, such as workflow, compositional orchestrator, etc.
  "orchestratorLevel":"",	//Orchestration Importance Levels
  "dssLabels":{"route":""}	//dssLabels is passed in through the front end, mainly used for current environment information
}
```

> Response Parameter

```json
{
  "massage":"",
  "data":{
    "page":[{
      "id":1, //primary key ID
      "workspaceId":"", //workspace id
      "projectId":"", //project id
      "orchestratorId":1, //Orchestration mode id (workflow, the orchestratorId returned by calling the orchestrator service)
      "orchestratorVersionId":1, //Orchestration mode version id (workflow, the orchestratorVersionId returned by calling the orchestrator service)
      "orchestratorName":"", //orchestrator Name
      "orchestratorMode":"", //Orchestration mode, the value obtained is dic_key(parent_key=p_orchestratorment_mode) in dss_dictionary
      "uses":"", 
      "description":"", 
      "createUser":"", 
      "createTime":"", 
      "updateUser":"", 
      "updateTime":"", 
      "orchestratorWays":[], 
      "orchestratorLevel":"", //Orchestration Importance Levels
      "flowEditLockExist":false //Whether the workflow edit lock is exited
    }]
  }
}
```



> Base Info

##### Interface Name：Modify the orchestrator mode

**Interface Path：**`POST`/api/rest_j/v1/dss/framework/orchestrator/modifyOrchestrator

> Request Param

**Body:**

```json
{
  "id":"",	 //must
  "orchestratorName":"",	//must orchestrator Name
  "orchestratorMode":"",	//Choreography type, such as workflow, compositional choreography, etc.
  "orchestratorLevel":"",	//Orchestration Importance Levels
  "uses":"",	//Orchestration use
  "description":"",	//must describe
  "orchestratorWays":[],	//must orchestrator  Ways
  "dssLabels":""	//Current environmental information
}
```

> Response Parameter

```json
{
  "massage":"",
  "data":{
    "orchestratorId":1
  }
}
```

> Base Info

##### Interface Name：delete orchestrator mode

**Interface Path：**`POST`/api/rest_j/v1/dss/framework/orchestrator/deleteOrchestrator

> Request Param

**Body:**

```json
{
  "id":""	 //must
}
```

> Response Parameter

```json
{
  "massage":""
}
```



> Base Info

##### Interface Name：Get a list of orchestration importance levels

**Interface Path：**`GET`/api/rest_j/v1/dss/framework/orchestrator/orchestratorLevels

> Request Param

**Body:**

```json
{}
```

> Response Parameter

```json
{
  "massage":"",
  "data":{
   	"orchestratorLevels":["HIGH","MEDIUM","LOW"],       
   }
}
```

---

#### DSSMigrateRestful

> Base Info

##### Interface Name：Import old DSS project

**Interface Path：**`POST`/api/rest_j/v1/dss/framework/release/importOldDSSProject

> Request Param

**Body:**

```json
{
  "file":""	 //must MultipartFile
}
```

> Response Parameter

```json
{
  "massage":""
}
```



> Base Info

##### Interface Name：Import the workflow exported from the dss1.0 interface

**Interface Path：**`POST`/api/rest_j/v1/dss/framework/release/importworkflow

> Request Param

**Body:**

```json
{
  "workspaceName":"",	 //must workspace Name
  "projectName":"",	//Create without project name
  "projectUser":"",	// Project user, the interface will ensure that this user is added to the permission list
  "flowName":"",
  "file":"" //Import file, MultipartFile type
}
```

> Response Parameter

```json
{
  "massage":"",
  "data":{
    "flowId":"",
    "bmlVersion":""
  }
}
```



> Base Info

##### Interface Name：Export SQL file

**Interface Path：**`GET`/api/rest_j/v1/dss/framework/release/exportOrcSqlFile

> Request Param

**Body:**

```json
{
  "outputFileName":"",	 //Default exportOrc
  "charset":"utf-8",	//encoding default utf-8
  "outputFileType":"zip",	// output file type default zip
  "projectName":"",	//project Name
  "orchestratorId":"", //orchestrator Id
  "orcVersionId":"", //orchestrator version number ID
  "addOrcVersion":false, //Whether to increase the version number
  "labels":"" 
}
```

> Response Parameter

```json
{}
```



> Base Info

##### Interface Name：Batch export all workflows under the project to the specified local directory

**Interface Path：**`POST`/api/rest_j/v1/dss/framework/release/exportallflow

> Request Param

**Body:**

```json
{
  "workspaceName":"",	  
  "projectName":"",	 
  "pathRoot":""  
}
```

> Response Parameter

```json
{
  "message":"",
  "data":{
    "count":0,
    "location":"",
    "serviceInstance":""
  }
}
```

---

#### DSSSideInfoRestful

> Base Info

##### Interface Name：get sidebar

**Interface Path：**`POST`/api/rest_j/v1/dss/framework/workspace/getSideInfos

> Request Param

**Body:**

```json
{
  "workspaceID":""	  
}
```

> Response Parameter

```json
{
  "message":"",
  "data":{
    "presentations":[{
      "name":"",
      "title":"",
      "type":"",
      "contents":[{
        "name":"",
        "title":"",
        "url":0,	//url type: 0-internal system, 1-external system; default is internal
        "icon":""	//icon is the icon representing the Content, if it is empty, there is no
      }]
    }]
  }
}
```

---

#### DSSWorkspacePrivRestful

> Base Info

##### Interface Name：Get the workspace menu

**Interface Path：**`GET`/api/rest_j/v1/dss/framework/workspace/exportOrcSqlFile

> Request Param

**Body:**

```json
{
  "workspaceId":""	 
}
```

> Response Parameter

```json
{
  "message":"",
  "data":{
    "workspaceMenuPrivs":{
      "workspaceId":1,
      "menuPrivVOS":[{
        "id":"",
        "name":"",
        "menuPrivs":[{}]
      }],
      "DSSWorkspaceComponentPrivVO":[{
        "id":"",
        "name":"",
        "componentPrivs":[{}]
      }],
      "DSSWorkspaceRoleVO":[{
        "roleId":"",
        "roleName":"",
        "roleFrontName":""
      }]
    }
  }
}
```



> Base Info

##### Interface Name：Get workspace settings

**Interface Path：**`GET`/api/rest_j/v1/dss/framework/workspace/getWorkspaceHomepageSettings

> Request Param

**Body:**

```json
{
  "workspaceId":""	 //工作workspace id
}
```

> Response Parameter

```json
{
  "message":"",
  "data":{
    "homepageSettings":{
      "roleName":"",
      "homepageName":"",
      "homepageUrl":"",
      "roleId":1,
      "roleFrontName":""
    }
  }
}
```



> Base Info

##### Interface Name：Update role menu permissions

**Interface Path：**`POST`/api/rest_j/v1/dss/framework/workspace/updateRoleMenuPriv

> Request Param

**Body:**

```json
{
  "menuId":"",	  
  "workspaceId":"",	 
  "menuPrivs":"" //menu permissions
}
```

> Response Parameter

```json
{
  "message":"" 
}
```



> Base Info

##### Interface Name：Update component permissions

**Interface Path：**`POST`/api/rest_j/v1/dss/framework/workspace/updateRoleComponentPriv

> Request Param

**Body:**

```json
{
  "componentId":"",	  
  "workspaceId":"",	 
  "componentPrivs":"" //component permissions
}
```

> Response Parameter

```json
{
  "message":"",
  "data":{
    "updateRoleComponentPriv":""
  }
}
```

---

#### DSSWorkspaceRestful

> Base Info

##### Interface Name：Create a workspace

**Interface Path：**`POST`/api/rest_j/v1/dss/framework/workspace/createWorkspace

> Request Param

**Body:**

```json
{
  "workspaceName":"",	 
  "department":"",	
  "description":"", 
  "tags":"",	
  "productName":""	
}
```

> Response Parameter

```json
{
  "message":"",
  "data":{
    "workspaceId":1,
    "workspaceName":""
  }
}
```



> Base Info

##### Interface Name：Get department list

**Interface Path：**`GET`/api/rest_j/v1/dss/framework/workspace/listDepartments

> Request Param

**Body:**

```json
{}
```

> Response Parameter

```json
{
  "message":"",
  "data":{
    "departments":[{
      "id":1,
      "name":"",
      "frontName":""
    }]
  }
}
```



> Base Info

##### Interface Name：Get a list of workspaces

**Interface Path：**`GET`/api/rest_j/v1/dss/framework/workspace/getWorkspaces

> Request Param

**Body:**

```json
{}
```

> Response Parameter

```json
{
  "message":"",
  "data":{
    "workspaces":[{
      "id":1,
      "name":"",
      "tags":"",
      "department":"",
      "description":"",
      "product":""
    }]
  }
}
```



> Base Info

##### Interface Name：Get user workspace

**Interface Path：**`GET`/api/rest_j/v1/dss/framework/workspace/getWorkspaceHomePage

> Request Param

**Body:**

```json
{
  "micro_module":"" //module name
}
```

> Response Parameter

```json
{
  "message":"",
  "data":{
    "workspaceHomePage":{
      "workspaceId":1,
      "roleName":"",
      "homePageUrl":"" 
    ]
  }
}
```



> Base Info

##### Interface Name：Get an overview of the workspace

**Interface Path：**`GET`/api/rest_j/v1/dss/framework/workspace/getOverview

> Request Param

**Body:**

```json
{
  "workspaceId":1  
}
```

> Response Parameter

```json
{
  "message":"",
  "data":{
    "overview":{
      "title":1,	 
      "description":"",	 
      "dssDescription":"", 	
      "videos":[{
        "id":"",
        "title":"",
        "url":""
      }],
      "demos":[{}]
    ]
  }
}
```



> Base Info

##### Interface Name：Flush the workspace cache

**Interface Path：**`GET`/api/rest_j/v1/dss/framework/workspace/refreshCache

> Request Param

**Body:**

```json
{}
```

> Response Parameter

```json
{
  "message":""
}
```

---

#### DSSWorkspaceRoleRestful

> Base Info

##### Interface Name：Get all roles in the workspace

**Interface Path：**`GET`/api/rest_j/v1/dss/framework/workspace/getWorkspaceRoles

> Request Param

**Body:**

```json
{
  "workspaceId":""
}
```

> Response Parameter

```json
{
  "message":"",
  "data":{
    "workspaceRoles":[{
      "roleId":1,
      "roleName":"",
      "roleFrontName":""
    }]
  }
}
```



> Base Info

##### Interface Name：Create a workspace role

**Interface Path：**`POST`/api/rest_j/v1/dss/framework/workspace/addWorkspaceRole

> Request Param

**Body:**

```json
{
  "workspaceId":1,	 
  "roleName":"",	 
  "menuIds":[],		 
  "componentIds":[]	 
}
```

> Response Parameter

```json
{
  "message":"" 
}
```

---

#### DSSWorkspaceUserRestful



> Base Info

##### Interface Name：Create workspace user

**Interface Path：**`GET`/api/rest_j/v1/dss/framework/workspace/getWorkspaceUsers

> Request Param

**Body:**

```json
{
  "workspaceId":1,	 
  "pageNow":1,	//current page default 1
  "pageSize":20,	//Quantity per page The default display is 20
  "department":"",	 
  "userName":"",	 
  "roleName":""		 
}
```

> Response Parameter

```json
{
  "message":"",
  "data":{
    "roles":"",
    "workspaceUsers":[{
      "id":"",
      "name":"",
      "roles":[],
      "department":"",
      "office":"",
      "creator":"",
      "joinTime":""
    }],
    "total":100
  }
}
```



> Base Info

##### Interface Name：Create a workspace for all users

**Interface Path：**`GET`/api/rest_j/v1/dss/framework/workspace/getWorkspaceUsers

> Request Param

**Body:**

```json
{}
```

> Response Parameter

```json
{
  "message":"",
  "data":{
    "users":{
      "editUsers":[],
      "accessUsers":[],
      "releaseUsers":[]
    }
  }
}
```



> Base Info

##### Interface Name：Whether the user exits the workspace

**Interface Path：**`GET`/api/rest_j/v1/dss/framework/workspace/existUserInWorkspace

> Request Param

**Body:**

```json
{
  "workspaceId":1,
  "queryUserName":""  
}
```

> Response Parameter

```json
{
  "message":"",
  "data":{
    "existFlag":true
  }
}
```



> Base Info

##### Interface Name：Add workspace user

**Interface Path：**`POST`/api/rest_j/v1/dss/framework/workspace/existUserInWorkspace

> Request Param

**Body:**

```json
{
  "workspaceId":1,
  "userName":"",  
  "roles":[],
  "userId":""
}
```

> Response Parameter

```json
{
  "message":""
}
```



> Base Info

##### Interface Name：Update workspace user

**Interface Path：**`POST`/api/rest_j/v1/dss/framework/workspace/updateWorkspaceUser

> Request Param

**Body:**

```json
{
  "workspaceId":1,
  "userName":"",  
  "roles":[],
  "userId":""
}
```

> Response Parameter

```json
{
  "message":""
}
```



> Base Info

##### Interface Name：delete workspace user

**Interface Path：**`POST`/api/rest_j/v1/dss/framework/workspace/updateWorkspaceUser

> Request Param

**Body:**

```json
{
  "workspaceId":1,
  "userName":"",  
  "roles":[],
  "userId":""
}
```

> Response Parameter

```json
{
  "message":""
}
```



> Base Info

##### Interface Name：get all users

**Interface Path：**`GET`/api/rest_j/v1/dss/framework/workspace/listAllUsers 

> Request Param

**Body:**

```json
{}
```

> Response Parameter

```json
{
  "message":"",
  "data":{
    "users":{
      "id":"",
      "username":"",
      "department":"",
      "office":""
    }
  }
}
```



> Base Info

##### Interface Name：Get workspace id based on username

**Interface Path：**`GET`/api/rest_j/v1/dss/framework/workspace/getWorkspaceIdByUserName

> Request Param

**Body:**

```json
{
  "userName":""
}
```

> Response Parameter

```json
{
  "message":"",
  "data":{
    "userWorkspaceIds":""
  }
}
```

---

#### OrchestratorIERestful

> Base Info

##### Interface Name：Orchestrate file uploads

**Interface Path：**`POST`/api/rest_j/v1/dss/framework/orchestrator/importOrchestratorFile

> Request Param

**Body:**

```json
{
  "projectName":"",	 
  "projectID":1,	 
  "labels":"",		//Environmental information
  "files":""		//MultipartFile type, supports multiple files 
}
```

> Response Parameter

```json
{
  "message":"",
  "data":{
    "importOrcId":1
  }
}
```



##### Interface Name：Arrangement file export

**Interface Path：**`GET`/api/rest_j/v1/dss/framework/orchestrator/exportOrchestrator

> Request Param

**Body:**

```json
{
  "outputFileName":"exportOrc",	//output filename default exportOrc
  "charset":"utf-8",	//encoding default utf-8
  "outputFileType":"zip",	//output file type default zip 
  "projectName":"",	//Construction name
  "orchestratorId":"",	
  "orcVersionId":"",	
  "addOrcVersion":"",	
  "labels":""	//Environmental information
}
```

> Response Parameter

```json
{}
```

---

#### OrchestratorRestful

> Base Info

##### Interface Name：Roll back a workflow version

**Interface Path：**`GET`/api/rest_j/v1/dss/framework/orchestrator/rollbackOrchestrator

> Request Param

**Body:**

```json
{
  "orchestratorId":1,	 
  "version":"",	 
  "projectId":1,	 
  "projectName":"",	 
  "labels":""	//Environmental information
}
```

> Response Parameter

```json
{
  "message":"",
  "data":{
    "newVersion":""
  }
}
```



> Base Info

##### Interface Name：Open Orchestration Mode

**Interface Path：**`POST`/api/rest_j/v1/dss/framework/orchestrator/openOrchestrator

> Request Param

**Body:**

```json
{
  "workspaceName":1,	 
  "orchestratorId":"",	 
  "labels":{
    "route":""
  },	
}
```

> Response Parameter

```json
{
  "message":"",
  "data":{
    "OrchestratorOpenUrl":"",
    "OrchestratorVo":""
  }
}
```



> Base Info

##### Interface Name：Get all version numbers in orchestration mode

**Interface Path：**`POST`/api/rest_j/v1/dss/framework/orchestrator/getVersionByOrchestratorId

> Request Param

**Body:**

```json
{
  "orchestratorId":""	//must  
}
```

> Response Parameter

```json
{
  "message":"",
  "data":{
    "list":[]
  }
}
```

---

#### FlowRestfulApi

> Base Info

##### Interface Name：Add subflow node

**Interface Path：**`POST`/api/rest_j/v1/dss/workflow/addFlow

> Request Param

**Body:**

```json
{
  "name":"",
  "workspaceName":"",	 
  "projectName":"",	 
  "version":"",		 
  "description":"",		 
  "parentFlowID":"",	 
  "uses":"",		 
  "labels":{
    "route":""
  },
  "userName":""
}
```

> Response Parameter

```json
{
  "massage":"",
  "data":{
    "flow":{
      "id":1,
      "name":"",
      "state":true,	//Represents published and unpublished
      "source":"",
      "description":"",
      "createTime":"",
      "creator":"",
      "isRootFlow":true,
      "rank":1,
      "projectID":1,
      "linkedAppConnNames":"",
      "dssLabels":"",
      "flowEditLock":"", //Workflow Edit Lock
      "hasSaved":true,	//ture indicates that the workflow has never been saved and is ignored when publishing
      "uses":"",
      "children":[],
      "flowType":"",
      "resourceId":"",
      "bmlVersion":""
    }
  }
}
```



> Base Info

##### Interface Name：Publish workflow

**Interface Path：**`POST`/api/rest_j/v1/dss/workflow/publishWorkflow

> Request Param

**Body:**

```json
{
  "workflowId":"",	 
  "comment":"",	 
  "labels":{
    "route":""
  },
  "dssLabel":"",
  "orchestratorId":1,	 
  "orchestratorVersionId":1	
}
```

> Response Parameter

```json
{
  "massage":""
}
```



> Base Info

##### Interface Name：Get publish task status

**Interface Path：**`POST`/api/rest_j/v1/dss/workflow/getReleaseStatus

> Request Param

**Body:**

```json
{
  "releaseTaskId":1	//must  
}
```

> Response Parameter

```json
{
  "massage":"",
  "data":{
    "status":""
  }
}
```



> Base Info

##### Interface Name：Update the Base Info of the workflow, excluding updating Json, BML version, etc.

**Interface Path：**`GET`/api/rest_j/v1/dss/workflow/get

> Request Param

**Body:**

```json
{
  "flowID":1,	//must  
  "isNotHaveLock":true
}
```

> Response Parameter

```json
{
  "massage":"",
  "data":{
    "flow":{
      "id":1,
      "name":"",
      "state":true,	//Represents published and unpublished
      "source":"",
      "description":"",
      "createTime":"",
      "creator":"",
      "isRootFlow":true,
      "rank":1,
      "projectID":1,
      "linkedAppConnNames":"",
      "dssLabels":"",
      "flowEditLock":"", //Workflow Edit Lock
      "hasSaved":true,	//ture indicates that the workflow has never been saved and is ignored when publishing
      "uses":"",
      "children":[],
      "flowType":"",
      "resourceId":"",
      "bmlVersion":""
    }
  }
}
```



> Base Info

##### Interface Name：Delete workflow

**Interface Path：**`POST`/api/rest_j/v1/dss/workflow/deleteFlow

> Request Param

**Body:**

```json
{
  "id":1,	//must  
  "sure":true,
  "labels":{
    "route":""
  }
}
```

> Response Parameter

```json
{
  "massage":""
}
```



> Base Info

##### Interface Name：Workflow save interface, if the workflow Json content changes, it will update the Json content of the workflow

**Interface Path：**`POST`/api/rest_j/v1/dss/workflow/saveFlow

> Request Param

**Body:**

```json
{
  "id":1,   //must  
  "json":"",
  "workspaceName":"",
  "projectName":"",
  "flowEditLock":"",
  "isNotHaveLock":true,
  "labels":{
    "route":""
  }
}
```

> Response Parameter

```json
{
  "massage":"",
  "data":{
    "flowVersion":"",
    "flowEditLock":""
  }
}
```



> Base Info

##### Interface Name：Workflow edit lock update interface

**Interface Path：**`GET`/api/rest_j/v1/dss/workflow/updateFlowEditLock

> Request Param

**Body:**

```json
{
  "flowEditLock":""
}
```

> Response Parameter

```json
{
  "massage":"",
  "data":{
    "flowEditLock":""
  }
}
```



> Base Info

##### Interface Name：Get extra toolbars

**Interface Path：**`POST`/api/rest_j/v1/dss/workflow/getExtraToolBars

> Request Param

**Body:**

```json
{
  "projectId":1,
  "workflowId":"",
  "labels":{
    "route":""
  }
}
```

> Response Parameter

```json
{
  "massage":"",
  "data":{
    "extraBars":[{
      "name":"",
      "url":"",
      "icon":""
    }]
  }
}
```

---

#### NodeRestfulApi

> Base Info

##### Interface Name：HU

**Interface Path：**`GET`/api/rest_j/v1/dss/workflow/listNodeType

> Request Param

**Body:**

```json
{
  "projectId":1,
  "workflowId":"",
  "labels":{
    "route":""
  }
}
```

> Response Parameter

```json
{
  "massage":"",
  "data":{
    "extraBars":[{
      "name":"",
      "url":"",
      "icon":""
    }]
  }
}
```



















