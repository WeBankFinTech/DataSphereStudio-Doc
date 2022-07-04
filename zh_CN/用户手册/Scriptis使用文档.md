Scriptis使用文档
------
## 简介
&nbsp;&nbsp;&nbsp;&nbsp;使用大数据平台的Spark、Hive和HBase等计算引擎，进行交互式查询与分析，支持数据挖掘和分析人员的日常使用。提供图形化、多样式的界面，让用户在进行数据分析、脚本编辑、测试、查询使用时更加方便，简单。
![](./images/scriptis.png)

## 工作空间
&nbsp;&nbsp;&nbsp;&nbsp;工作空间是一个文件目录，用户对该目录拥有所有的权限可以进行文件管理操作等。一般对应着Linkis服务器部署的一个文件目录，每一个登陆用户都对应着一个文件目录，存储用户脚本和结果集等文件。
![](./images/scriptis_workspace.png)
&nbsp;&nbsp;&nbsp;&nbsp;当鼠标右键工作空间文件夹时，右键功能主要包含复制路径，新建目录，新建脚本，刷新。
![](./images/scriptis_workspace_dir.png)
&nbsp;&nbsp;&nbsp;&nbsp;当鼠标右键工作空间文件夹下的文件时，脚本右键功能，脚本右键主要有打卡到侧边，复制路径，重命名，删除，导入到hive（csv,txt,excel类型文件），导入到hdfs等功能。
![](./images/scriptis_workspace_file.png)
## 数据库
数据库模块获取的是登录用户具有权限的hive库，右键库的主要功能包括刷库，刷表，刷字段信息。表右键功能-查询表，快捷生产临时hive脚本进行数据查看，复制表名和删除表即复制表字段。表右键功能查看表结构，可以展示表的字段详细信息，表详情信息，表分区信息等。
![](./images/scriptis_database.png)
## UDF和函数
 UDF功能是方便用户对UDF进行分类展示，以及用户可以对个人函数进行管理。UDF的配置管理已经移到Linkis管理台，相关配置需要参考Linkis UDF相关文档。

## HDFS
Linkis在安装后，对每个用户都默认提供了一个HDFS路径，存储用户资源文件。Scriptis会显示展现用户的HDFS文件夹，可以鼠标右键对该文件夹进行增删改，同时该路径下的文件，也可以通过右键功能进行管理。
![](./images/scriptis_hdfs.png)
