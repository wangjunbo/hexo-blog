title: 如何使用Python连接hive
author: peace
tags:
  - Hive
categories:
  - 编程
date: 2021-08-31 05:41:00
---
安装依赖
```
pip install sasl
pip install thrift
pip install thrift-sasl
pip install PyHive
```
python脚本示例
```

from pyhive import hive

HOST="127.0.0.1"
PORT=10000
USERNAME="hadoop"
DATABASE="default"

conn=hive.Connection(host=HOST, port=PORT, username=USERNAME,database=DATABASE)
 
cursor = conn.cursor()
#cursor.execute("INSERT INTO TABLE test_out(name,count,time) SELECT name,count(1),to_date(time) FROM test GROUP BY name,to_date(time)")
cursor.execute("SELECT * FROM test")
for result in cursor.fetchall():
    print(result[2])
```
参考 https://segmentfault.com/a/1190000022358127