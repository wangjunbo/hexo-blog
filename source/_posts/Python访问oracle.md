title: 'Python操作oracle,日期比较和查询表中所有字段'
author: peace
tags:
  - Python
  - 数据库
categories:
  - 编程
date: 2018-06-05 15:15:00
---
Python访问远程oracle，需要安装一些软件，而这个过程简直就像shit  
cx_Oracle 是一个 Python访问oracle的扩展模块。  
1.安装cx_Oracle
```
yum install python-pip
pip install cx_Oracle
```
2.下载两个访问oracle需要用到的rpm  
[下载地址](http://www.oracle.com/technetwork/topics/linuxx86-64soft-092277.html)
下载过程就像shit,又强制注册，又多次弹出没有同意条款的，多试几次,看你运气了。  
3.安装软件
```
rpm -ivh  oracle-instantclient12.2-basic-12.2.0.1.0-1.x86_64.rpm
```
4.加入环境变量
```
vim /etc/profile
export LD_LIBRARY_PATH=/usr/lib/oracle/12.2/client64/lib:$LD_LIBRARY_PATH
source /etc/profile
```
5.创建一个a.py文件
```
import cx_Oracle
con = cx_Oracle.connect('eqxiu_dev/keqs123@test.db.hohode.cn/oragbk')
print con.version
```
6.测试是否成功
```
python a.py
```
如果出现类似如下的版本号，说明成功了。
```
10.2.0.4.0
```

### 日期比较和查询表中所有字段
```
import cx_Oracle
con = cx_Oracle.connect('username/password@host/server')
print con.version
cur = con.cursor()
cur.execute("select * from base_user where REG_TIME >= to_date('2018-08-08 00:00:00','yyyy-mm-dd hh24:mi:ss') and rownum<20 ")
for result in cur:
    print result

columnsnames = cur.execute("select COLUMN_NAME from user_tab_columns where TABLE_NAME='BASE_USER'").fetchall()
for name in columnsnames:
	print name
cur.close()
con.close() 
```
[Oracle日期类型的比较](https://www.oracle.com/technetwork/cn/articles/dsl/prez-python-timesanddates-093014-zhs.html)
其它查询http://www.oracle.com/technetwork/articles/dsl/python-091105.html   
参考 https://www.linuxidc.com/Linux/2010-10/29187.htm