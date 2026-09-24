title: spark-shell执行spark 文件
author: peace
tags:
  - 大数据
categories:
  - 编程
date: 2018-06-11 16:31:00
---
Spark file example, a.scala
```
import org.apache.hadoop.fs.FileSystem
import org.apache.hadoop.fs.Path
import org.apache.spark.sql.{Dataset, SaveMode, SparkSession}

val argArray = spark.sparkContext.getConf.get("spark.driver.args").split("\\s+")
print(argArray)
val logs = spark.read.json(argArray(0)).select("cats")
logs.cache()
logs.createOrReplaceTempView("tracker")

val sql1 = "select count(1) from  tracker  where cats.cat='store' and cats.act='aa'"
spark.sql(sql1).show(false)

val sql2 = "select count(1) from (select explode(cats) cats from tracker ) where cats.cat='store' and cats.act='bb'"
spark.sql(sql1).show(false)
spark.close()
```

run script example test.sh
```
#!/bin/bash

TaskName="mianfei"
cd `dirname $0`
/data/work/spark2.0/bin/spark-shell \
-i mianfei.scala \
--name ${TaskName} \
--master yarn \
--deploy-mode client \
--executor-memory 1G \
--num-executors 15 \
--executor-cores 2 \
--conf spark.driver.args="/data/logs/20180609/* helloworld"
exit 0
```