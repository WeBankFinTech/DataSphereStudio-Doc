# DSS-UserGuide模块设计

### 引言

DSS用户手册模块是DSS1.0新增的功能模块，用于给DSS的用户提供使用指引，里面录入了许多DSS使用过程中遇到的问题和解决方法，也包含了一些功能点使用说明。用户能够自助式的搜索遇到问题相关解决方案。后期也可以用来关联错误码，支持在弹出的错误码后，直接定位到知识库中已经录入的解决方案。guide模块是将文件以html的方式存放在表的字段中，需要解析md文件并转化为html, 由于某些文件存在链接需要跳转，需要另外搭建gitbook用于展示和管理这些文档，为了能够高效同步dss-guide-user模块，采取将gitLab上的文件打包，然后上传解压到gitbook所在服务器的指定目录下，通过guide-user定时扫描指定目录从而达到同步的目的。

## dss_guide主要模块介绍

DSS_Guide模块主要包含了Restful、Service、Dao、Entity的定义。

### GuideGroupService

用来解决GuideGroup的增加、查询、修改、保存、删除等能力，还有具备同步SUMMARY.md的能力。guide模块可以通过解析此文件，然后根据解析出的各级目录在文件中的配置路径，定位到需要读取的文件并向数据库定时写入，从而完成同步，当服务在其他服务器上运行时，为了避免重复安装gitbook，guide模块需要通过配置文件所在服务器ip，然后会自动将文件同步到guide模块所在服务器进行展示。

### GuideContentService

用来处理GuideContent的保存、查询、更新、删除操作。

### GuideChapterService

用来专门处理手册章节的具体内容，包含章节的搜索、基于ID的查询、删除、保存等。

### GuideCatalogService

用来实现知识库的同步、支持批量插入目录内容，并实现对目录结构分类的保存、删除、查询等操作


### 核心流程图

![1653309535303.png](../../images/DSS-UserGuide模块设计/1653309535303.png)

![](../../images/DSS-UserGuide模块设计/1653309707841.png)


### 数据结构

![](../../images/DSS-UserGuide模块设计/1653309930194.png)

### dss_guide_group

划分dss_guide 的分组，包含group_id、path（访问路径）、title等

### dss_guide_chapter

用来存储dss_guide章节的详细内容，包含catalog_id、title、content、content_html。和dss_guide_catalog的内容进行关联。

### dss_guide_content

用来存储分组后的说明内容，会规划到对应的group下面。包含title、type、content、content_html等

### dss_guide_catalog

用来对dss_guide进行内容分类，相当于知识库的目录结构，具有层级目录关系。
