
## 一、前端新增

DSS 工作流支持两种工作流节点的新增方式：

- Scriptis 脚本类型的工作流节点；
- 第三方 AppConn 的工作流节点。

请先确认您想要新增的工作流节点类型。

请注意：Scriptis 脚本类型的工作流节点，需先确保是 Scriptis 已经支持的脚本类型。如不是 Scriptis 已经支持的脚本类型，可参考：[新增 Scriptis 脚本类型](Scriptis如何新增脚本类型.md)。

### 1.1 修改 nodeType.js 文件

修改 ```web/src/apps/workflows/service/nodeType.js``` 文件，具体如下：

```
    // 1. 导入该工作流节点的 SVG 图标
    import shell from '../module/process/images/shell.svg';
    
    // 2. 指定该工作流节点的 nodeType
    const NODETYPE = {
        SHELL: 'linkis.shell.sh'
    }
    
    // 3. 指定该工作流节点，其对应的脚本类型的后缀
    const ext = {
      [NODETYPE.SHELL]: 'shell'
    }

    // 4. 绑定图标常量，与第一步的导入 SVG 图标对应。
    const NODEICON = {
      [NODETYPE.SHELL]: {
        icon: shell,
        class: {'shell': true}
      }
    }
```

### 1.2 第三方节点类型的特殊对接

如果您想新增第三方 AppConn 的工作流节点，一般情况下，会通过 Iframe 的方式，嵌入第三方系统页面。

如果您不想通过 Iframe 嵌入第三方系统页面的方式新增工作流节点，只是想在工作流节点的右侧属性栏增加一些参数，则可直接跳过该章节。

一般情况下，存在以下两种第三方节点的创建方式：

- 先创建，后打开。不能直接拖拽一个该工作流节点到 UI 画布，需先在 DSS 工作流界面弹出新增节点对话框，由 DSS 后台调用对应第三方 ```AppConn``` 的 ```RefCreationOperation``` 创建成功对应第三方的 Job 后，才能在工作流 UI 界面展示该工作流节点，双击打开跳转到该工作流节点对应的第三方系统界面。
- 先打开，后创建。可直接拖拽一个该工作流节点到 UI 界面，双击该节点即可打开跳转到对应的第三方系统界面，第三方系统界面点击保存后触发 DSS 侧的工作流节点保存操作。

第一种为通用情况，如 Visualis 的 Widget节点，这种情形无需做任何特殊处理。

第二种情况会涉及到前端 Iframe 之间的通信，需要对第三方系统页面做一些改造。

第三方系统界面的保存流程通常如下所示：

    用户在第三方系统界面点击保存 -> 第三方系统界面发起 Ajax 请求，请求第三方系统的后台新增一个新任务 -> 第三方系统的后台保存成功，返回该任务的 id 给到第三方系统前端。
 
为了能与 DSS 工作流进行对接，第三方系统界面的保存流程需改为以下流程：
    
    第三方系统前端从 URL 中获取到 nodeId  -> 用户在第三方系统界面点击保存 -> 第三方系统界面发起 Ajax 请求，请求第三方系统的后台新增一个新任务 -> 第三方系统的后台保存成功，返回该任务的 id 给到第三方系统前端 -> 第三方系统前端通过 Iframe 通信技术，将该任务 id 传给 DSS 前端。
    
可以看到，不同之处在于需要在最前和最后增加两个步骤，即：

- 第三方系统前端从 URL 中获取到 nodeId。nodeId 用于向父域页面传递数据时，指定接收的 DSS 工作流节点，可参考下面的数据格式说明。
- 第三方系统前端通过 Iframe 通信技术，将该任务 id 传给 DSS 前端。

关于第三方系统前端的 URL 中如何带上 nodeId，可参考：[第三方系统接入DSS开发指南](AppConn开发指南.md)，用于封装第三方节点页面的 Iframe URL，前端会使用该 URL 跳转到第三方系统。

Iframe 内的第三方系统界面，如何向父级页面传递数据？

如下所示：

```html
    // 向父域页面传递数据
    window.parent.postMessage('需要传送的数据，必须是字符串，数据格式请往下看！', '*');
```

需注意：
- 传递字符串数据以避免浏览器兼容性问题，json 数据可以使用 ```stringify```，```parse``` 来处理；
- 建议 ```postMessage``` 的第二个参数为 '*'，因为显示指定域，需知道父级页面的对应域名；
- ```postMessage``` 要等 iFrame load 完之后才能调用。

```postMessage``` 的第一个参数的数据格式，需为一个已经转换为字符串的 json，格式如下：

```json
{
  "type": "第三方 AppConn 类型，如：visualis",
  "nodeId": "DSS 工作流的 node key",
  "data": {
    "action": "add 或 delete，指事件类型为新增或删除",
    "jobContent": {
      // 一般建议传递该任务节点的唯一 id，DSS 工作流会将这个 map 放到 DSS 工作流节点的 jobContent 之中。
      // 以下是 Visualis 的一个示例
      "widgetId": 122
    }
  }
}
```

特别注意：**```postMessage``` 的第一个参数的数据格式，需为一个已经转换为字符串的 json**。

## 二、后台新增

前提条件：需先实现一个 AppConn，关于如何实现一个 AppConn，请参考：[第三方系统接入DSS开发指南](AppConn开发指南.md)。

### 2.1 新增 icons 文件

需在 ```dss-appconn/appconns/dss-<appconnName>-appconn/src/main/icons``` 目录中增加新工作流节点的图标。

您在新增图标时，请直接复制图标的代码，放入到文件之中。如：新建 widget.icon 文件，并复制相关 SVG 代码到该文件中。

### 2.2 init.sql 增加工作流节点的 dml sql

以下展示如何增加一个工作流节点，如下所示：

```mysql-sql

-- 1. 增加一个工作流节点
delete from `dss_workflow_node`  where `appconn_name` = '具体的AppConnName，如：Visualis';
insert into `dss_workflow_node` (`name`, `appconn_name`, `node_type`, `jump_type`, `support_jump`, `submit_to_scheduler`, `enable_copy`, `should_creation_before_node`, `icon_path`) 
    values('工作流节点名，如：display','具体的AppConnName，如：visualis','工作流节点类型，如：linkis.appconn.visualis.display',1,'1','1','0','1','icons/display.icon');
select @workflow_node_id:=id from `dss_workflow_node` where `name` = '工作流节点名，如：display';

-- 2. 将工作流节点绑定到一个分类（工作流 UI 界面的节点类型），目前已支持的类型有：
--     1-数据交换；2-数据开发；3-数据质量；4-数据可视化；5-数据输出；6-信号节点；7-功能节点；8-机器学习
delete from `dss_workflow_node_to_group` where `node_id`=@workflow_node_id;
INSERT INTO `dss_workflow_node_to_group`(`node_id`,`group_id`) values (@workflow_node_id,4);

-- 3. 为工作流节点的右侧属性栏，增加一个 input 输入框
delete from `dss_workflow_node_to_ui` where `workflow_node_id`=@workflow_node_id;
delete from `dss_workflow_node_ui` where `key` = 'input 输入框的key值，必须为英文且全局唯一';
insert  into `dss_workflow_node_ui`(`key`,`description`,`description_en`,`lable_name`,`lable_name_en`,`ui_type`,`required`,`value`,`default_value`,`is_hidden`,`condition`,`is_advanced`,`order`,`node_menu_type`,`is_base_info`,`position`) 
    values ('input 输入框的key值，必须为英文且全局唯一','中文提示描述','English description','input 输入框的标题','Input name','Input',1,NULL,NULL,0,NULL,0,1,1,1,'node');
select @workflow_node_ui_id:=id from `dss_workflow_node_ui` where `key` = 'input 输入框的key值，必须为英文且全局唯一';
INSERT INTO `dss_workflow_node_to_ui`(`workflow_node_id`,`ui_id`) values (@workflow_node_id, @workflow_node_ui_id);

-- 4. 为该 input 输入框增加一个前端校验规则
delete from `dss_workflow_node_ui_validate` where id=100;
insert into `dss_workflow_node_ui_validate` (id, `validate_type`, `validate_range`, `error_msg`, `error_msg_en`, `trigger`) values(100, 'NumInterval','[1,15]','驱动器内存大小，默认值：2','Drive memory size, default value: 2','blur');
insert into `dss_workflow_node_ui_to_validate`(`ui_id`,`validate_id`) values (@workflow_node_ui_id,100);

```

#### 2.2.1 增加一个工作流节点

```dss_workflow_node``` 核心字段解释：

- `appconn_name`，所关联的 AppConn 的 name
- `node_type`，工作流节点类型，必须以 linkis.[appconn/engineconn]开头。如果是 Linkis 引擎节点，比如 sparkSQL，则必须是：linkis.spark.sql；如果是第三方 AppConn，比如 Visualis 的 widget，则必须是：linkis.appconn.visualis.widget。
- `support_jump`，是否支持跳转URL，即工作流节点是否支持Iframe嵌入
- `jump_type`，跳转类型：1 表示是外部节点，2 表示是 Scriptis 节点。如果 supportJump 为 false，则该字段无意义。
- `submit_to_scheduler`，【保留字段】表示是否可以发布给调度系统，1 为可以发布
- `enable_copy`，节点是否支持拷贝，1 为支持拷贝
- `should_creation_before_node`，该工作流节点在拖拽到工作流画布前，是否需要先弹窗创建，1 为需要先弹窗创建
- `icon_path`，工作流节点图标路径，每个工作流节点的图标都存储在对应的 AppConn 之中

#### 2.2.2 将工作流节点绑定到一个分类

```dss_workflow_node_to_group``` 表用于将工作流节点绑定到一个分类（工作流 UI 界面的节点类型）。

目前 ```dss_workflow_node_group``` 表已在定义的类型有：

- 1-数据交换
- 2-数据开发
- 3-数据质量
- 4-数据可视化
- 5-数据输出
- 6-信号节点
- 7-功能节点
- 8-机器学习

如果已有的 ```node_group_id``` 无法满足您的需求，您可以自己往 ```dss_workflow_node_group``` 表中插入新的工作流节点分类。

#### 2.2.3 增加 input 输入框

```dss_workflow_node_ui``` 核心字段解释：

- `key`，表单控件的 key 值，例如：spark.executor.memory，必须全局唯一，该 key 会存储在 flow json 的 node params 属性之中。
- `description`，描述，用于输入型的默认提示。
- `lable_name`，input 控件的标题。
- `ui_type`，input 控件类型，目前已经支持的有：'SELECT, TextArea, Radio, Checkbox, Mutil-SELECT, Binder, Uploader'。其中 Binder 用于绑定上游节点，Uploader 为文件上传控件。
- `required`，该 input 控件是否是必填的，1 为必填
- `value`，该 input 控件可选择的值（如SELECT等）。
- `default_value`，该 input 控件默认值
- `is_hidden`，是否默认隐藏该 input 控件，1 为隐藏
- `condition`，如果是默认隐藏的，当满足 condition 时，该控件才会显示出来，比如：spark.driver.memory=5G，表示 input 控件 key 为 spark.driver.memory 的值为 5G 时，触发显示这个控件。
- `is_advanced`，是否为高级参数，高级参数默认隐藏，只有当用户在右侧属性栏点击“展示高级参数”时，才会展示出来。1 为高级参数
- `order`，展示顺序，越小越排前面。
- `node_menu_type`，input 输入框类型，1 表示工作流节点右侧属性栏的 input 输入框，0 表示工作流节点新建对话框的 input 输入框
- `is_base_info`，是否是工作流节点右侧属性栏的基本信息，1 为基本信息
- `position`，该 key-value 键值对存储的位置，有：node、startup、runtime。其中 startup 表示存储在 flow json 的 node -> params -> startup 中，runtime 表示存储在 flow json 的 node -> params -> runtime 中，node 表示存储在 flow json 的 node 中。

#### 2.2.4 为该 input 输入框增加一个前端校验规则

```dss_workflow_node_ui_validate``` 核心字段解释：

- `id`，由于允许一个 ```dss_workflow_node_ui_validate``` 给多个 ```dss_workflow_node_ui``` 使用，所以需要用户自己决定 id 值，用户在选择一个 id 时，必须确保该 id 全表唯一。
- `validate_type`，校验类型，有：None, NumInterval, FloatInterval, Include, Regex, OPF'
- `validate_range`，校验范围，比如：[1,15]
- `error_msg`，校验失败时的中文错误提示信息
- `error_msg_en`，校验失败时的英文错误提示信息
- `trigger`，校验的触发规则，有：blur、change。其中 blur 指失去鼠标焦点，change 指内容发生变化时

其中 NumInterval 和 FloatInterval 要求必须满足 `validate_range` 限定的数值范围，例如：[1,15]。

Include 要求必须包含 `validate_range` 的内容。

Regex 要求必须满足 `validate_range` 的正则表达式。

OPF 要求必须是 `validate_range` 数组中的一个，例如：[\"苹果\",\"香蕉\"]，表示该 input 控件的 value 必须是苹果或是香蕉。
