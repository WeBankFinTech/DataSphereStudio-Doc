# DSS User Test Example 1：Scala

The purpose of DSS user test samples is to provide a set of test samples for new users of the platform to familiarize themselves with the common operations of DSS and to verify the correctness of the DSS platform

![home](../../Images/Using_Document/Scriptis/home.png)

## 1.1 Spark Core（entry function sc）

In Scriptis, SparkContext is already registered for you by default, so just use sc directly:

### 1.1.1 Single Value operator (Map operator as an example)

```scala
val rddMap = sc.makeRDD(Array((1,"a"),(1,"d"),(2,"b"),(3,"c")),4)
val res = rddMap.mapValues(data=>{data+"||||"})
res.collect().foreach(data=>println(data._1+","+data._2))
```

### 1.1.2 Double Value operator (union operator as an example)

```scala
val rdd1 = sc.makeRDD(1 to 5)
val rdd2 = sc.makeRDD(6 to 10)
val rddCustom = rdd1.union(rdd2)
rddCustom.collect().foreach(println)
```

### 1.1.3 K-V operator (reduceByKey operator is an example)

```scala
val rdd1 = sc.makeRDD(List(("female",1),("male",2),("female",3),("male",4)))
val rdd2 = rdd1.reduceByKey((x,y)=>x+y)
rdd2.collect().foreach(println)
```

### 1.1.4 Execute the operator (the above collect operator is an example)

### 1.1.5 Read files from hdfs and do simple execution

```scala
case class Person(name:String,age:String)
val file = sc.textFile("/test.txt")
val person = file.map(line=>{
               val values=line.split(",")
         
     Person(values(0),values(1))
})
val df = person.toDF()
df.select($"name").show()
```



## 1.2 UDF function test

### 1.2.1 function definition



```scala
def ScalaUDF3(str:  String):  String  =  "hello, "  + str  + "this is a third attempt"
```

### 1.2.2 register function

function-》personal function-》Right click to add spark function=》The registration method is the same as the regular spark development

![img](../../Images/Using_Document/Scriptis/udf1.png)               

## 1.3 UDAF function test

### 1.3.1 Jar package upload

​        Develop a udaf function for averaging on idea, package it into a jar (wordcount) package, and upload the dss jar folder.

![img](../../Images/Using_Document/Scriptis/udf2.png)            

### 1.3.2 register function

function-》personal function-》Right click to add spark function=》The registration method is the same as the regular spark development 

  ![img](../../Images/Using_Document/Scriptis/udf-3.png)                        