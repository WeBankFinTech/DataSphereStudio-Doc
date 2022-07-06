# DSS User Test Example 3：SparkSQL

The purpose of DSS user test samples is to provide a set of test samples for new users of the platform to familiarize themselves with the common operations of DSS and to verify the correctness of the DSS platform

![home](../../Images/Using_Document/Scriptis/home.png)

## 3.1 RDD and DataFrame conversion

### 3.1.1 RDD to DataFrame

```scala
case class MyList(id:Int)
       
val lis = List(1,2,3,4)
       
val listRdd = sc.makeRDD(lis)
import spark.implicits._
val df = listRdd.map(value => MyList(value)).toDF()
       
df.show()
```

### 3.1.2 DataFrame to RDD

```scala
case class MyList(id:Int)
       
val lis = List(1,2,3,4)
val listRdd = sc.makeRDD(lis)
import spark.implicits._
val df = listRdd.map(value => MyList(value)).toDF()
println("------------------")
       
val dfToRdd = df.rdd
       
dfToRdd.collect().foreach(print(_))
```

## 3.2 DSL syntax style implementation

```scala
val df = df1.union(df2)
val dfSelect = df.select($"department")
dfSelect.show()
```

## 3.3 SQL syntax style implementation (entry function sqlContext)

```scala
val df = df1.union(df2)
       
df.createOrReplaceTempView("dfTable")
val innerSql = """
                         SELECT department
                         FROM dfTable
                        """
val sqlDF = sqlContext.sql(innerSql)
sqlDF.show()
```

​                        