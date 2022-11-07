# 第三方系统接入 DSS 开发指南

总共分为四步：

1. 在 DSS 创建一个全新的 AppConn 目录
2. 后台实现一个全新的 AppConn；
3. 补充 AppConn 要求的相关文件；
4. 如果想作为工作流节点接入 DSS 工作流，还需完成工作流节点的新增。

具体如下：

## 一、在 DSS 创建一个全新的 AppConn 目录

新 AppConn 的目录格式要求如下，请依照要求的格式创建一个新的 AppConn module：

```html

|-- DataSphereStudio    
|   |-- dss-appconn  // AppConn 框架的根目录
|   |   |-- appconns // 所有 AppConn 的存放目录
|   |   |   |-- dss-<appconnName>-appconn   // 新增的 AppConn 的根目录
|   |   |   |   |-- src
|   |   |   |   |   |-- main
|   |   |   |   |   |   |-- assembly  // 打成 DSS 可加载的 appconnName.zip 包的配置目录
|   |   |   |   |   |   |   |-- distribution.xml   // 打包的配置文件
|   |   |   |   |   |   |-- icons   // 如果想作为工作流节点，这里用于存放各个工作流节点的图标
|   |   |   |   |   |   |-- java    // AppConn代码的存放目录
|   |   |   |   |   |   |-- resources   // 该 AppConn 的资源文件存放目录
|   |   |   |   |   |   |   |-- appconn.properties   // 该 AppConn 的配置文件，必须命名为 appconn.properties
|   |   |   |   |   |   |   |-- init.sql   // 往 DSS 数据库中导入 AppConn 信息的 dml sql 文件
|   |   |   |   |-- pom.xml  // 新增的 AppConn 的 pom文件

```

## 二、后台实现一个全新的 AppConn

请参考：[AppConn 开发指南](AppConn开发指南.md)。

## 三、补充 AppConn 要求的相关文件

主要分为以下几步：

- 补充 pom.xml 文件
- 补充 icons 文件
- 补充 init.sql
- 补充 distribution.xml
- appconn.properties 的作用

### 3.1 补充 pom.xml 文件

请参考：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>dss</artifactId>
        <groupId>com.webank.wedatasphere.dss</groupId>
        <version>1.1.0</version>
        <relativePath>../../../pom.xml</relativePath>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>dss-${appconnName}-appconn</artifactId>

    <dependencies>
        <!-- 以下是必须的 DSS dependency -->
        <dependency>
            <groupId>com.webank.wedatasphere.dss</groupId>
            <artifactId>dss-appconn-core</artifactId>
            <version>${dss.version}</version>
            <exclusions>
                <exclusion>
                    <artifactId>linkis-common</artifactId>
                    <groupId>org.apache.linkis</groupId>
                </exclusion>
                <exclusion>
                    <artifactId>json4s-jackson_2.11</artifactId>
                    <groupId>org.json4s</groupId>
                </exclusion>
            </exclusions>
        </dependency>

        <!-- 如果想接入 DSS 工作流，请加上以下 dependency -->
        <dependency>
            <groupId>com.webank.wedatasphere.dss</groupId>
            <artifactId>dss-development-process-standard-execution</artifactId>
            <version>${dss.version}</version>
        </dependency>

        <!-- 以下 dependency 可按需引用 -->
        <dependency>
            <groupId>org.apache.linkis</groupId>
            <artifactId>linkis-cs-client</artifactId>
            <version>${linkis.version}</version>
            <scope>compile</scope>
        </dependency>

        <dependency>
             <groupId>org.apache.linkis</groupId>
             <artifactId>linkis-httpclient</artifactId>
             <version>${linkis.version}</version>
             <exclusions>
                  <exclusion>
                       <artifactId>linkis-common</artifactId>
                       <groupId>org.apache.linkis</groupId>
                  </exclusion>
                  <exclusion>
                       <artifactId>json4s-jackson_2.11</artifactId>
                       <groupId>org.json4s</groupId>
                  </exclusion>
             </exclusions>
        </dependency>


    </dependencies>

    <build>
        <plugins>
            <!-- 该 plugin 可直接引用 -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-deploy-plugin</artifactId>
            </plugin>
            <!-- 如果有 Scala 代码，需加上该 plugin -->
            <plugin>
                <groupId>net.alchim31.maven</groupId>
                <artifactId>scala-maven-plugin</artifactId>
            </plugin>
            <!-- 该 plugin 可直接引用 -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-jar-plugin</artifactId>
            </plugin>
            <!-- 该 plugin 可直接引用 -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-assembly-plugin</artifactId>
                <version>2.3</version>
                <inherited>false</inherited>
                <executions>
                    <execution>
                        <id>make-assembly</id>
                        <phase>package</phase>
                        <goals>
                            <goal>single</goal>
                        </goals>
                        <configuration>
                            <descriptors>
                                <descriptor>src/main/assembly/distribution.xml</descriptor>
                            </descriptors>
                        </configuration>
                    </execution>
                </executions>
                <configuration>
                    <skipAssembly>false</skipAssembly>
                    <finalName>out</finalName>
                    <appendAssemblyId>false</appendAssemblyId>
                    <attach>false</attach>
                    <descriptors>
                        <descriptor>src/main/assembly/distribution.xml</descriptor>
                    </descriptors>
                </configuration>
            </plugin>
        </plugins>
        <resources>
            <!-- 该 resource 可直接引用 -->
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.xml</include>
                </includes>
            </resource>
            <!-- 该 resource 可按需配置 -->
            <resource>
                <directory>src/main/resources</directory>
                <excludes>
                    <exclude>**/application.yml</exclude>
                    <exclude>**/bootstrap.yml</exclude>
                    <exclude>**/log4j2.xml</exclude>
                </excludes>
            </resource>
        </resources>
    </build>
</project>
```

### 3.2 icons 文件

如果该 AppConn 想作为工作流节点接入 DSS 工作流，则相关工作流节点的图标，需存储在 ```dss-appconn/appconns/dss-<appconnName>-appconn/src/main/icons``` 目录。

您在新增图标时，请直接复制图标的代码，放入到文件之中。如：新建 widget.icon 文件，并复制相关 SVG 代码到该文件中。

### 3.3 补充 init.sql

这里只给出了一个外部系统接入到 DSS，可在顶部菜单栏中使用的基本 dml sql 的示例，如您想将 AppConn 作为工作流节点进行接入，还需参考 [DSS工作流如何新增工作流节点](DSS工作流如何新增工作流节点.md)。

如下所示：

```mysql-sql

select @old_dss_appconn_id:=id from `dss_appconn` where `appconn_name` = '具体的AppConnName，如：Visualis';

delete from  `dss_workspace_menu_appconn` WHERE `appconn_id` = @old_dss_appconn_id;
delete from `dss_appconn_instance` where `appconn_id` = @old_dss_appconn_id;
delete from  `dss_appconn` where `appconn_name`='具体的AppConnName，如：Visualis';

-- 除了 appconn_class_path 和 resource 用户无需关注，其他字段都需关注
-- 下面有做详细介绍
INSERT INTO `dss_appconn` (`appconn_name`, `is_user_need_init`, `level`, `if_iframe`, `is_external`, `reference`, `class_name`, `appconn_class_path`, `resource`) VALUES ('具体的AppConnName，如：Visualis', 0, 1, NULL, 0, NULL, 'com.webank.wedatasphere.dss.appconn.visualis.VisualisAppConn', '', '');

select @dss_appconn_id:=id from `dss_appconn` where `appconn_name` = '具体的AppConnName，如：Visualis';

-- 用户需关注的字段：menu_id、title_en、title_cn、desc_en、desc_cn、labels_en、labels_cn、is_active、access_button_en、access_button_cn、manual_button_url
-- 下面有做详细介绍
INSERT INTO `dss_workspace_menu_appconn`(`appconn_id`,`menu_id`,`title_en`,`title_cn`,`desc_en`,`desc_cn`,`labels_en`,`labels_cn`,`is_active`,`access_button_en`,`access_button_cn`,`manual_button_en`,`manual_button_cn`,`manual_button_url`,`icon`,`order`,`create_by`,`create_time`,`last_update_time`,`last_update_user`,`image`) 
    values(@dss_appconn_id,2,'appconn English title','这是引擎中文名','This is English description.','这是中文描述。','label1,label2','标签1,标签2','1','enter AppConnName1','进入引擎1','user manual','用户手册','请补充用户手册地址',NULL,10,'system',NULL,'system',NULL,NULL);

-- 用户需关注的字段：enhance_json、homepage_uri
-- 下面有做详细介绍
INSERT INTO `dss_appconn_instance` (`appconn_id`, `label`, `url`, `enhance_json`, `homepage_uri`) VALUES (@dss_appconn_id, 'DEV', 'http://APPCONN_INSTALL_IP:APPCONN_INSTALL_PORT/', '', '');

```

#### 3.3.1 dss_appconn 表

```dss_appconn``` 表是 DSS 的核心表，除了 appconn_class_path 和 resource 用户无需关注，其他字段都需关注。

具体关注点如下：

- ```is_user_need_init```，用户是否需要初始化，该字段为保留字段，直接设置为 0 即可
- ```if_iframe```，DSS 是否可以通过 Iframe 嵌入的方式打开第三方 AppConn 页面，1 为可以通过 Iframe 方式打开，0 为通过前端路由的方式打开；
- ```is_external```，打开第三方 AppConn 的页面时，是否通过打开新的浏览器 Tab 的方式打开，1 为打开新的浏览器 Tab 打开，0 为在本页面打开
- ```reference```，关联的 AppConn。一些 AppConn 想直接复用已实现 AppConn，可直接用该字段指定对应的 AppConn 名。例如：某个 AppConn 只想与 DSS 打通 免密跳转（即一级规范），则该字段可直接填 sso (即关联到 SSOAppConn)； 
- ```class_name```，该 AppConn 的主类。如果 ```reference``` 不为空，则该字段为空。

#### 3.3.2 dss_workspace_menu_appconn 表

```dss_workspace_menu_appconn``` 表需要用户关注的点，如下：

- 其他字段无需改动，可按需修改相关字段：menu_id、title_en、title_cn、desc_en、desc_cn、labels_en、labels_cn、is_active、access_button_en、access_button_cn、manual_button_url
- ```menu_id```，即顶部菜单栏的分类 id，目前分为：1-数据交换；2-数据分析；3-生产运维；4-数据质量；5-管理员功能；6-数据应用
- 如果已有的 ```menu_id``` 无法满足您的需求，您可以自己往 ```dss_workspace_menu``` 表中插入新的菜单分类。
- ```is_active```，即该 AppConn 是否可以被访问，如果为 1 表示可用；为 0 表示 敬请期待
- ```manual_button_url```，用户手册的 URL 地址

#### 3.3.2 dss_appconn_instance 表

```dss_appconn_instance``` 表需要用户关注的点，如下：

- ```enhance_json```，AppConn 的额外参数，为 `map json` 格式的字符串，与 `appconn.properties` 作用相同，都是为该 AppConn 配置额外参数。
- ```homepage_uri```，主页的 URI。请注意，是 URI，不是 URL。例如：如果主页 URL 为：```http://ip:port/test/home```，则主页 URI 为：test/home

这里介绍一下 `enhance_json` 或 `appconn.properties` 在 DSS 之中的一个使用场景。

例如：`enhance_json` 的内容为： `{"reqUri": "/api/v1/redirect"}`

`reqUri`：(非必须属性) 用于如果第三方应用工具集成了 DSS 一级规范，当在 DSS 的顶部菜单栏点击跳转到第三方应用工具的首页时，需要用到该属性。
 
`reqUri` 用于指定一个 RESTFUL 请求的 URI，该 URI 可访问第三方系统后台的某个 RESTFUL 接口（Restful 接口无要求，能访问即可），DSS 放置在第三方应用工具的一级规范 Jar 包会自动拦截该请求，
加上用户态后自动重定向给实际的前端首页。

更多细节可参考 [DSS 一级规范](https://github.com/WeBankFinTech/DataSphereStudio-Doc/blob/main/zh_CN/%E5%BC%80%E5%8F%91%E6%96%87%E6%A1%A3/AppConn%E5%BC%80%E5%8F%91%E6%8C%87%E5%8D%97.md#21-onlyssoappconn--%E6%89%93%E9%80%9A-sso-%E5%85%8D%E7%99%BB%E5%BD%95%E8%B7%B3%E8%BD%AC)

### 3.4 distribution.xml 文件

下面的参考样例，除了 appconnName 需要用户按实际名称修改以外，其他都无需修改，可直接拷贝使用。

请按需拷贝使用，具体请参考：

```xml
<assembly
        xmlns="http://maven.apache.org/plugins/maven-assembly-plugin/assembly/2.3"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://maven.apache.org/plugins/maven-assembly-plugin/assembly/2.3 http://maven.apache.org/xsd/assembly-1.1.2.xsd">
    <!-- 这里需保证 id 唯一 -->
    <id>dss-appconnName-appconn</id> 
    <formats>
        <format>dir</format>
    </formats>
    <includeBaseDirectory>true</includeBaseDirectory>
    <!-- 具体的 AppConn 名 -->
    <baseDirectory>appconnName</baseDirectory>

    <dependencySets>
        <!-- 如果有 dependency，则需 lib 目录 -->
        <dependencySet>
            <outputDirectory>lib</outputDirectory>
            <useProjectArtifact>true</useProjectArtifact>
            <useTransitiveDependencies>true</useTransitiveDependencies>
            <unpack>false</unpack>
            <useStrictFiltering>true</useStrictFiltering>
            <useTransitiveFiltering>true</useTransitiveFiltering>
        </dependencySet>
    </dependencySets>

    <fileSets>
        <!-- 如果有 appconn.properties，则需 conf 目录 -->
        <fileSet>
            <directory>${basedir}/conf</directory>
            <includes>
                <include>*</include>
            </includes>
            <fileMode>0777</fileMode>
            <outputDirectory>conf</outputDirectory>
            <lineEnding>unix</lineEnding>
        </fileSet>
        <!-- 存放 init.sql -->
        <fileSet>
            <directory>${basedir}/src/main/resources</directory>
            <includes>
                <include>init.sql</include>
            </includes>
            <fileMode>0777</fileMode>
            <outputDirectory>db</outputDirectory>
        </fileSet>
        <!-- 存放工作流节点的 icons -->
        <fileSet>
            <directory>${basedir}/src/main/icons</directory>
            <includes>
                <include>*</include>
            </includes>
            <fileMode>0777</fileMode>
            <outputDirectory>icons</outputDirectory>
        </fileSet>
    </fileSets>
</assembly>
```

### 3.5 appconn.properties 的作用

appconn.properties 用于配置该 AppConn 额外的参数，如果您的 AppConn 没有定义任何的参数，您也可以不创建 appconn.properties 文件。

appconn.properties 与 ```dss_appconn_instance``` 表的 ```enhance_json``` 字段作用相同。

使用 appconn.properties 的好处是：

- ```enhance_json``` 是一个 map json 格式的字符串，修改有一定的门槛，需要做转换才能使用；
- 修改 ```enhance_json``` 需要改动数据库，远没有直接改 AppConn 配置文件来的简单。

我们推荐您使用 appconn.properties 来配置 AppConn 的参数，您也可以按照您的实际需要去选择使用其中的某种方式。

## 四、工作流节点的新增

请参考：[DSS 工作流如何新增工作流节点](DSS工作流如何新增工作流节点.md)。

## 五、DSS 如何加载新的 AppConn

使用 ```install-appconn.sh``` 来安装新的 AppConn，如下：

```shell script
  cd $DSS_HOME/bin
  sh install-appconn.sh
```

如果是已存在的 AppConn，当 AppConn 有改动时，请使用 ```appconn-refresh.sh``` 来刷新该 AppConn，如下：

```shell script
  cd $DSS_HOME/bin
  sh appconn-refresh.sh
```

请注意：您需提前将打包好的 AppConn zip 放到 AppConn 的 安装目录。

## 六、AppConn 贡献

**如果您想将 AppConn 插件贡献给社区，请您在实现该 AppConn 前先与社区取得联系并沟通确认**。且在发版前，您还需编写 `AppConn插件安装文档`

关于插件安装文档，可参考： [VisualisAppConn 插件安装文档](https://github.com/WeBankFinTech/Visualis/blob/master/visualis_docs/zh_CN/Visualis_appconn_install_cn.md)
         
## 七、DSS 如何使用 AppConn 和第三方应用进行交互
     
DSS 同第三方应用系统的交互都是通过相应的 AppConn 实例来完成的。

当 AppConn 被部署到指定目录后，```dss-framework-project-server``` 服务启动时会加载或**刷新**所有的 AppConn，并把它存入物料库。

其它的 DSS 微服务将从 ```dss-framework-project-server``` 中按需拉取所有 AppConn 的物料，并动态加载到服务进程，作为 AppConn 实例被调用。

![DSS框架设计](../Images/开发文档/第三方系统如何接入DSS/DSS框架设计.png)