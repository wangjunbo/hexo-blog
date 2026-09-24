title: Spark知识
author: peace
tags:
  - 大数据
categories:
  - 编程
date: 2018-05-30 18:41:00
---
### 引入包
```
    val spark = builder.getOrCreate()
    import spark.implicits._
    import org.apache.spark.sql.functions._
```
lit function is for adding literal values as a column。

```
import org.apache.spark.sql.functions._
df.withColumn("D", lit(750))
```
https://stackoverflow.com/questions/38587609/spark-add-new-column-with-the-same-value-in-scala/38588307

### submit提交参数
添加额外的jar包  
--jars /data/apps/publib/mysql-connector-java-5.1.38.jar

### 提交失败时查看日志
```
 yarn logs -applicationId application_1520296293387_121203
```

### [在spark中读取hive表数据](https://acadgild.com/blog/how-to-access-hive-tables-to-spark-sql)
warehouse=/user/hive/warehouse     
metastore=thrift://hadoop001.iu.com:9083
```
    val argsMap = LangUtil.parseCommandLineArguments(args)
    val warehouse = argsMap.get("warehouse")
    val metastore = argsMap.get("metastore")

    val builder = SparkSession.builder()
      .config("spark.sql.warehouse.dir",warehouse)
      .config("hive.metastore.uris",metastore)
      .enableHiveSupport()
    val spark = builder.getOrCreate()
    import spark.implicits._

    spark.sql("select * from d_pv where create_date='2018-06-07' limit 20 ").show(20,false)

    spark.close()
```
### [提取字段中的部分值 regexp_extract](https://blog.csdn.net/lvdan1/article/details/78340231)
```
spark.sql(" select regexp_extract(url,'(http://store.hohode.com/scene-)(.*?)(\\\\.html)',2) product_id , d_i , cats.act act from (select explode(cats) cats , d_i from tracker )  ")
```

### [agg的使用](https://stackoverflow.com/questions/37032025/how-to-sum-the-values-of-one-column-of-a-dataframe-in-spark-scala/37032902)
```
import org.apache.spark.sql.functions._
df.filter(" act = '"+btnType+"'  ").agg((sum("price"))).first().get(0)
```
### 为列命名
```
      val lookup = Map("_1" -> "id","_2" -> "type","_3" -> "cookie_id")
      val rlog = spark.read.textFile(path)
      val rlog1 = rlog.map(_.split("#")).map(x=>(getSceneId(x(15)) ,x(21),x(3) ))
      val rlog2 = rlog1.select(rlog1.columns.map(c => col(c).as(lookup.getOrElse(c, c))): _*)
```
### 错误1
Unable to find encoder for type stored in a Dataset.  Primitive types (Int, String, etc) and Product types (case classes) are supported by importing spark.implicits._  
可能是少了隐式转换
```
import sparkSession.implicits._
```
https://stackoverflow.com/questions/38664972/why-is-unable-to-find-encoder-for-type-stored-in-a-dataset-when-creating-a-dat