title: Elasticsearch知识
author: peace
tags:
  - Elasticsearch
categories:
  - 编程
date: 2018-05-28 12:00:00
---
### [elasticSearch(5.3.0)的评分机制的研究](http://www.cnblogs.com/wangjiuyong/p/7055724.html)
### [lasticSearch 6.X 不再支持多个doc_type](https://lunatictwo.github.io/2017/11/18/ElasticSearch%206.X%20%E4%B8%8D%E5%86%8D%E6%94%AF%E6%8C%81%E5%A4%9A%E4%B8%AAdoc_type/)

### 
``` Elasticsearch mapping设置 [日期设置](https://zhuanlan.zhihu.com/p/34240906)
PUT user/log/_mapping
{
    "properties": {
        "title": {
            "type": "text",
            "analyzer": "ik_max_word",
            "search_analyzer": "ik_max_word"
        },
        "content": {
            "type": "text",
            "analyzer": "ik_max_word",
            "search_analyzer": "ik_max_word"
        },
        "datetime": {
            "type": "date",
            "format": "yyyy-MM-dd HH:mm:ss||yyyy-MM-dd||epoch_millis"
        }
    }
}

```

[elasticsearch的keyword与text的区别（5.4）](https://my.oschina.net/jsonyang/blog/1204659)
keyword：存储数据时候，不会分词建立索引

text：存储数据时候，会自动分词，并生成索引（这是很智能的，但在有些字段里面是没用的，所以对于有些字段使用text则浪费了空间）。


