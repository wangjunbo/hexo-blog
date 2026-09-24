title: Elasticsearch入门
author: peace
tags:
  - Elasticsearch
categories:
  - 编码
date: 2020-05-09 10:57:00
---
Elasticsearch 天生就是分布式的。 Elasticsearch 在分布式方面几乎是透明的。  

可以水平扩容 ，或横向扩容。

当一个节点被选举成为 主 节点时， 它将负责管理集群范围内的所有变更，例如增加、删除索引，或者增加、删除节点等。 而主节点并不需要涉及到文档级别的变更和搜索等操作，所以当集群只拥有一个主节点的情况下，即使流量的增加它也不会成为瓶颈。

作为用户，我们可以将请求发送到 集群中的任何节点 ，包括主节点。 每个节点都知道任意文档所处的位置，并且能够将我们的请求直接转发到存储我们所需文档的节点。 无论我们将请求发送到哪个节点，它都能负责从各个包含我们所需文档的节点收集回数据，并将最终结果返回給客户端。

### 查看节点健康状况

`status` 字段指示着当前集群在总体上是否工作正常。它的三种颜色含义如下：green所有的主分片和副本分片都正常运行。`yellow`所有的主分片都正常运行，但不是所有的副本分片都正常运行。`red`有主分片没能正常运行。
```
GET /_cluster/health
```

## 索引,分区,节点
<!-- more -->

在往 Elasticsearch 添加数据时需要用到 索引 —— 保存相关数据的地方。 索引实际上是指向一个或者多个物理`分片`的逻辑命名空间 。

一个分片是一个 Lucene 的实例，以及它本身就是一个完整的搜索引擎。

下图展示了Elasticsearch实例，节点，分片和文档之间的关系

[elasticsearch](https://gblobscdn.gitbook.com/assets%2F-M4ne6aWRxw-kzBN2TrR%2F-M5S0xvTA1ExcJxdBlZo%2F-M5S1QGmFnHfT1gtyeIk%2Fimage.png?alt=media&token=fad07f5b-f589-4536-ba8e-48fffc5358de)

技术上来说，一个主分片最大能够存储 Integer.MAX_VALUE - 128 个文档，但是实际最大值还需要参考你的使用场景：包括你使用的硬件， 文档的大小和复杂程度，索引和查询文档的方式以及你期望的响应时长。

一个副本分片只是一个主分片的拷贝。副本分片作为硬件故障时保护数据不丢失的冗余备份，并为搜索和返回文档等读操作提供服务。读操作——搜索和返回数据——可以同时被主分片 或 副本分片所处理，所以当你拥有越多的副本分片时，也将拥有越高的吞吐量。

在索引建立的时候就已经确定了主分片数，但是副本分片数可以随时修改。
索引的名字必须小写，不能以下划线开头，不能包含逗号。

### 创建索引模板
```
PUT _template/spider
{
  "index_patterns": [
    "spider_*"
  ],
  "settings": {
    "index": {
      "number_of_shards": "3",
      "refresh_interval": "60s",
      "number_of_replicas": 0,
      "codec": "best_compression"
    }
  },
  "mappings": {
    "properties": {
      "id": {
        "type": "keyword"
      },
      "domain": {
        "type": "keyword"
      },
      "res_type": {
        "type": "integer"
      },
      "name": {
        "type": "text",
        "analyzer": "ik_max_word",
        "fields": {
          "keyword": {
            "type": "keyword",
            "ignore_above": 256
          }
        }
      },
      "url": {
        "type": "text",
        "index": "false"
      },
      "size": {
        "type": "integer",
        "index": "false"
      },
      "height": {
        "type": "integer",
        "index": "false"
      },
      "width": {
        "type": "integer",
        "index": "false"
      },
      "tags": {
        "type": "text",
        "analyzer": "simple",
        "fielddata": true
      },
      "content": {
        "type": "text",
        "analyzer": "ik_max_word"
      },
      "description": {
        "type": "text",
        "analyzer": "ik_max_word"
      },
      "extension": {
        "type": "keyword"
      },
      "svg": {
        "type": "text",
        "index": "false"
      },
      "path": {
        "type": "keyword",
        "index": "false"
      },
      "owner_type": {
        "type": "keyword"
      },
      "website_id": {
        "type": "keyword"
      },
      "create_time": {
        "format": "yyyy-MM-dd HH:mm:ss||yyyy-MM-dd HH:mm||yyyy年MM月dd日 HH:mm||yyyy年MM月dd日 HH:mm:ss||yyyy年MM月dd日||yyyy-MM-dd||epoch_millis",
        "type": "date"
      },
      "update_time": {
        "type": "date",
        "format": "yyyy-MM-dd HH:mm:ss||yyyy-MM-dd HH:mm||yyyy年MM月dd日 HH:mm||yyyy年MM月dd日 HH:mm:ss||yyyy年MM月dd日||yyyy-MM-dd||epoch_millis"
      },
      "title": {
        "type": "text",
        "analyzer": "ik_max_word",
        "fields": {
          "keyword": {
            "type": "keyword",
            "ignore_above": 256
          }
        }
      },
      "summary": {
        "type": "text",
        "analyzer": "ik_max_word"
      },
    }
  }
}
```
### 创建索引
如下创建了一个blogs的索引，副本为1，也就是有2份数据，1份主分片的，1份副本的。
```
PUT /spider_wenan
{
   "settings" : {
      "number_of_shards" : 3,
      "number_of_replicas" : 1
   }
}
```

### 将文档从一个index复制到另一个index
```
POST _reindex
{
  "source": {
    "index": "spider_wenan"
  },
  "dest": {
    "index": "spider_wenan_tmp1"
  }
}

```
### 动态调整副本分片数目
```
PUT /blogs/_settings
{
   "number_of_replicas" : 2
}
```
