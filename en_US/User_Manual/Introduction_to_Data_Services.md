DSS Plugin - Data Api Services
----------
## motivation
&nbsp;&nbsp;&nbsp;&nbsp;In actual business, business personnel and data developers usually need to be connected, and business personnel are usually not familiar with data development, so when there is data demand, they usually need to communicate with data personnel. Data developers usually develop data according to the needs of the business side, and then complete the requirements by sharing scripts or data.
For data extraction, there are many scenarios in actual production, but most of the main structures are similar. Next, the scenarios of data provision will be described according to several aspects.
## traditional data extraction
&nbsp;&nbsp;&nbsp;&nbsp;There are two ways when the business side provides the requirements, and the data developer develops the script for fetching data and hands over with the business side.
- The first type: directly share the script for fetching data to business personnel (usually requiring shared storage or other methods), and the business personnel run the script to obtain data by themselves. The advantages and disadvantages of this method are as follows:
  Advantages: suitable for some temporary needs, the script is usually only used once or several times, which is more convenient.
  Disadvantage: Because business personnel also need to execute scripts, business personnel need permissions for the libraries and tables involved in the script. There is a risk of business people tampering with the script.
- The second type: Data developers run the fetching scripts themselves, and then share the data with business personnel. The advantages and disadvantages of this method are as follows:
  Advantages: suitable for some temporary needs, the script is usually only used once or several times, which is more convenient.
  Disadvantages: There is a risk of security compliance, and data that the business does not have permission to view is at risk of being leaked.
## Data View
&nbsp;&nbsp;&nbsp;&nbsp;Data views are similar to data services, but there are some differences in the process. When business users need data, they first put forward the demand, and then apply for the permission to use the data with the bill of lading. After the bill of lading is completed, the data developer can develop the demand and publish the developed script to the business user.
Due to the security requirements in the industry, access to data without permission requires Bill of Lading authorization. So the advantages and disadvantages of data views are similar to data services.
The difference is that the B/L application of the data view is initiated by the business user.
## Data Service (Api Service)
&nbsp;&nbsp;&nbsp;&nbsp;Data service is a service provided to users with database table permissions, and it can also prevent tampering of published scripts. In terms of business, the way of data service, the business side first initiates the demand, and then the data developer develops the script and publishes it as a data service.
When publishing data services, the process of authorizing business users to use them requires authorization according to the company's security requirements. The bill of lading request is initiated by the data developer. After the authorization is passed, business users can use the data service. There are the following problems with the way the data is served.
Advantages: Scripts suitable for some fixed requirements, more convenient for scenarios where the script is used multiple times. At the same time, it can prevent tampering with the script. Business users can access data without granting database table-level permissions. It is more friendly, and business users can fetch numbers with simple operations.
## Data Services and Data Views
&nbsp;&nbsp;&nbsp;&nbsp;The data view trial scenario is generally that the business side initiates a demand for data development from top to bottom. Before the demand is provided, the data development does not know the required library table permissions and the specific content of the development.
The data service is different. In some relatively fixed business scenarios, the data service is initiated from the bottom to the top, and the data developer specifies the set of data access. The business side uses this script, and the data that the business side can view can only be provided by data development. subset of the dataset.
## how to use
### compile
```shell
# Compiled and packaged by DSS
mvn -N install
mvn -DskipTests=true clean package
```
&nbsp;&nbsp;&nbsp;&nbsp;After the packaged tar package is installed and decompressed, the corresponding lib package can be found in dss-apps. If you want to extract the service, you can package it separately, and the related startup scripts and configurations need to be stripped and modified. Currently, it is recommended to compile and package with DSS.
### Startup and shutdown
```shell
# 1. Use batch start and stop scripts
sh sbin/start-all.sh
sh sbin/stop-all.sh

# 2. Start and stop the service individually
sh sbin/dss-daemon.sh start dss-datapipe-server
sh sbin/dss-daemon.sh stop dss-datapipe-server
sh sbin/dss-daemon.sh restart dss-datapipe-server
```
### Related Restrictions
&nbsp;&nbsp;&nbsp;&nbsp;Currently, DSS data services can only be used within DSS Scritis. Before using, you need to pay attention to the following:
1. Only Spark SQL scripts are supported to be published as data services.
2. The script parameters of the data service need to be configured with the ${params} parameter in Scritis.
3. After publishing the data service, you can view the published data service in the data service module.
4. Only users with data service permissions in the workspace configuration can use the data service function.

### Data Service Architecture
&nbsp; &nbsp; &nbsp; List. Only the publisher or proxy user has permissions on the entire table. The architecture diagram of the data service is as follows:


### Data service usage
&nbsp;&nbsp;&nbsp;&nbsp;Data Api Service is a part of DSS plug-ins and a part of DSS ecosystem. For the usage instructions of data services, please refer to the document [Introduction to Data Service Usage]().



