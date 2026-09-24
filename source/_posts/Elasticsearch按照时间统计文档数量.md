title: Elasticsearch按照时间统计文档数量
author: peace
tags:
  - Elasticsearch
categories:
  - 编程
date: 2019-10-11 10:14:00
---
Elasticsearch按时间聚合
<!-- more -->

```
GET md_log/_search
{  
  "size": 0,  
  "aggs": {  
    "group_by_state": {  
      "date_histogram": {         
        "field": "time",    
        "interval": "hour",        
        "format": "yyyy-MM-dd HH",  
        "min_doc_count": 0,
        "order": {
           "_key": "desc"
        }
      }  
    }  
  },  
  "query": {  
    "bool": {
      "must": [
        {
          "range": {
            "time": {
            "gte": 1570636800000,  
            "lt": 1570723200000
            }
          }
        }
      ],
      "must_not": [
        {
          "term": {
            "execute_type": {
              "value": "0"
            }
          }
        }
      ]
    }
  }  
} 
```
结果数据
```
{
  "took" : 178,
  "timed_out" : false,
  "_shards" : {
    "total" : 5,
    "successful" : 5,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : 229,
    "max_score" : 0.0,
    "hits" : [ ]
  },
  "aggregations" : {
    "group_by_state" : {
      "buckets" : [
        {
          "key_as_string" : "2019-10-10 14",
          "key" : 1570716000000,
          "doc_count" : 13
        },
        {
          "key_as_string" : "2019-10-10 13",
          "key" : 1570712400000,
          "doc_count" : 3
        },
        {
          "key_as_string" : "2019-10-10 12",
          "key" : 1570708800000,
          "doc_count" : 14
        },
        {
          "key_as_string" : "2019-10-10 11",
          "key" : 1570705200000,
          "doc_count" : 31
        },
        {
          "key_as_string" : "2019-10-10 10",
          "key" : 1570701600000,
          "doc_count" : 23
        },
        {
          "key_as_string" : "2019-10-10 09",
          "key" : 1570698000000,
          "doc_count" : 65
        },
        {
          "key_as_string" : "2019-10-10 08",
          "key" : 1570694400000,
          "doc_count" : 11
        },
        {
          "key_as_string" : "2019-10-10 07",
          "key" : 1570690800000,
          "doc_count" : 0
        },
        {
          "key_as_string" : "2019-10-10 06",
          "key" : 1570687200000,
          "doc_count" : 0
        },
        {
          "key_as_string" : "2019-10-10 05",
          "key" : 1570683600000,
          "doc_count" : 0
        },
        {
          "key_as_string" : "2019-10-10 04",
          "key" : 1570680000000,
          "doc_count" : 0
        },
        {
          "key_as_string" : "2019-10-10 03",
          "key" : 1570676400000,
          "doc_count" : 0
        },
        {
          "key_as_string" : "2019-10-10 02",
          "key" : 1570672800000,
          "doc_count" : 0
        },
        {
          "key_as_string" : "2019-10-10 01",
          "key" : 1570669200000,
          "doc_count" : 0
        },
        {
          "key_as_string" : "2019-10-10 00",
          "key" : 1570665600000,
          "doc_count" : 0
        },
        {
          "key_as_string" : "2019-10-09 23",
          "key" : 1570662000000,
          "doc_count" : 0
        },
        {
          "key_as_string" : "2019-10-09 22",
          "key" : 1570658400000,
          "doc_count" : 0
        },
        {
          "key_as_string" : "2019-10-09 21",
          "key" : 1570654800000,
          "doc_count" : 5
        },
        {
          "key_as_string" : "2019-10-09 20",
          "key" : 1570651200000,
          "doc_count" : 14
        },
        {
          "key_as_string" : "2019-10-09 19",
          "key" : 1570647600000,
          "doc_count" : 9
        },
        {
          "key_as_string" : "2019-10-09 18",
          "key" : 1570644000000,
          "doc_count" : 0
        },
        {
          "key_as_string" : "2019-10-09 17",
          "key" : 1570640400000,
          "doc_count" : 19
        },
        {
          "key_as_string" : "2019-10-09 16",
          "key" : 1570636800000,
          "doc_count" : 22
        }
      ]
    }
  }
}
```

kibana可视化配置 https://s0www0elastic0co.icopy.site/guide/en/kibana/4.5/visualize.html


参考 http://doc.codingdict.com/elasticsearch/147/
https://blog.csdn.net/qq_28988969/article/details/81565765
https://www.iteye.com/blog/wsdtq123-2346070