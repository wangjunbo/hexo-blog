title: >-
  There was a problem confirming the ssl certificate: [SSL:
  TLSV1_ALERT_PROTOCOL_VERSION] tlsv1 alert protocol version (_ssl.c:590) -
  skipping
author: peace
tags:
  - Python
categories:
  - 编程
date: 2018-06-24 10:41:00
---
参考 http://www.qingpingshan.com/m/view.php?aid=384613

在mac上使用pip安装软件包scrapy，报ssl错误：
```
Collecting scrapy
Could not fetch URL https://pypi.python.org/simple/scrapy/: There was a problem confirming the ssl certificate: [SSL: TLSV1_ALERT_PROTOCOL_VERSION] tlsv1 alert protocol version (_ssl.c:661) - skipping
Could not find a version that satisfies the requirement scrapy (from versions: )
No matching distribution found for scrapy
```

相关软件版本   
python：2.7
pip：9.0.1
OSX Sierra 10.12.6
解决方法：

升级pip到最新版本（至少9.0.3）
```
curl https://bootstrap.pypa.io/get-pip.py | python
```
原因是 Python.org sites 终止支持TLS1.0和1.1，TLS需要>=1.2

参考：https://stackoverflow.com/questions/49768770/not-able-to-install-python-packages-ssl-tlsv1-alert-protocol-version

