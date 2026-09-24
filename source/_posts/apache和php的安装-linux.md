title: Elasticsearch中初步使用filter
tags:
  - Elasticsearch
categories: []
date: 2016-01-18 16:24:00
---
```elasticsearch
GET /nm*/_search
{
   "query": {
     "filtered": {
       "query": {
          "match_all": {} ①
       },
       "filter": {
           "term": { "pub_time": 1449791040000           }
       }
     }
   }
}
```
一定要保证①处能够查询出数据, 然后后边的 filter 才会有意义, 否则根本查询不到数据.