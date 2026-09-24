title: Parquet Schema 合并
author: peace
tags:
  - 大数据
  - ''
categories:
  - 编程
date: 2018-04-11 12:03:00
---
类似 ProtocolBuffer，Avro，以及 Thrift，Parquet 也支持 schema 演变。用户可以从一个简单的 schema 开始，并且根据需要逐渐地向 schema 中添加更多的列。这样，用户最终可能会有多个不同但是具有相互兼容 schema 的 Parquet 文件。Parquet 数据源现在可以自动地发现这种情况，并且将所有这些文件的 schema 进行合并。   

由于 schema 合并是一个性格开销比较高的操作，并且在大部分场景下不是必须的，从 Spark 1.5.0 开始默认关闭了这项功能。你可以通过以下方式开启 :    

设置数据源选项 mergeSchema 为 true 当读取 Parquet 文件时（如下面展示的例子），或者
这是全局 SQL 选项 spark.sql.parquet.mergeSchema 为 true。
import spark.implicits._   
```
// Create a simple DataFrame, store into a partition directory
val squaresDF = spark.sparkContext.makeRDD(1 to 5).map(i => (i, i * i)).toDF("value", "square")
squaresDF.write.parquet("data/test_table/key=1")
  
// Create another DataFrame in a new partition directory,
// adding a new column and dropping an existing column
val cubesDF = spark.sparkContext.makeRDD(6 to 10).map(i => (i, i * i * i)).toDF("value", "cube")
cubesDF.write.parquet("data/test_table/key=2")
  
// Read the partitioned table
val mergedDF = spark.read.option("mergeSchema", "true").parquet("data/test_table")
mergedDF.printSchema()
  
// The final schema consists of all 3 columns in the Parquet files together
// with the partitioning column appeared in the partition directory paths
// root
//  |-- value: int (nullable = true)
//  |-- square: int (nullable = true)
//  |-- cube: int (nullable = true)
//  |-- key: int (nullable = true)
```