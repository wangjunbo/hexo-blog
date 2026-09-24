title: Elasticsearch exists 和 missing
author: peace
tags:
  - Elasticsearch
categories:
  - 编程
date: 2018-07-03 16:50:00
---
https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-exists-query.html
```
GET /search_data/template/_search
{
    "query": {
        "exists" : { "field" : "gid" }
    }
}
```
There isn’t a missing query. Instead use the exists query inside a must_not clause as follows:
```
GET /search_data/template/_search
{
    "query": {
        "bool": {
          "must_not": {
            "exists" : { "field" : "gid" }
          }
        }
    }
}
```