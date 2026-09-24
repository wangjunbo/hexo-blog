title: 安装并运行Elasticsearch
categories:
  - 编程
tags:
  - Elasticsearch
date: 2018-02-06 15:21:00
---
<https://www.elastic.co/guide/en/elasticsearch/guide/current/running-elasticsearch.html>
安装elasticsearch比较简单,按照上边链接中的描述就行了,关键是如何安装sense.


## To install and run Sense:
Run the following command in the Kibana directory to download and install the Sense app:
./bin/kibana plugin --install elastic/sense 

Windows: bin\kibana.bat plugin --install elastic/sense.
Note
You can download Sense from https://download.elastic.co/elastic/sense/sense-latest.tar.gz to install it on an offline machine.
Start Kibana.
./bin/kibana 

Windows: bin\kibana.bat.
Open Sense your web browser by going to http://localhost:5601/app/sense.


