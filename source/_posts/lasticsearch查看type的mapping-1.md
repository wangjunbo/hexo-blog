title: Elasticsearch查看type的mapping
tags:
  - Elasticsearch
categories: []
date: 2016-02-19 10:59:00
---
使用以下方式查看elasticsearch中type的mapping
```elasticsearch
GET /my_index/_mapping/my_type
```
详情请参考https://www.elastic.co/guide/en/elasticsearch/guide/current/mapping-intro.html#_viewing_the_mapping