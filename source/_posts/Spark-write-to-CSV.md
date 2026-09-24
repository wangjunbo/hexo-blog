title: Spark Read and Write to CSV
author: peace
tags:
  - 大数据
categories:
  - 编程
date: 2018-04-20 17:10:00
---
### Read
```
val logs = spark.read.csv("/tmp/sousuo")
or
val logs = spark.read.option("delimiter", "\t").csv("/tmp/sousuo")
or
val logs = spark.read.option("delimiter", ",").csv("/tmp/sousuo")
```

### Write
```
df
   .repartition(1)
   .write.format("com.databricks.spark.csv")
   .option("header", "true")
   .save("mydata.csv")
```
or coalesce:
```
df
   .coalesce(1)
   .write.format("com.databricks.spark.csv")
   .option("header", "true")
   .save("mydata.csv")
```
参考 https://stackoverflow.com/questions/31674530/write-single-csv-file-using-spark-csv