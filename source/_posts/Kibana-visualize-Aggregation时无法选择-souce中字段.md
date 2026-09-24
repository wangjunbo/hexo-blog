title: Kibana visualize Aggregation时无法选择_souce中字段
author: peace
tags:
  - Elasticsearch
categories:
  - 编程
date: 2018-02-08 17:05:00
---
Elasticsearch版本为5.6.1   
可以使用如下mapping方式
```
PUT /index2
{
  "mappings": {
    "log" : {
      "properties" : {
          "timestamp":{  
                   "format":"strict_date_optional_time||epoch_millis",  
                   "type":"date"  
         },
          "hostname": {
            "type": "keyword",
            "ignore_above": 1024
          },
        "pct" : {
          "type":"float"
        }
      }
    }
  }
}
```
注意上述代码中hostname是如何定义的。   
将Kibana的index Patterns中相关index删除后重新创建index。
然后再在Kibana visualize Aggregation时就可以选择_souce中字段了，比如上边的hostname。

