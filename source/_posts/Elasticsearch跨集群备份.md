title: Elasticsearch跨集群备份
author: peace
tags:
  - Elasticsearch
categories:
  - 编程
date: 2021-12-30 09:36:00
---
## 概述
目前7.0以上版本跨集群备份的方式有很多，例如**elasticsearch-dump，reindex，snapshot，logstash。**

## 方案对比
| **方案** | **elasticsearch-dump** | **reindex** | **snapshot** | **logstash** |
| --- | --- | --- | --- | --- |
| 基本原理 | 逻辑备份，类似mysqldump将数据一条一条导出后再执行导入 | reindex 是 Elasticsearch 提供的一个 API 接口，可以把数据从一个集群迁移到另外一个集群 | 从源集群通过Snapshot API 创建数据快照，然后在目标集群中进行恢复 | 从一个集群中读取数据然后写入到另一个集群 |
| 网络要求 | 集群间互导需要网络互通，先导出文件再通过文件导入集群则不需要网络互通 | 网络需要互通 | 无网络互通要求 | 网络需要互通 |
| 迁移速度 | 慢 | 快 | 快 | 一般 |
| 适合场景 | 适用于数据量小的场景 | 适用于数据量大，在线迁移数据的场景 | 适用于数据量大，接受离线数据迁移的场景 | 适用于数据量一般，近实时数据传输 |
| 配置复杂度 | 中等 | 简单 | 复杂 | 中等 |

<!-- more -->

## reindex介绍
这里介绍其中一种比较简单的方式。
使用官方提供的reindex API就可以实现，但前提是两个ES集群是网络互通的。
​

_reindex不会尝试设置目标索引。它不会复制源索引的设置信息。您应该在运行_reindex操作之前设置目标索引，包括设置映射，分片数，副本等。_
​

源ES集群的版本是7.8.0，目标ES的版本是7.13.0
​

### 在目标ES的kibana上执行命令
```python
POST _reindex?requests_per_second=500&wait_for_completion=false
{
  "source": {
    "remote": {
      "host": "http://10.0.20.13:9200",
      "username": "xiaoming",
      "password": "eqIR1CGhM8lQ"
    },
    "index": "scene_model",
    "_source": ["title","publish_time","total_pv","biz_type"],
    "size":500,
    "query": {
      "bool": {
        "must": [
          {
            "range": {
              "publish_time": {
                "gte": "2021-01-01"
              }
            }
          },
          {
            "range": {
              "total_pv": {
                "gt": 20
              }
            }
          },
          {
            "term": {
              "biz_type": {
                "value": "0"
              }
            }
          }
        ]
      }
    }

  },
  "dest": {
    "index": "scene_model_tmp"
  }
}
```
就可以将源机器上的scene_model的部分数据备份到目标机器上。
### 部分代码解释
**_reindex **
**​**表示此次使用Elasticsearch reindex API的方式进行索引数据的备份。
**​**

**requests_per_second=500 **
表示每秒请求查询多少文档
​

**wait_for_completion=false**
这个设置可以让ES在后台执行此操作，而不是在页面上等待任务的执行完成。如果不设置这个参数，对于要执行很长时间的任务，一会儿就会返回错误信息，比如502或者其它的一些报错信息。
​

**source 和 dest **
分别指定源ES集群和目标ES的一些配置信息。
​

**source 中的remote **
指定源服务器的ip，用户名和密码(如果有的话)。
​

**source中的index **
指定从源ES集群的哪个索引备份数据。
​

**source中的_source字段 **
备份的时候需要备份哪些字段。默认会备份所有的字段，但是有时候只需要关注几个字段，其它的字段不需要，就通过_source字段指定那些需要的字段就行了。
​

**source中的 size 字段 **
默认情况下reindex一个批次会请求1000条数据，你可以使用size这个参数修改批次的大小。[参考链接](https://www.elastic.co/guide/en/elasticsearch/reference/7.17/docs-reindex.html)
​

**source中的query 子句**​
可以设定查询条件，只有符合查询条件的数据才会备份。
dest中的index指定目标索引。


## 查询和终止任务
在请求elasticsearch api 发出后有时候运行请求后想停止，需要用到两个api
```python
GET  _tasks?detailed=true&actions=*reindex //查询正在运行的任务 
```
actions 参数就是你要查询的动作，*代表所有动作
```
POST _tasks/RnT2C85FQIqfi4zRGyfJMw:50571743/_cancel  //取消任务 
```
RnT2C85FQIqfi4zRGyfJMw 代表：第一条api返回的node字段
50571743代表：第一条api返回的id字段
​

参考文档
 1 [https://blog.csdn.net/cr7258/article/details/114957725](https://blog.csdn.net/cr7258/article/details/114957725)
2 [https://www.elastic.co/guide/en/elasticsearch/reference/7.17/docs-reindex.html](https://www.elastic.co/guide/en/elasticsearch/reference/7.17/docs-reindex.html)