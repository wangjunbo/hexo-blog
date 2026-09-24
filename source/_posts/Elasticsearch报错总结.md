title: Elasticsearch报错总结
author: peace
tags:
  - Elasticsearch
categories:
  - 编程
date: 2018-02-25 14:21:00
---
1. No search type for [count]  
可能是因为版本没有选对。比如使用5.X的Elasticsearch,在Grafana上的DataSource总选择的却是2.X

2. 使用filter搜索不到数据  
使用
```
GET /tracker/_search
{
    "query": {
      "bool": {
        "filter": {
          "term": {
            "bro": "Chrome"
          }
        }
      }
    }
}
```
语句搜索数据时，无法搜到。当使用
```
GET /tracker/_search
{
    "query": {
      "bool": {
        "filter": {
          "term": {
            "bro": "chrome"
          }
        }
      }
    }
}
```
搜索数据时，则可以。这其中最关键的是使用filter时，过滤term需要使用英文小写。

[在Elasticsearch中对 text 类型的字段进行聚合异常Fielddata is disabled，Set fielddata=true](https://my.oschina.net/u/193184/blog/1505175)