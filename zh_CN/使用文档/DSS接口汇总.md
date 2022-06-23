### DSS接口汇总

#### FlowEntranceRestfulApi

> 基本信息

##### 接口名称：工作流执行

**接口路径：**`POST` /api/rest_j/v1/dss/flow/entrance/execute

> 请求参数

**Body:**

````json
{
  "isReExecute":""	//必填 新增参数，失败重新执行时需要增加此参数 
}
````

> 返回参数

```json
{
  "status":"",    //状态码
  "message":"",	  //信息
  "data":{
    "taskID":"",  //任务ID
    "execID":""   //执行ID
  }
}
```



> 基本信息

##### 接口名称：获取工作流状态

**接口路径：**`POST` /api/rest_j/v1/flow/entrance/{id}/status

> 请求参数

**Body:**

````json
{
  "id":"", //必填
  "taskID":""  //任务ID
}
````

> 返回参数

````json
{
  "status":"",   //状态码
  "message":"",  //信息
  "data":{
    "taskID":"",  //任务ID
    "execID":""   //执行ID
  }
}
````



> 基本信息

##### 接口名称：结束工作流

**接口路径：**`GET` /api/rest_j/v1/dss/flow/entrance/{id}/killWorkflow

> 请求参数

**Body:**

````json
{
  "taskID":""  //必填 
}
````

> 响应参数

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

> 基本信息

##### 接口名称：获取任务进度

**接口路径：**`GET`/api/rest_j/v1/dss/framework/release/getReleaseStatus

> 请求参数

**Body:**

````json
{
  "dssLabel":"",	//必填 dssLabel
  "releaseTaskId":""	//必填 查询的发布id
}
````

> 响应参数

````json
{
  "massage":"",
  "data":{
    "status":"",
    "errorMsg":""
  }
}
````



> 基本信息

##### 接口名称：提交发布任务

**接口路径：**`POST` /api/rest_j/v1/dss/framework/release/releaseOrchestrator

> 请求参数

**Body:**

```json
{
  "orchestratorId":"",	//必填 编排模式id
  "orchestratorVersionId":"",	//必填 编排模式版本id
  "comment":"",	//必填 发布注释
  "dssLabel":""	//必填 dssLabel
}
```

> 响应参数

````json
{
  "massage":"",
  "data":""
}
````

---

#### ApiServiceTokenResfulApi

> 基本信息

##### 接口名称：token查询

**接口路径：**`GET` /api/rest_j/v1/dss/apiservice/tokenQuery

> 请求参数

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

> 响应参数

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



> 基本信息

##### 接口名称：刷新申请单

**接口路径：**`GET` /api/rest_j/v1/dss/apiservice/approvalRefresh

> 请求参数

**Body:**

```json
{
  "approvalNo":"" 
}
```

> 响应参数

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

> 基本信息

##### 接口名称：表查询

**接口路径：**`POST`/api/rest_j/v1/dss/workflow/tables

> 请求参数

**Body:**

```json
{
  "contextID":"",	 //ID
  "nodeName":"",	//节点名
  "labels":{
    "route":""
  }
}
```

> 响应参数

```json
{
  "massage":"",
  "data":{
    "tables":[]
  }
}
```



> 基本信息

##### 接口名称：列查询

**接口路径：**`POST`/api/rest_j/v1/dss/workflow/columns

> 请求参数

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

> 响应参数

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

> 基本信息

##### 接口名称：数据字典 - 根据key获取

**接口路径：**`POST`/api/rest_j/v1/dss/framework/workspace/getDicList

> 请求参数

**Body:**

```json
{
  "workspaceId":"",	 //必填 空间id
  "dicKey":"",	
  "parentKey":""	//父key 
}
```

> 响应参数

```json
{
  "massage":"",
  "data":{
    "list":[{
      "id":0,
      "workspaceId":0, //空间id
      "parentKey":"", //父key
      "dicName":"", //名称
      "dicNameEn":"", //英文名称
      "dicKey":"", //key 相当于编码，空间是w_开头，工程是p_
      "dicValue":"", //key对应的值
      "dicValueEn":"", //key对应的值(英文)
      "title":"", //标题
      "titleEn":"", //标题(英文)
      "url":"",
      "urlType":1, //url类型: 0-内部系统，1-外部系统；默认是内部
      "icon":"", //图标
      "orderNum":1, //序号
      "remark":"", //备注
      "createUser":"", //创建人
      "createTime":"", //创建时间
      "updateUser":"", //更新人
      "updateTime":"" //更新时间
    }]
  }
}
```



> 基本信息

##### 接口名称：获取数据字典

**接口路径：**`POST`/api/rest_j/v1/dss/framework/workspace/getDicSecondList

> 请求参数

**Body:**

```json
{
  "workspaceId":"",	 //必填 空间id
  "dicKey":"",	//key 相当于编码，空间是w_开头，工程是p_
  "parentKey":""	//父key 
}
```

> 响应参数

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

> 基本信息

##### 接口名称：创建编排模式

**接口路径：**`POST`/api/rest_j/v1/dss/framework/orchestrator/createOrchestrator

> 请求参数

**Body:**

```json
{
  "orchestratorName":"",	 //必填 编排名称
  "orchestratorMode":"",	//编排模式，如工作流，组合编排等
  "orchestratorWays":[],	//必填 编排方式
  "orchestratorLevel":"",	//编排重要性级别
  "dssLabels":[],	
  "uses":"",	//编排用途
  "description":"",	//必填 描述
  "projectName":"", //工程名称
  "workspaceName":"" //工作空间名称
}
```

> 响应参数

```json
{
  "massage":"",
  "data":{
    "orchestratorId":1
  }
}
```



> 基本信息

##### 接口名称：查询所有的编排模式

**接口路径：**`POST`/api/rest_j/v1/dss/framework/orchestrator/getAllOrchestrator

> 请求参数

**Body:**

```json
{
  "workspaceId":"",	 //必填
  "projectId":"",	//必填
  "orchestratorMode":"",	//编排类型，如工作流，组合编排等
  "orchestratorLevel":"",	//编排重要性级别
  "dssLabels":{"route":""}	//dssLabels是通过前端进行传入的，主要是用来进行当前的环境信息
}
```

> 响应参数

```json
{
  "massage":"",
  "data":{
    "page":[{
      "id":1, //主键ID
      "workspaceId":"", //空间id
      "projectId":"", //工程id
      "orchestratorId":1, //编排模式id（工作流,调用orchestrator服务返回的orchestratorId）
      "orchestratorVersionId":1, //编排模式版本id（工作流,调用orchestrator服务返回的orchestratorVersionId）
      "orchestratorName":"", //编排名称
      "orchestratorMode":"", //编排模式，取得的值是dss_dictionary中的dic_key(parent_key=p_orchestratorment_mode)
      "uses":"", //用途
      "description":"", //描述
      "createUser":"", //创建人
      "createTime":"", //创建时间
      "updateUser":"", //更新人
      "updateTime":"", //更新时间
      "orchestratorWays":[], //编排方式
      "orchestratorLevel":"", //编排重要性级别
      "flowEditLockExist":false //工作流编辑锁是否退出
    }]
  }
}
```



> 基本信息

##### 接口名称：修改编排模式

**接口路径：**`POST`/api/rest_j/v1/dss/framework/orchestrator/modifyOrchestrator

> 请求参数

**Body:**

```json
{
  "id":"",	 //必填
  "orchestratorName":"",	//必填 编排名称
  "orchestratorMode":"",	//编排类型，如工作流，组合编排等
  "orchestratorLevel":"",	//编排重要性级别
  "uses":"",	//编排用途
  "description":"",	//必填 描述
  "orchestratorWays":[],	//必填 编排方式
  "dssLabels":""	//当前的环境信息
}
```

> 响应参数

```json
{
  "massage":"",
  "data":{
    "orchestratorId":1
  }
}
```

> 基本信息

##### 接口名称：删除编排模式

**接口路径：**`POST`/api/rest_j/v1/dss/framework/orchestrator/deleteOrchestrator

> 请求参数

**Body:**

```json
{
  "id":""	 //必填
}
```

> 响应参数

```json
{
  "massage":""
}
```



> 基本信息

##### 接口名称：获取编排重要级别列表

**接口路径：**`GET`/api/rest_j/v1/dss/framework/orchestrator/orchestratorLevels

> 请求参数

**Body:**

```json
{}
```

> 响应参数

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

> 基本信息

##### 接口名称：导入旧的DSS工程

**接口路径：**`POST`/api/rest_j/v1/dss/framework/release/importOldDSSProject

> 请求参数

**Body:**

```json
{
  "file":""	 //必填 MultipartFile
}
```

> 响应参数

```json
{
  "massage":""
}
```



> 基本信息

##### 接口名称：导入从dss1.0接口导出的工作流

**接口路径：**`POST`/api/rest_j/v1/dss/framework/release/importworkflow

> 请求参数

**Body:**

```json
{
  "workspaceName":"",	 //必填 工作空间
  "projectName":"",	//项目名没有就创建
  "projectUser":"",	// 项目用户，接口会保证本用户加入到权限列表
  "flowName":"",
  "file":"" //导入文件，MultipartFile类型
}
```

> 响应参数

```json
{
  "massage":"",
  "data":{
    "flowId":"",
    "bmlVersion":""
  }
}
```



> 基本信息

##### 接口名称：导出SQL文件

**接口路径：**`GET`/api/rest_j/v1/dss/framework/release/exportOrcSqlFile

> 请求参数

**Body:**

```json
{
  "outputFileName":"",	 //默认exportOrc
  "charset":"utf-8",	//编码 默认utf-8
  "outputFileType":"zip",	// 输出文件类型 默认zip
  "projectName":"",	//工程名称
  "orchestratorId":"", //编排id
  "orcVersionId":"", //编排版本号ID
  "addOrcVersion":false, //是否增加版本号
  "labels":"" //标签
}
```

> 响应参数

```json
{}
```



> 基本信息

##### 接口名称：批量导出工程下所有工作流到指定本地目录

**接口路径：**`POST`/api/rest_j/v1/dss/framework/release/exportallflow

> 请求参数

**Body:**

```json
{
  "workspaceName":"",	 //工作空间名
  "projectName":"",	//工程名
  "pathRoot":"" //路径
}
```

> 响应参数

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

> 基本信息

##### 接口名称：获取侧边栏

**接口路径：**`POST`/api/rest_j/v1/dss/framework/workspace/getSideInfos

> 请求参数

**Body:**

```json
{
  "workspaceID":""	 //工作空间id
}
```

> 响应参数

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
        "url":0,	//url类型: 0-内部系统，1-外部系统；默认是内部
        "icon":""	//icon是表示Content的图标，如果为空就是没有
      }]
    }]
  }
}
```

---

#### DSSWorkspacePrivRestful

> 基本信息

##### 接口名称：获取工作空间菜单

**接口路径：**`GET`/api/rest_j/v1/dss/framework/workspace/exportOrcSqlFile

> 请求参数

**Body:**

```json
{
  "workspaceId":""	 //工作空间id
}
```

> 响应参数

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



> 基本信息

##### 接口名称：获取工作空间设置

**接口路径：**`GET`/api/rest_j/v1/dss/framework/workspace/getWorkspaceHomepageSettings

> 请求参数

**Body:**

```json
{
  "workspaceId":""	 //工作空间id
}
```

> 响应参数

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



> 基本信息

##### 接口名称：更新角色菜单权限

**接口路径：**`POST`/api/rest_j/v1/dss/framework/workspace/updateRoleMenuPriv

> 请求参数

**Body:**

```json
{
  "menuId":"",	 //菜单id
  "workspaceId":"",	//工作空间id
  "menuPrivs":"" //菜单
}
```

> 响应参数

```json
{
  "message":"" 
}
```



> 基本信息

##### 接口名称：更新组件权限

**接口路径：**`POST`/api/rest_j/v1/dss/framework/workspace/updateRoleComponentPriv

> 请求参数

**Body:**

```json
{
  "componentId":"",	 //组件id
  "workspaceId":"",	//工作空间id
  "componentPrivs":"" //组件权限
}
```

> 响应参数

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

> 基本信息

##### 接口名称：建立工作空间

**接口路径：**`POST`/api/rest_j/v1/dss/framework/workspace/createWorkspace

> 请求参数

**Body:**

```json
{
  "workspaceName":"",	 //工作空间名称
  "department":"",	//部门
  "description":"", //描述
  "tags":"",	//描述
  "productName":""	//产品名称
}
```

> 响应参数

```json
{
  "message":"",
  "data":{
    "workspaceId":1,
    "workspaceName":""
  }
}
```



> 基本信息

##### 接口名称：获取部门列表

**接口路径：**`GET`/api/rest_j/v1/dss/framework/workspace/listDepartments

> 请求参数

**Body:**

```json
{}
```

> 响应参数

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



> 基本信息

##### 接口名称：获取工作空间列表

**接口路径：**`GET`/api/rest_j/v1/dss/framework/workspace/getWorkspaces

> 请求参数

**Body:**

```json
{}
```

> 响应参数

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



> 基本信息

##### 接口名称：获取用户工作空间

**接口路径：**`GET`/api/rest_j/v1/dss/framework/workspace/getWorkspaceHomePage

> 请求参数

**Body:**

```json
{
  "micro_module":"" //模块名
}
```

> 响应参数

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



> 基本信息

##### 接口名称：获取工作空间概述

**接口路径：**`GET`/api/rest_j/v1/dss/framework/workspace/getOverview

> 请求参数

**Body:**

```json
{
  "workspaceId":1 //工作空间ID
}
```

> 响应参数

```json
{
  "message":"",
  "data":{
    "overview":{
      "title":1,	//标题
      "description":"",	//描述
      "dssDescription":"", 	//DSS描述
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



> 基本信息

##### 接口名称：刷新工作空间缓存

**接口路径：**`GET`/api/rest_j/v1/dss/framework/workspace/refreshCache

> 请求参数

**Body:**

```json
{}
```

> 响应参数

```json
{
  "message":""
}
```

---

#### DSSWorkspaceRoleRestful

> 基本信息

##### 接口名称：获取工作空间中所有的角色

**接口路径：**`GET`/api/rest_j/v1/dss/framework/workspace/getWorkspaceRoles

> 请求参数

**Body:**

```json
{
  "workspaceId":""
}
```

> 响应参数

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



> 基本信息

##### 接口名称：创建工作空间角色

**接口路径：**`POST`/api/rest_j/v1/dss/framework/workspace/addWorkspaceRole

> 请求参数

**Body:**

```json
{
  "workspaceId":1,	//工作空间id
  "roleName":"",	//角色名
  "menuIds":[],		//菜单id
  "componentIds":[]	//组件id
}
```

> 响应参数

```json
{
  "message":"" 
}
```

---

#### DSSWorkspaceUserRestful



> 基本信息

##### 接口名称：创建工作空间用户

**接口路径：**`GET`/api/rest_j/v1/dss/framework/workspace/getWorkspaceUsers

> 请求参数

**Body:**

```json
{
  "workspaceId":1,	//工作空间id
  "pageNow":1,	//当前页 默认1
  "pageSize":20,	//每页数量 默认显示20条
  "department":"",	//部门
  "userName":"",	//用户名
  "roleName":""		//角色名
}
```

> 响应参数

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



> 基本信息

##### 接口名称：创建工作空间所有用户

**接口路径：**`GET`/api/rest_j/v1/dss/framework/workspace/getWorkspaceUsers

> 请求参数

**Body:**

```json
{}
```

> 响应参数

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



> 基本信息

##### 接口名称：用户是否退出工作空间

**接口路径：**`GET`/api/rest_j/v1/dss/framework/workspace/existUserInWorkspace

> 请求参数

**Body:**

```json
{
  "workspaceId":1,
  "queryUserName":"" //查询用户名
}
```

> 响应参数

```json
{
  "message":"",
  "data":{
    "existFlag":true
  }
}
```



> 基本信息

##### 接口名称：添加工作空间用户

**接口路径：**`POST`/api/rest_j/v1/dss/framework/workspace/existUserInWorkspace

> 请求参数

**Body:**

```json
{
  "workspaceId":1,
  "userName":"", //用户名
  "roles":[],
  "userId":""
}
```

> 响应参数

```json
{
  "message":""
}
```



> 基本信息

##### 接口名称：更新工作空间用户

**接口路径：**`POST`/api/rest_j/v1/dss/framework/workspace/updateWorkspaceUser

> 请求参数

**Body:**

```json
{
  "workspaceId":1,
  "userName":"", //用户名
  "roles":[],
  "userId":""
}
```

> 响应参数

```json
{
  "message":""
}
```



> 基本信息

##### 接口名称：删除工作空间用户

**接口路径：**`POST`/api/rest_j/v1/dss/framework/workspace/updateWorkspaceUser

> 请求参数

**Body:**

```json
{
  "workspaceId":1,
  "userName":"", //用户名
  "roles":[],
  "userId":""
}
```

> 响应参数

```json
{
  "message":""
}
```



> 基本信息

##### 接口名称：获取所有用户

**接口路径：**`GET`/api/rest_j/v1/dss/framework/workspace/listAllUsers 

> 请求参数

**Body:**

```json
{}
```

> 响应参数

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



> 基本信息

##### 接口名称：根据用户名获取工作空间id

**接口路径：**`GET`/api/rest_j/v1/dss/framework/workspace/getWorkspaceIdByUserName

> 请求参数

**Body:**

```json
{
  "userName":""
}
```

> 响应参数

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

> 基本信息

##### 接口名称：编排文件上传

**接口路径：**`POST`/api/rest_j/v1/dss/framework/orchestrator/importOrchestratorFile

> 请求参数

**Body:**

```json
{
  "projectName":"",	//工程名
  "projectID":1,	//工程id
  "labels":"",		//环境信息
  "files":""		//MultipartFile类型，支持多文件 
}
```

> 响应参数

```json
{
  "message":"",
  "data":{
    "importOrcId":1
  }
}
```



##### 接口名称：编排文件导出

**接口路径：**`GET`/api/rest_j/v1/dss/framework/orchestrator/exportOrchestrator

> 请求参数

**Body:**

```json
{
  "outputFileName":"exportOrc",	//输出文件名 默认exportOrc
  "charset":"utf-8",	//编码 默认utf-8
  "outputFileType":"zip",	//输出文件类型 默认zip 
  "projectName":"",	//工程名
  "orchestratorId":"",	//编排id
  "orcVersionId":"",	//版本号id
  "addOrcVersion":"",	//新增版本号id
  "labels":""	//环境信息
}
```

> 响应参数

```json
{}
```

---

#### OrchestratorRestful

> 基本信息

##### 接口名称：回滚工作流版本

**接口路径：**`GET`/api/rest_j/v1/dss/framework/orchestrator/rollbackOrchestrator

> 请求参数

**Body:**

```json
{
  "orchestratorId":1,	//编排id
  "version":"",	//版本号id
  "projectId":1,	//项目id
  "projectName":"",	//工程名
  "labels":""	//环境信息
}
```

> 响应参数

```json
{
  "message":"",
  "data":{
    "newVersion":""
  }
}
```



> 基本信息

##### 接口名称：打开编排模式

**接口路径：**`POST`/api/rest_j/v1/dss/framework/orchestrator/openOrchestrator

> 请求参数

**Body:**

```json
{
  "workspaceName":1,	//工作空间名称
  "orchestratorId":"",	//编排id
  "labels":{
    "route":""
  },	
}
```

> 响应参数

```json
{
  "message":"",
  "data":{
    "OrchestratorOpenUrl":"",
    "OrchestratorVo":""
  }
}
```



> 基本信息

##### 接口名称：获取编排模式下的所有版本号

**接口路径：**`POST`/api/rest_j/v1/dss/framework/orchestrator/getVersionByOrchestratorId

> 请求参数

**Body:**

```json
{
  "orchestratorId":""	//必填 编排id
}
```

> 响应参数

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

> 基本信息

##### 接口名称：添加subflow节点

**接口路径：**`POST`/api/rest_j/v1/dss/workflow/addFlow

> 请求参数

**Body:**

```json
{
  "name":"",
  "workspaceName":"",	//工作空间名称
  "projectName":"",	//工程名
  "version":"",		//版本号
  "description":"",		//描述
  "parentFlowID":"",	//工作流父节点id
  "uses":"",		//用途
  "labels":{
    "route":""
  },
  "userName":""
}
```

> 响应参数

```json
{
  "massage":"",
  "data":{
    "flow":{
      "id":1,
      "name":"",
      "state":true,	//代表发布过和未发布过
      "source":"",
      "description":"",
      "createTime":"",
      "creator":"",
      "isRootFlow":true,
      "rank":1,
      "projectID":1,
      "linkedAppConnNames":"",
      "dssLabels":"",
      "flowEditLock":"", //工作流编辑锁
      "hasSaved":true,	//ture表示工作流从来没存过，发布的时候忽略
      "uses":"",
      "children":[],
      "flowType":"",
      "resourceId":"",
      "bmlVersion":""
    }
  }
}
```



> 基本信息

##### 接口名称：发布工作流

**接口路径：**`POST`/api/rest_j/v1/dss/workflow/publishWorkflow

> 请求参数

**Body:**

```json
{
  "workflowId":"",	//工作流id
  "comment":"",	//备注
  "labels":{
    "route":""
  },
  "dssLabel":"",
  "orchestratorId":1,	//编排id
  "orchestratorVersionId":1	//编排版本id
}
```

> 响应参数

```json
{
  "massage":""
}
```



> 基本信息

##### 接口名称：获取发布任务状态

**接口路径：**`POST`/api/rest_j/v1/dss/workflow/getReleaseStatus

> 请求参数

**Body:**

```json
{
  "releaseTaskId":1	//必填 发布id
}
```

> 响应参数

```json
{
  "massage":"",
  "data":{
    "status":""
  }
}
```



> 基本信息

##### 接口名称：更新工作流的基本信息，不包括更新Json,BML版本等

**接口路径：**`GET`/api/rest_j/v1/dss/workflow/get

> 请求参数

**Body:**

```json
{
  "flowID":1,	//必填 工作流id
  "isNotHaveLock":true
}
```

> 响应参数

```json
{
  "massage":"",
  "data":{
    "flow":{
      "id":1,
      "name":"",
      "state":true,	//代表发布过和未发布过
      "source":"",
      "description":"",
      "createTime":"",
      "creator":"",
      "isRootFlow":true,
      "rank":1,
      "projectID":1,
      "linkedAppConnNames":"",
      "dssLabels":"",
      "flowEditLock":"", //工作流编辑锁
      "hasSaved":true,	//ture表示工作流从来没存过，发布的时候忽略
      "uses":"",
      "children":[],
      "flowType":"",
      "resourceId":"",
      "bmlVersion":""
    }
  }
}
```



> 基本信息

##### 接口名称：删除工作流

**接口路径：**`POST`/api/rest_j/v1/dss/workflow/deleteFlow

> 请求参数

**Body:**

```json
{
  "id":1,	//必填 工作流id
  "sure":true,
  "labels":{
    "route":""
  }
}
```

> 响应参数

```json
{
  "massage":""
}
```



> 基本信息

##### 接口名称：工作流保存接口，如工作流Json内容有变化，会更新工作流的Json内容

**接口路径：**`POST`/api/rest_j/v1/dss/workflow/saveFlow

> 请求参数

**Body:**

```json
{
  "id":1,   //必填 工作流id
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

> 响应参数

```json
{
  "massage":"",
  "data":{
    "flowVersion":"",
    "flowEditLock":""
  }
}
```



> 基本信息

##### 接口名称：工作流编辑锁更新接口

**接口路径：**`GET`/api/rest_j/v1/dss/workflow/updateFlowEditLock

> 请求参数

**Body:**

```json
{
  "flowEditLock":""
}
```

> 响应参数

```json
{
  "massage":"",
  "data":{
    "flowEditLock":""
  }
}
```



> 基本信息

##### 接口名称：获取额外工具栏

**接口路径：**`POST`/api/rest_j/v1/dss/workflow/getExtraToolBars

> 请求参数

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

> 响应参数

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

> 基本信息

##### 接口名称：HU

**接口路径：**`GET`/api/rest_j/v1/dss/workflow/listNodeType

> 请求参数

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

> 响应参数

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



















