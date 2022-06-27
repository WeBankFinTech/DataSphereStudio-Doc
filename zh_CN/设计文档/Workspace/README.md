介绍
-------------------------
Workspace：工作空间模块，属于project服务，提供工作空间所有功能的接口实现。主要包含这些接口：获取字典表信息接口、工作空间菜单、组件权限的crud操作接口，
工作空间角色、用户的crud操作接口。工作空间左上角菜单栏组件收藏接口。

用户使用功能点：

| 一级模块      | 二级模块                  |       用户功能          |
|-------------|-----------               |----------------       |
|   工作空间    | 工作空间管理（仅工作空间管理员） |    查询、添加、编辑、删除工作空间用户    |
|             |                          |    工作空间角色的菜单组件的权限设置    |
|             |                         |    工作空间角色的首页设置             |
|             | 首页左上角菜单栏           |    收藏组件至左侧列表               |
|             |                         |    订阅已收藏组件，会展示在上方导航栏    |
|             |                         |    点击组件跳转至对应组件页面          |

### Restful/Service类功能介绍：

| 核心接口/类              | 核心功能                            |
|---------------------------|------------------------------|
| DSSDictionaryRestful、DSSDictionaryServiceImpl           |  提供字典信息获取接口，通过参数key或parentKey从dictionary表查询对应记录     |
| DSSWorkspacePrivRestful、DSSWorkspacePrivServiceImpl    | 提供对工作空间角色的菜单组件权限信息的查看、编辑功能                 |
| DSSWorkspaceRestful、DSSWorkspaceServiceImpl            | 提供工作空间基本功能接口，如创建工作空间、获取工作空间列表、获取菜单组件权限信息等                 |
| DSSWorkspaceRoleRestful、DSSWorkspaceRoleServiceImpl   | 提供工作空间角色的查询、创建接口              |
| DSSWorkspaceUserRestful、DSSWorkspaceUserServiceImpl   | 提供工作空间用户的增删改查接口           |

### 用户功能UML类图：
![](images/workspace_uml.drawio.png)

## 数据库表设计

工作空间基本信息表：
```roomsql
CREATE TABLE `dss_workspace` (
`id` bigint(20) NOT NULL AUTO_INCREMENT,
`name` varchar(255) DEFAULT NULL COMMENT '工作空间名',
`label` varchar(255) DEFAULT NULL COMMENT '标签',
`description` varchar(255) DEFAULT NULL  COMMENT '描述',
`create_by` varchar(255) DEFAULT NULL COMMENT '创建者',
`create_time` datetime DEFAULT NULL COMMENT '创建时间',
`department` varchar(255) DEFAULT NULL COMMENT '部门id',
`product` varchar(255) DEFAULT NULL COMMENT '产品，预留字段',
`source` varchar(255) DEFAULT NULL COMMENT '预留字段',
`last_update_time` datetime DEFAULT NULL,
`last_update_user` varchar(30) DEFAULT NULL COMMENT '最新修改用户',
`workspace_type`  varchar(20) DEFAULT NULL comment '工作空间类型',
PRIMARY KEY (`id`),
UNIQUE KEY `name` (`name`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```
工作空间字典表：
```roomsql
CREATE TABLE `dss_workspace_dictionary` (
  `id` int(11) NOT NULL AUTO_INCREMENT COMMENT '主键ID',
  `workspace_id` int(11) DEFAULT '0' COMMENT '空间ID，默认为0，所有空间都有',
  `parent_key` varchar(200) DEFAULT '0' COMMENT '父key',
  `dic_name` varchar(200) NOT NULL COMMENT '名称',
  `dic_name_en` varchar(300) DEFAULT NULL COMMENT '名称（英文）',
  `dic_key` varchar(200) NOT NULL COMMENT 'key 相当于编码，空间是w_开头，工程是p_',
  `dic_value` varchar(500) DEFAULT NULL COMMENT 'key对应的值',
  `dic_value_en` varchar(1000) DEFAULT NULL COMMENT 'key对应的值（英文）',
  `title` varchar(200) DEFAULT NULL COMMENT '标题',
  `title_en` varchar(400) DEFAULT NULL COMMENT '标题（英文）',
  `url` varchar(200) DEFAULT NULL COMMENT 'url',
  `url_type` int(1) DEFAULT '0' COMMENT 'url类型: 0-内部系统，1-外部系统；默认是内部',
  `icon` varchar(200) DEFAULT NULL COMMENT '图标',
  `order_num` int(2) DEFAULT '1' COMMENT '序号',
  `remark` varchar(1000) DEFAULT NULL COMMENT '备注',
  `create_user` varchar(100) DEFAULT NULL COMMENT '创建人',
  `create_time` datetime DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  `update_user` varchar(100) DEFAULT NULL COMMENT '更新人',
  `update_time` datetime DEFAULT CURRENT_TIMESTAMP COMMENT '更新时间',
  PRIMARY KEY (`id`),
  UNIQUE KEY `idx_unique_workspace_id` (`workspace_id`,`dic_key`),
  KEY `idx_parent_key` (`parent_key`),
  KEY `idx_dic_key` (`dic_key`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='数据字典表';
```
组件菜单信息表
```roomsql
CREATE TABLE `dss_workspace_menu` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(64) DEFAULT NULL COMMENT '菜单名',
  `title_en` varchar(64) DEFAULT NULL COMMENT '菜单英文标题',
  `title_cn` varchar(64) DEFAULT NULL COMMENT '菜单中文标题',
  `description` varchar(255) DEFAULT NULL COMMENT '描述',
  `is_active` tinyint(1) DEFAULT '1' COMMENT '是否启用',
  `icon` varchar(255) DEFAULT NULL COMMENT '图标',
  `order` int(2) DEFAULT NULL COMMENT '顺序',
  `create_by` varchar(255) DEFAULT NULL COMMENT '创建者',
  `create_time` datetime DEFAULT NULL COMMENT '创建时间',
  `last_update_time` datetime DEFAULT NULL,
  `last_update_user` varchar(30) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```