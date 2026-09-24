title: Elasticsearch分词疑惑
author: peace
tags:
  - Elasticsearch
categories:
  - 编程
date: 2018-05-04 16:51:00
---
今天使用must_not进行过滤的时候，发现“五一”关键词过滤不掉，例如标题“五一劳动节放假通知”，后来经过反复的探索，发现是分词器的问题。
```
GET /_analyze
{
  "analyzer": "ik_smart",
  "text": "五一劳动节放假通知"
}
```
分词结果如下：
```
{
  "tokens": [
    {
      "token": "五一劳动节",
      "start_offset": 0,
      "end_offset": 5,
      "type": "CN_WORD",
      "position": 0
    },
    {
      "token": "放假",
      "start_offset": 5,
      "end_offset": 7,
      "type": "CN_WORD",
      "position": 1
    },
    {
      "token": "通知",
      "start_offset": 7,
      "end_offset": 9,
      "type": "CN_WORD",
      "position": 2
    }
  ]
}
```
会把“五一劳动节”当成一个token，而不是“五一”和“劳动节”，所以“五一”关键词就匹配不都了。

### 使用某个索引
```
GET /bigdata/_analyze
{
  "analyzer": "ik_smart",
  "text": "五一劳动节放假通知"
}
```