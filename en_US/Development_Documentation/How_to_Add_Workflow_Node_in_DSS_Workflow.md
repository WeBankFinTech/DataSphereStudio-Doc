
## 1. Front-end addition

DSS workflow supports two new ways of adding workflow nodes:

- Workflow nodes of the Scriptis script type;
- Workflow nodes for third-party AppConn.

Please confirm the type of workflow node you want to add first.

Please note: Workflow nodes of the Scriptis script type must be of a script type already supported by Scriptis. If it is not a script type already supported by Scriptis, please refer to：[Added Scriptis script type](How_to_add_script_types_in_Scriptis.md)。

### 1.1 Modify the nodeType.js file

Modify the ```web/src/apps/workflows/service/nodeType.js``` file as follows:

```
    // 1. Import the SVG icon of the workflow node
    import shell from '../module/process/images/shell.svg';
    
    // 2. Specify the nodeType of the workflow node
    const NODETYPE = {
        SHELL: 'linkis.shell.sh'
    }
    
    // 3. Specify the workflow node and the suffix of its corresponding script type
    const ext = {
      [NODETYPE.SHELL]: 'shell'
    }

    // 4. Bind the icon constants, corresponding to the imported SVG icons in the first step.
    const NODEICON = {
      [NODETYPE.SHELL]: {
        icon: shell,
        class: {'shell': true}
      }
    }
```

### 1.2 Special docking of third-party node types

If you want to add a workflow node of a third-party AppConn, in general, the third-party system page will be embedded in an Iframe.

If you don't want to add a workflow node by embedding a third-party system page through an Iframe, but just want to add some parameters to the property bar on the right side of the workflow node, you can skip this chapter directly.

In general, there are two ways to create third-party nodes:

- Create first, then open. You cannot directly drag and drop a workflow node to the UI canvas. You need to pop up a new node dialog box on the DSS workflow interface. The DSS background calls the ```RefCreationOperation``` corresponding to the third-party ```AppConn``` to create After successfully corresponding to the third-party job, the workflow node can be displayed on the workflow UI interface. Double-click to open and jump to the third-party system interface corresponding to the workflow node.
- Open first, then create. You can directly drag and drop a workflow node to the UI interface, double-click the node to open and jump to the corresponding third-party system interface, and click Save on the third-party system interface to trigger the workflow node save operation on the DSS side.

The first is the general case, such as the Widget node of Visualis, which does not require any special handling.

The second case involves the communication between the front-end Iframes, which requires some modifications to the third-party system pages.

The saving process of the third-party system interface is usually as follows:

    The user clicks save on the third-party system interface -> the third-party system interface initiates an Ajax request, requesting a new task in the background of the third-party system -> the background of the third-party system is successfully saved, and returns the id of the task to the front-end of the third-party system .

In order to connect with the DSS workflow, the saving process of the third-party system interface needs to be changed to the following process:

    The front-end of the third-party system obtains the nodeId from the URL -> the user clicks save on the third-party system interface -> the third-party system interface initiates an Ajax request to request a new task in the background of the third-party system -> the background of the third-party system is successfully saved , return the id of the task to the front-end of the third-party system -> the front-end of the third-party system transmits the task id to the DSS front-end through Iframe communication technology.

As you can see, the difference is that two steps need to be added to the first and last, namely:

- The third-party system front end gets the nodeId from the URL. nodeId is used to specify the received DSS workflow node when transferring data to the parent domain page. Please refer to the following data format description.
- The third-party system front-end transmits the task id to the DSS front-end through the Iframe communication technology.

For how to include nodeId in the URL of the front-end of the third-party system, please refer to: [Third-party system access DSS development guide](AppConn_Development_Guide.md), which is used to encapsulate the Iframe URL of the third-party node page, and the front-end will use this URL to jump to third-party systems.
How does the third-party system interface in the Iframe pass data to the parent page?

As follows:

```html
    // Pass data to parent domain page
    window.parent.postMessage('The data to be transmitted must be a character string, please see the data format below！', '*');
```

Note：
- Pass string data to avoid browser compatibility issues, json data can be processed using ```stringify```, ```parse```;
- It is recommended that the second parameter of ``postMessage```` is '*', because to display the specified domain, you need to know the corresponding domain name of the parent page;
- ````postMessage``` can only be called after the iFrame is loaded.

The data format of the first parameter of ```postMessage``` needs to be a json that has been converted to a string. The format is as follows:

```json
{
  "type": "Third-party AppConn types, such as: visualis",
  "nodeId": "node key for DSS workflow",
  "data": {
    "action": "add or delete, which means the event type is add or delete",
    "jobContent": {
      // It is generally recommended to pass the unique id of the task node, and the DSS workflow will put this map into the jobContent of the DSS workflow node.
      // Here is an example from Visualis
      "widgetId": 122
    }
  }
}
```

pay attention：**The data format of the first parameter of ```postMessage``` needs to be a json that has been converted to a string**。

## 2. New background

Prerequisite: You need to implement an AppConn first. For how to implement an AppConn, please refer to：[Third-party system access DSS development guide](AppConn_Development_Guide.md)。

### 2.1 Add icons file

The icon for the new workflow node needs to be added to the ```dss-appconn/appconns/dss-<appconnName>-appconn/src/main/icons``` directory.

When you add an icon, please directly copy the code of the icon and put it into the file. For example: create a new widget.icon file, and copy the relevant SVG code into this file.

### 2.2 init.sql adds the dml sql of the workflow node

The following shows how to add a workflow node as follows:

```mysql-sql

-- 1. Add a workflow node
delete from `dss_workflow_node`  where `appconn_name` = 'Specific AppConnName, such as: Visualis';
insert into `dss_workflow_node` (`name`, `appconn_name`, `node_type`, `jump_type`, `support_jump`, `submit_to_scheduler`, `enable_copy`, `should_creation_before_node`, `icon_path`) 
    values('Workflow node name, such as: display','Specific AppConnName, such as: visualis','Workflow Node Type，such as：linkis.appconn.visualis.display',1,'1','1','0','1','icons/display.icon');
select @workflow_node_id:=id from `dss_workflow_node` where `name` = 'Workflow node name, such as: display';

-- 2. Bind a workflow node to a category (node type of the workflow UI interface), currently supported types are:
--     1-Data exchange; 2-Data development; 3-Data quality; 4-Data visualization; 5-Data output; 6-Signal node; 7-Function node; 8-Machine learning
delete from `dss_workflow_node_to_group` where `node_id`=@workflow_node_id;
INSERT INTO `dss_workflow_node_to_group`(`node_id`,`group_id`) values (@workflow_node_id,4);

-- 3. Add an input input box to the right property bar of the workflow node
delete from `dss_workflow_node_to_ui` where `workflow_node_id`=@workflow_node_id;
delete from `dss_workflow_node_ui` where `key` = 'input The key value of the input box, must be in English and globally unique';
insert  into `dss_workflow_node_ui`(`key`,`description`,`description_en`,`lable_name`,`lable_name_en`,`ui_type`,`required`,`value`,`default_value`,`is_hidden`,`condition`,`is_advanced`,`order`,`node_menu_type`,`is_base_info`,`position`) 
    values ('input The key value of the input box, must be in English and globally unique','Chinese prompt description','English description','the title of the input box','Input name','Input',1,NULL,NULL,0,NULL,0,1,1,1,'node');
select @workflow_node_ui_id:=id from `dss_workflow_node_ui` where `key` = 'input The key value of the input box, must be in English and globally unique';
INSERT INTO `dss_workflow_node_to_ui`(`workflow_node_id`,`ui_id`) values (@workflow_node_id, @workflow_node_ui_id);

-- 4. Add a front-end validation rule to the input input box
delete from `dss_workflow_node_ui_validate` where id=100;
insert into `dss_workflow_node_ui_validate` (id, `validate_type`, `validate_range`, `error_msg`, `error_msg_en`, `trigger`) values(100, 'NumInterval','[1,15]','驱动器内存大小，默认值：2','Drive memory size, default value: 2','blur');
insert into `dss_workflow_node_ui_to_validate`(`ui_id`,`validate_id`) values (@workflow_node_ui_id,100);

```

#### 2.2.1 Add a workflow node

```dss_workflow_node``` Explanation of core fields：

- `appconn_name`，The name of the associated AppConn
- `node_type`，Workflow node type, must start with linkis.[appconn/engineconn]. If it is a Linkis engine node, such as sparkSQL, it must be: linkis.spark.sql; if it is a third-party AppConn, such as a Visualis widget, it must be: linkis.appconn.visualis.widget.
- `support_jump`，Whether to support jump URL, that is, whether the workflow node supports Iframe embedding
- `jump_type`，Jump type: 1 means it is an external node, 2 means it is a Scriptis node. If supportJump is false, this field has no meaning.
- `submit_to_scheduler`，[Reserved field] Indicates whether it can be released to the scheduling system, 1 means it can be released
- `enable_copy`，Whether the node supports copying, 1 means copying
- `should_creation_before_node`，Whether the workflow node needs to be created with a pop-up window before dragging it to the workflow canvas, 1 means it needs to be created with a pop-up window first
- `icon_path`，Workflow node icon path, the icon of each workflow node is stored in the corresponding AppConn

#### 2.2.2 Bind workflow nodes to a category

```dss_workflow_node_to_group``` Tables are used to bind workflow nodes to a category (node type of the workflow UI interface).

Currently, the types defined in the ``dss_workflow_node_group``` table are:

- 1- Data exchange
- 2- Data Development
- 3- Data Quality
- 4-Data visualization
- 5-Data output
- 6-Signal Node
- 7-function node
- 8-Machine Learning

If the existing ```node_group_id``` does not meet your needs, you can insert a new workflow node classification into the ```dss_workflow_node_group``` table by yourself.

#### 2.2.3 add input box

```dss_workflow_node_ui``` Explanation of core fields：

- `key`, the key value of the form control, for example: spark.executor.memory, must be globally unique, the key will be stored in the node params property of flow json.
- `description`, description, default prompt for input type.
- `lable_name`, the title of the input control.
- `ui_type`, the input control type, currently supported: 'SELECT, TextArea, Radio, Checkbox, Mutil-SELECT, Binder, Uploader'. Among them, Binder is used to bind upstream nodes, and Uploader is a file upload control.
- `required`, whether the input control is required, 1 is required
- `value`, the selectable value of the input control (such as SELECT, etc.).
- `default_value`, the default value of the input control
- `is_hidden`, whether to hide the input control by default, 1 is hidden
- `condition`, if it is hidden by default, the control will only be displayed when the condition is satisfied, for example: spark.driver.memory=5G, it means that the input control key is spark.driver.memory and the value is 5G, triggering Show this control.
- `is_advanced`, whether it is an advanced parameter, the advanced parameter is hidden by default, and will be displayed only when the user clicks "Show Advanced Parameters" in the property bar on the right. 1 is an advanced parameter
- `order`, the display order, the smaller the order, the higher the order.
- `node_menu_type`, the type of input input box, 1 represents the input input box of the property bar on the right side of the workflow node, and 0 represents the input input box of the new dialog box of the workflow node
- `is_base_info`, whether it is the basic information of the property bar on the right side of the workflow node, 1 is the basic information
- `position`, the location where the key-value key-value pair is stored, including: node, startup, runtime. Where startup means stored in node -> params -> startup of flow json, runtime means stored in node -> params -> runtime of flow json, node means stored in node of flow json.

#### 2.2.4 Add a front-end validation rule to the input input box

```dss_workflow_node_ui_validate``` core field explanation:

- `id`, because one ```dss_workflow_node_ui_validate``` is allowed to be used by multiple ```dss_workflow_node_ui```, so the user needs to decide the id value by himself. When the user selects an id, he must ensure that the id is unique in the whole table.
- `validate_type`, the validation type, there are: None, NumInterval, FloatInterval, Include, Regex, OPF'
- `validate_range`, validation range, for example: [1,15]
- `error_msg`, the Chinese error message when the verification fails
- `error_msg_en`, the English error message when the verification fails
- `trigger`, the trigger rule for verification, including: blur, change. Among them, blur refers to losing the mouse focus, and change refers to when the content changes.

The NumInterval and FloatInterval requirements must meet the range of values limited by `validate_range`, for example: [1,15].

Include requires that the content of `validate_range` must be included.

Regex requires a regular expression that must satisfy `validate_range`.

The OPF requirement must be one of the `validate_range` array, for example: [\"apple\",\"banana\"], indicating that the value of the input control must be an apple or a banana.
