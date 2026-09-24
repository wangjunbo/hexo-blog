title: 使用特定analyzer分析文本
categories:
  - 编程
tags:
  - Elasticsearch
date: 2018-02-06 15:21:00
---
## 使用特定analyzer分析文本 
<pre>
curl -XGET 'localhost:9200/_analyze' -d '
{
  "analyzer" : "standard",
  "text" : "this is a test"
}
</pre>

## 使用某个字段的分析器分析文本,这里分别使用tag和tweet
<http://es.xiaoleilu.com/052_Mapping_Analysis/45_Mapping.html>
<pre>
GET /gb/_analyze?field=tag
{
    "text":"Black-cats"
}

GET /gb/_analyze?field=tag
{
    "text":"Black-cats"
}
</pre>