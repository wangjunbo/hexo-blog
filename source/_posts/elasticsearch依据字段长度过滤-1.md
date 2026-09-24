title: Elasticsearch依据字段长度过滤
tags:
  - Elasticsearch
categories: []
date: 2016-01-28 17:09:00
---
查询title字段的长度小于9的文档
```
GET /nm*/_search
{
  "query": {
    "filtered": {
      "query": {
          "match": {
              "title": {
                  "query": "黄晓明和杨颖结婚",
                  "operator": "and",
                  "minimum_should_match": "90%"
              }
          }        
      },
      "filter": {
        "script" : {
            "script" : "doc['title'].size() < 9"
        }
      }
    }
  }
}
```