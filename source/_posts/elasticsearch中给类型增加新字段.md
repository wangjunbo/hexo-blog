title: Elasticsearch中给类型增加新字段
tags:
  - Elasticsearch
categories: []
date: 2016-02-02 14:51:00
---
https://www.elastic.co/guide/en/elasticsearch/guide/current/_controlling_analysis.html

For instance, let’s add a new field to my_index:
```elasticsearch
PUT /my_index/_mapping/my_type
{
    "my_type": {
        "properties": {
            "english_title": {
                "type":     "string",
                "analyzer": "english"
            }
        }
    }
}
```