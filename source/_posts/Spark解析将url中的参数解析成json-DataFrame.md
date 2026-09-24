title: Spark解析将url中的参数解析成json DataFrame
author: peace
tags:
  - Spark
categories:
  - 编程
date: 2020-02-25 15:33:00
---
```
import org.apache.spark.sql.types.StructType
import org.apache.spark.sql.functions.to_json
import org.apache.spark.sql.functions._ 

val schema = new StructType().add("sns", org.apache.spark.sql.types.StringType, true).add("tit", org.apache.spark.sql.types.StringType, true).add("e_t", org.apache.spark.sql.types.StringType, true).add("product", org.apache.spark.sql.types.StringType, true)


var  dateFormat:java.text.SimpleDateFormat = new java.text.SimpleDateFormat("yyyyMMddHHmmss")
var cal:java.util.Calendar=java.util.Calendar.getInstance()
cal.add(java.util.Calendar.HOUR,-1)
val yesterday=dateFormat.format(cal.getTime())
val month=yesterday.substring(0,6)
val day=yesterday.substring(6,8)
val hour=yesterday.substring(0,10)
val path="/data/nginx/origin/q_gif/"+month+"/"+day+"/"+hour+"*"
val df=spark.read.textFile(path)
//(arr(0),uri)
val dfArr = df.flatMap{ line => 
     val arr = line.split("\\|")
     if(arr != null && arr.length >= 3 && arr(3).length >7 ){
        val argArr = arr(3).substring(7).split("&")
        val result = argArr.flatMap{ argLine =>
            val pair = argLine.split("=")
            if(pair.length == 2){
            val p = (pair(0),pair(1))
            Some(p)
            }else{
                None
            } 
        }
        Some(result.toMap)
     }else{T
         None
     }
}
 

val jsonStringDf = dfArr.withColumn("mapfield", to_json($"value")).select("mapfield")
val dfJSON = jsonStringDf.withColumn("jsonData",from_json(col("mapfield"),schema)).select("jsonData.*")
dfJSON.repartition(10).write.mode("overwrite").format("parquet").save("/tmp/data/testgzip")
```