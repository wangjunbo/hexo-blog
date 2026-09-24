title: Create separate columns from array column in Spark Dataframe in Scala
author: peace
tags:
  - Spark
categories:
  - 编程
date: 2019-01-04 13:34:00
---
```
scala> import org.apache.spark.sql.Column
scala> val df = Seq((Array(3,5,25), 3),(Array(2,7,15),4),(Array(1,10,12),2)).toDF("column1", "column2")
df: org.apache.spark.sql.DataFrame = [column1: array<int>, column2: int]

scala> def getColAtIndex(id:Int): Column = col(s"column1")(id).as(s"column1_${id+1}")
getColAtIndex: (id: Int)org.apache.spark.sql.Column

scala> val columns: IndexedSeq[Column] = (0 to 2).map(getColAtIndex) :+ col("column2") //Here, instead of 2, you can give the value of n
columns: IndexedSeq[org.apache.spark.sql.Column] = Vector(column1[0] AS `column1_1`, column1[1] AS `column1_2`, column1[2] AS `column1_3`, column2)

scala> df.select(columns: _*).show
+---------+---------+---------+-------+
|column1_1|column1_2|column1_3|column2|
+---------+---------+---------+-------+
|        3|        5|       25|      3|
|        2|        7|       15|      4|
|        1|       10|       12|      2|
+---------+---------+---------+-------+
```
from https://stackoverflow.com/questions/52276284/create-separate-columns-from-array-column-in-spark-dataframe-in-scala-when-array/52277257#52277257


