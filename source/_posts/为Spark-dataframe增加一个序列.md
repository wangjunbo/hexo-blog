title: 为Spark dataframe增加一个序列
author: peace
tags:
  - 大数据
categories:
  - 编程
date: 2018-05-21 16:21:00
---
[add a column to your dataframe with an auto-increment integer value](https://stackoverflow.com/questions/46149567/add-sequence-number-column-in-dataframe-usnig-scala)

```
val oracleTableDF2 = oracleTableDF.withColumn("SeqNum", monotonically_increasing_id())
```