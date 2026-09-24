title: es查询的时候控制相似度
tags:
  - Elasticsearch
categories: []
date: 2016-01-27 19:09:00
---
详细内容https://www.elastic.co/guide/en/elasticsearch/guide/current/match-multi-word.html#match-precision
```elasticsearch
GET /nm*/_search
{
  "query": {
    "match": {
      "title": {
         "query" : "李小冉晒麻雀海报：国家利益高于一",
         "minimum_should_match": "90%"
      }
    }
  }
}
```