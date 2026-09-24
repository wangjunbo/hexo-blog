title: Spark将数据写入mysql
author: peace
tags:
  - 大数据
categories:
  - 编程
date: 2018-08-13 16:23:00
---
使用shell，添加mysql jar
```
/data/work/spark-2.2/bin/spark-shell --master yarn --executor-memory 5G --num-executors 5 --jars  /jars/mysql-connector-java-5.1.35.jar
```

Spark向mysql写入数据的具体代码
```
val df = spark.read.json("/data/log/log2018070319_0001.snappy")
val prop = new java.util.Properties
prop.setProperty("driver", "com.mysql.jdbc.Driver")
prop.setProperty("user", "root")
prop.setProperty("password", "123456") 
//jdbc mysql url - destination database is named "data"
val url = "jdbc:mysql://hohode.com:13306/sp"
//destination database table 
val table = "sample_test"
//write data from spark dataframe to database
df.limit(10).write.mode("append").jdbc(url, table, prop)
```

[reference](http://bigdatums.net/2016/10/16/writing-to-a-database-from-spark/)