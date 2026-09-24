title: WaterDrop将Kafka中的数据写入Clickhouse
author: peace
tags:
  - Clickhouse
categories:
  - 编程
date: 2019-10-11 19:09:00
---
下载并解压WaterDrop
```
wget https://github.com/InterestingLab/waterdrop/releases/download/v1.4.1/waterdrop-1.4.1.zip
```

修改配置文件waterdrop-env.sh
```
cd /data/work/waterdrop-1.4.1
vim config/waterdrop-env.sh
SPARK_HOME=/data/work/spark-2.4  #配置为spark的路径
```
<!-- more -->

增加配置文件small.conf
```
spark {
  spark.streaming.batchDuration = 5
  spark.app.name = "small_spark_streaming"
  spark.ui.port = 14020
  spark.executor.instances = 3
  spark.executor.cores = 1
  spark.executor.memory = "1g"
}

input {
  kafkaStream  {
    topics = "small"
    consumer.bootstrap.servers = "hadoop008.eqxiu.com:9092,hadoop006.eqxiu.com:9092,hadoop007.eqxiu.com:9092"
    consumer.zookeeper.connect = "hadoop004:2181,hadoop003:2181,hadoop002:2181,hadoop001:2181,hadoop005:2181"
    consumer.group.id = "clickhouse_small"
    consumer.failOnDataLoss = false
    consumer.auto.offset.reset = latest
    consumer.rebalance.max.retries = 100
  }
}
filter {
      json{
            source_field = "raw_message"
      }
}

output {
    clickhouse {
        host = "10.10.8.1:8123"
        database = "bw"
        table = "small"
        fields = ["act","b_t","b_v","bro","c_i","c_p","s_t","c_t","cit","cou","url","ref","u_i"]
        username = ""
        password = ""
        retry_codes = [209, 210 ,1002]
        retry = 10
	    bulk_size = 1000
    }
}
```
创建Clickhouse表
```
create table bw.small( act String, b_t String, b_v String, bro String, c_i String, c_p String, s_t String, c_t String, cit String, cou String, url String, ref String, u_i String ) ENGINE = MergeTree() partition by toYYYYMMDD(toDateTime(toUInt64(s_t)/1000)) order by (s_t);
```

启动写入程序
```
cd /data/work/waterdrop-1.4.1
sh bin/start-waterdrop.sh --master yarn --deploy-mode client --config small.conf 
```