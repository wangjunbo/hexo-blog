title: get TopN of all groups after group by using Spark DataFrame
author: peace
tags:
  - 大数据
categories:
  - 编程
date: 2018-04-13 17:37:00
---
https://stackoverflow.com/questions/33655467/get-topn-of-all-groups-after-group-by-using-spark-dataframe

You can use rank window function as follows
```
import org.apache.spark.sql.expressions.Window
import org.apache.spark.sql.functions.{rank, desc}

val n: Int = ???

// Window definition
val w = Window.partitionBy($"user").orderBy(desc("rating"))

// Filter
df.withColumn("rank", rank.over(w)).where($"rank" <= n)
```
If you don't care about ties then you can replace rank with rowNumber