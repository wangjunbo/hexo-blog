title: 数据类型keyword和text的区别
author: peace
tags:
  - Elasticsearch
categories:
  - 编程
date: 2018-02-25 15:27:00
---
From: <http://blog.csdn.net/lionel_fengj/article/details/78367570>

在 ES2.x 版本字符串数据是没有 keyword 和 text 类型的，只有string类型，ES更新到5版本后，取消了 string 数据类型，代替它的是 keyword 和 text 数据类型，那么 keyword 和 text 有什么区别了？  
Text 数据类型被用来索引长文本，比如说电子邮件的主体部分或者一款产品的介绍。这些文本会被分析，在建立索引前会将这些文本进行分词，转化为词的组合，建立索引。允许 ES来检索这些词语。text 数据类型不能用来排序和聚合。 
    
    curl -XPUT 'localhost:9200/employees/' -d '
    {
        "mappings":{
            "employee":{
                 "properties": {
                     "intro":"text"
                 }
            }
        }
    }
    '

Keyword 数据类型用来建立电子邮箱地址、姓名、邮政编码和标签等数据，不需要进行分词。可以被用来检索过滤、排序和聚合。keyword 类型字段只能用本身来进行检索。 
    
    curl -XPUT 'localhost:9200/employees/' -d '
    {
        "mappings":{
            "employee":{
                 "properties": {
                     "name":"keyword"
                 }
            }
        }
    }
    '

注意，如果不像以上通过mapping 配置索引时，遇到字符串类型时候的字端，系统会默认为“text”类型。检索的时候对字符串进行分析。所以要想只通过字段本身来进行检索，还是需要按照上面把该字段改为“keyword”类型。 
