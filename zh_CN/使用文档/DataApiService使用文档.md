数据服务
----------

DSS目前支持将SQL脚本发布成数据服务API，分享给其他用户使用。业务用户可以在不用写代码和无大数据平台用户的情况下，设置参数，就可以执行数据服务的脚本，并直接浏览或者下载平台数据。

![](../Images/apiservice/c65f98286cd82f3d9100f602a31f4302.png)

**1、创建数据服务**

由于业务的需要，需要建立一个数据服务，授权给他人使用，当发布用户进入Scriptis，编辑新建脚本文件，编写SQL语句，同时可以在SQL语句中嵌入变量，方便后续的业务人员自己设置参数后就可以获取数据。

![](../Images/apiservice/b7f2ba78f56e660c9345895205cd47ed.png)


在保存该Spark SQL脚本后，可在脚本编辑栏上方点击“发布为数据API”（该“发布为数据API”的功能，只有部分用户具有该权限，不具备权限的用户，该功能按钮不可见），在点击后需要填如以下API创建信息（其中带红色
\* 标记为必填项），新建API信息填写如下图所示。

![](../Images/apiservice/105c29559c6ff03db92efc7cc0b7d15d.png)

点击下一步，用于设置变量的信息。
![](../Images/apiservice/c44c3ee7da22fdd19eb62379271a8410.png)


发布用户可以在工作空间的主页，通过“进入数据服务开发”，和“更多应用工具”进入使用数据服务，在数据服务的标签点击“更多”，可以进入数据服务管理界面

![](../Images/apiservice/150207df2ea89d2b0c2f3ccb8d46577b.png)



**2、使用数据服务**

在进入数据服务页面后，可以看到该用户可使用的数据服务列表页面，其中default表示默认所有的数据服务，用户可以点击对应的标签筛选出自己需要使用的数据服务，同时可以在搜索框中，分别用名称、状态、提交人进行筛选，详情页面如下图所示。

![](../Images/apiservice/50bd31ef9795efe92aa333f7ac8518c6.png)

使用用户点击“进入使用”，可以在筛选条件中，设置参数的值，由此可见，使用用户是发布用户数据集的一个子集。

![](../Images/apiservice/dfbf44e1fe710b76883fe0eb24346707.png)

**3、修改数据服务**

 一个数据服务由于业务的需要可能发生修改，当发布用户对数据服务的脚本进行了修改，可以点击“更新数据API”。
更新数据服务，可以选择绑定的其他数据服务。

![](../Images/apiservice/ff7b18b4eec06f5dfd2da1e3693e2e59.png)

**4、使用postman访问数据服务**

数据服务发布后，支持使用api接口访问，可以给其它系统直接调用。提交查询如下图所示：

![](../Images/apiservice/postman01.png)

获取到任务执行ID, 再根据ID可以获取任务的执行进度，日志，结果集等。

![](../Images/apiservice/postman02.png)

说明：数据服务的token可以从/queryById?的接口返回(点击进入使用)，字段为userToken.  所有接口的访问都是要经过GateWay认证的。数据服务的token只能用于数据服务的管理流程。使用postman的认证需要使用页面的cookie后者走linkis-gateway的秘钥认证方式。在head 里面加入Token-Code: XXX   这里指定linkis-gateway的登录秘钥   Token-User: XXX   这里指定linkis-gateway的登录用户。


