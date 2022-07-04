## 一、后台新增

在 ```linkis-computation-governance\linkis-manager\label-common\src\main\java\com\webank\wedatasphere\linkis\manager\label\entity\engine\EngineType.scala``` 中，
新增一个全新的枚举引擎类型。

关于如何实现一个全新的 Linkis 引擎，请参考：[如何实现一个新引擎](https://linkis.apache.org/zh-CN/docs/latest/development/new_engine_conn)。

## 二、前端新增

找到 ```web/src/common/config/scriptis.js``` 这个文件，在其数组上新增一条记录即可。

该记录是 object 类型，可配置以下属性：

- rule：正则表达式，用于匹配符合正则的文件名后缀；
- executable：该脚本类型是否可被执行；
- lang：在 monaco editor 编辑器中使用哪种语言，对应其高亮、联想词等功能。可参考 monaco editor 官网支持的语言，或者自定义语言；
- application：与 Linkis 某个引擎类型对应，用于在 websocket 或者 http 轮询时的传递参数 executeApplicationName；
- runType：与 Linkis 某个引擎的脚本类型对应，用于在 websocket 或者 http 轮询时的传递参数 runType；
- ext：该脚本类型的文件后缀，如：```.hql```；
- scriptType：系统支持的脚本类型，用于在新建脚本时作判断使用，该属性是由于 application 和 runType 均可能出现重复，所以用于在前端区分脚本类型；
- abbr：该脚本类型的后缀名（该属性已废弃，后续会删除）；
- label：在用户新建脚本 UI 界面上，显示的脚本类型名称，首字母大写；
- logo：该脚本的图标。存放路径为：```web/src/apps/scriptis/assets/styles/home.scss```，请在文件最后加上新的LOGO。
- isCanBeNew：是否允许被新建，用于在工作空间等可新建脚本的场景中显示；
- isCanBeOpen：是否允许被打开，用于在工作空间等模块中可被双击或右键打开（该属性是在前台生效，后台也有对脚本是否可打开做校验）；
- flowType：工作流中使用的类型。
