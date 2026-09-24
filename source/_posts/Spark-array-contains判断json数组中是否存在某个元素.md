title: Spark array_contains判断json数组中是否存在某个元素
author: peace
tags:
  - 大数据
categories:
  - 编程
date: 2018-07-25 15:09:00
---
在非spark-shell环境中需要使用如下方式导入包
```
import org.apache.spark.sql.functions._
```
对于结构如下的数据
```
scala> res5.printSchema
root
 |-- cats: array (nullable = true)
 |    |-- element: struct (containsNull = true)
 |    |    |-- act: string (nullable = true)
 |    |    |-- cat: string (nullable = true)
 |-- u_i: string (nullable = true)
```
先使用下边的方式将json嵌套数据转换成单个字段的数组
<!-- more -->
```
val reducedLogs = spark.sql("select cats.act, cats.cat, u_i ,d_i from table1")
```
具体使用方式如下
```
scala> import org.apache.spark.SparkContext
import org.apache.spark.SparkContext

scala> import org.apache.spark.sql.DataFrame
import org.apache.spark.sql.DataFrame

scala> def testData (sc: SparkContext): DataFrame = {
     |     val stringRDD = sc.parallelize(Seq
     |      ("""{ "name": "ned", "tags": ["blue", "big", "private"] }""",
     |       """{ "name": "albert", "tags": ["private", "lumpy"] }""",
     |       """{ "name": "zed", "tags": ["big", "private", "square"] }""",
     |       """{ "name": "jed", "tags": ["green", "small", "round"] }""",
     |       """{ "name": "ed", "tags": ["red", "private"] }""",
     |       """{ "name": "fred", "tags": ["public", "blue"] }"""))
     |     val sqlContext = new org.apache.spark.sql.SQLContext(sc)
     |     import sqlContext.implicits._
     |     sqlContext.read.json(stringRDD)
     |   }
testData: (sc: org.apache.spark.SparkContext)org.apache.spark.sql.DataFrame

scala>   
     | val df = testData (sc)
df: org.apache.spark.sql.DataFrame = [name: string, tags: array<string>]

scala> val report = df.select ("*").where (array_contains (df("tags"), "private"))
report: org.apache.spark.sql.DataFrame = [name: string, tags: array<string>]

scala> report.show
+------+--------------------+
|  name|                tags|
+------+--------------------+
|   ned|[blue, big, private]|
|albert|    [private, lumpy]|
|   zed|[big, private, sq...|
|    ed|      [red, private]|
+------+--------------------+
```
参考https://stackoverflow.com/questions/34833653/filter-spark-dataframe-with-row-field-that-is-an-array-of-strings