title: Elasticsearch(5.x) template的使用
author: peace
tags:
  - Elasticsearch
categories:
  - 编程
date: 2018-02-25 17:10:00
---
 
[官方模板定义](https://www.elastic.co/guide/en/elasticsearch/reference/6.0/indices-templates.html)  
[Dynamic templates](https://www.elastic.co/guide/en/elasticsearch/reference/current/dynamic-templates.html#dynamic-templates)

template字段用来匹配索引名字  
keyword是String的一种子类型，说明字段是not not_analyzed  
使用的时候将//开头的两行注释去掉  
```
PUT /_template/template1
{
  "order": 0,
  "template": "index*",
  "mappings": {
    "_default_": {
      "dynamic_templates": [

        //针对特别字段进行定义
        {
          "long_to_date": {
            "mapping": {
              "doc_values": true,
              "type": "date"
            },
            "match": "dt",
            "match_mapping_type": "long"
          }
        },

        //以下是通用字段定义
        {
          "long2integer": {
            "mapping": {
              "doc_values": true,
              "type": "integer"
            },
            "match": "*",
            "unmatch":"dt",
            "match_mapping_type": "long"
          }
        },
        {
          "string2keyword": {
            "mapping": {
              "index": "not_analyzed",
              "omit_norms": true,
              "doc_values": true,
              "type": "keyword"
            },
            "match": "*",
            "match_mapping_type": "string"
          }
        },

        {
          "date": {
            "mapping": {
              "doc_values": true,
              "type": "date",
               "format":"strict_date_optional_time||epoch_millis"
            },
            "match": "*",
            "match_mapping_type": "date"
          }
        }

      ],
      "_all": {
        "enabled": false
      }
    }
  }
}
```