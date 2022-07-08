## 1. New background

In ```linkis-computation-governance\linkis-manager\label-common\src\main\java\com\webank\wedatasphere\linkis\manager\label\entity\engine\EngineType.scala```,
Added a new enumeration engine type.

For how to implement a new Linkis engine, please refer to: [How to implement a new engine](https://linkis.apache.org/zh-CN/docs/latest/development/new_engine_conn).

## 2. New front-end

Find the ``web/src/common/config/scriptis.js``` file and add a new record to its array.

The record is of type object and can be configured with the following properties:

- rule: regular expression, used to match regular file name suffixes;
- executable: whether the script type can be executed;
- lang: Which language is used in the monaco editor, corresponding to its highlighting, associative words and other functions. You can refer to the languages ​​supported by the official website of monaco editor, or customize the language;
- application: corresponds to an engine type of Linkis and is used to pass the parameter executeApplicationName during websocket or http polling;
- runType: corresponds to the script type of a Linkis engine, used to pass the parameter runType during websocket or http polling;
- ext: the file suffix of the script type, such as: ````.hql```;
- scriptType: The script type supported by the system, which is used for judgment when creating a new script. This attribute is used to distinguish script types at the front end because both application and runType may be duplicated;
- abbr: the suffix name of the script type (this attribute is deprecated and will be deleted later);
- label: The name of the script type displayed on the UI interface of the user's new script, with the first letter capitalized;
- logo: The script's icon. The storage path is: ```web/src/apps/scriptis/assets/styles/home.scss```, please add a new LOGO at the end of the file.
- isCanBeNew: Whether it is allowed to be newly created, which is used to display in scenarios where new scripts can be created, such as workspaces;
- isCanBeOpen: Whether it is allowed to be opened, it can be double-clicked or right-clicked to open in modules such as workspace (this property is valid in the foreground, and the background also checks whether the script can be opened);
- flowType: The type used in the workflow.