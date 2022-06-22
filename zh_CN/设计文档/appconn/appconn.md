DSS-AppConn设计文档
------
## 简介
&nbsp;&nbsp;&nbsp;&nbsp;原始的AppJoint的原理，定义一个顶层接口AppJoint，第三方通过实现该接口，并把自身的连接信息存在DSS表中，在DSS中实现一个与第三方系统通信的一个"代理服务"，在初始化初期，通过反射的机制创建该服务的实例，并利用表中的连接信息，可以实现DSS利用该"代理服务"与第三方系统建立HTTP通信，从而调用第三方系统。但在AppJoint的设计上，存在不足，每个接入的应用实例，都需要生成一个AppJoint实例，同一个应用的不同实例并没有逻辑上关联起来，每个系统的应用实例AppConn是DSS1.0的顶层接口，在DSS1.0中，自身的编排模式、工作流，单任务节点等，都是一个AppConn的实例，除此之外，接入DSS的第三方系统，需要实现AppConn接口，来实现DSS与第三方系统的融合，从而实现调用第三方应用，逻辑上，AppConn比AppJoint的抽象逻辑更高，AppConn类似于一类实例，而AppJoint类似于一个实例。

### 相关模块介绍
|一级模块| 二级模块|功能介绍|
|-------------|-----------|----------------|
|dss-appconn|appconns|接入DSS实现AppConn相关规范实现代码|
|           |dss-appconn-core|appconn接口及基础类定义|
|           |dss-appconn-loader|接入的应用AppConn编译包的实例化加载组装|
|           |dss-appconn-manager|与framework模块交互，管理相关AppConn实例信息|
|           |dss-scheduler-appconn|对调度系统实现的抽象AppConn定义|
|           |linkis-appconn-engineplugin|实现linkis appconn的相关规范，打通DSS AppConn和Linkis的交互|  



| 核心接口/类              | 核心功能                            |
|---------------------------|------------------------------|
| DSSDictionaryRestful、DSSDictionaryServiceImpl           |  提供字典信息获取接口，通过参数key或parentKey从dictionary表查询对应记录     |
| DSSWorkspacePrivRestful、DSSWorkspacePrivServiceImpl    | 提供对工作空间角色的菜单组件权限信息的查看、编辑功能                 |
| DSSWorkspaceRestful、DSSWorkspaceServiceImpl            | 提供工作空间基本功能接口，如创建工作空间、获取工作空间列表、获取菜单组件权限信息等                 |
| DSSWorkspaceRoleRestful、DSSWorkspaceRoleServiceImpl   | 提供工作空间角色的查询、创建接口              |
| DSSWorkspaceUserRestful、DSSWorkspaceUserServiceImpl   | 提供工作空间用户的增删改查接口           |

### AppConn架构图
![](./images/appconn_class_uml.png)
![](./images/appconn_structure.png)  
![](./images/appconn_load_process.png)
