title: Elasticsearch配置ik分词器
tags:
  - Elasticsearch
categories: []
date: 2016-01-27 17:19:00
---
假定你已经安装了elasticsearch2.1.0和maven, 下面的步骤针对elasticsearch2.1.0有效, 其它版本可能不使用

 
### 下载ik
因为我的elasticsearch是2.1.0,所以去这里下载https://github.com/medcl/elasticsearch-analysis-ik/releases/tag/v1.6.1 

其它版本的elasticsearch对应的ik去https://github.com/medcl/elasticsearch-analysis-ik 下载

### 解压ik，并进入ik
### 执行 mvn package 编译ik
### 复制文件
将elasticsearch-analysis-ik-master/target/releases/elasticsearch-analysis-ik-1.6.1.zip 拷贝到es的plugin/ik目录下并解压
  
同时将elasticsearch-analysis-ik-master/config/ik文件夹拷贝到es的config下

### 然后重启es,就可以了.