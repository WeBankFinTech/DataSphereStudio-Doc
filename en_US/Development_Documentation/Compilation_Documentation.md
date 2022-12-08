## 1.Compile the overall DSS：

   After getting the project code from git, use maven to package the project installation package. 

   (1) You can modify the versions of Linkis, Java, Scala, Maven and other software in the top-level pom.xml file to suit your company's big data environment, as follows:

```xml
  <properties>
          <dss.version>1.1.1</dss.version>
          <linkis.version>1.1.1</linkis.version>
          <scala.version>2.11.12</scala.version>
          <jdk.compile.version>1.8</jdk.compile.version>
          <maven.version>3.3.3</maven.version>
  </properties>

```

   (2) **If you are using it locally for the first time, you must execute the following command in the directory where the outermost project pom.xml is located**：

```bash
    mvn -N  install
```

   (3) Execute the following command in the directory where the outermost project pom.xml is located
    
```bash
    mvn clean install
```

  

  (4) Get the installation package, in the assembly->target directory of the project:

```
    wedatasphere-dss-x.x.x-dist.tar.gz
```





