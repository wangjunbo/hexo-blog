title: 删除Elasticsearch文档中的字段
author: peace
tags:
  - Elasticsearch
categories:
  - 编程
date: 2019-01-23 16:44:00
---
from https://cinhtau.net/2017/09/01/remove-field-from-elasticsearch-document/

```
POST /test_index/log/_update_by_query
{
  "script": {
    "inline": "ctx._source.remove('source_type')",
    "lang": "painless"
  },
  "query": {
    "bool": {
      "must": [
        {
          "exists": {
            "field": "source_type"
          }
        }
      ]
    }
  }
}
```