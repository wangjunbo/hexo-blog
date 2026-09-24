title: Linux单独安装oracle客户端
author: peace
tags:
  - 数据库
categories:
  - 编程
date: 2018-06-29 20:31:00
---
[linux单独安装oracle客户端及exp/imp工具配置](https://www.jianshu.com/p/8cb0730f103b)
这个链接的文章讲的非常好。其中有一些需要注意的我写下来以便查阅。

下边的配置最好不要，因为可能会引起乱码。如果已经出现了乱码，把它删掉，然后关闭连接服务器的窗口，重新打开打开一个窗口连接服务器。
```
export NLS_LANG='simplified chinese_china.ZHS16GBK' 
```

tnsnames.ora文件的位置不要放错了，是在network/admin目录下。还有里面的内容一定要顶格写，不能有缩进，否则会报错ORA-12154: TNS:could not resolve the connect identifier specified。这个文件中有四个参数需要改，第一行的名字DESCRIPTION，还有服务器名，端口号，还有SERVICE_NAME,其中DESCRIPTION和SERVICE_NAME最好一致。配置如下：
```
orac1 =  
(DESCRIPTION =  
  (ADDRESS = (PROTOCOL = TCP)(HOST = www.hohode.com)(PORT = 1521))  
  (CONNECT_DATA =  
    (SERVER = DEDICATED)  
    (SERVICE_NAME = orac1)  
   )  
)
```
下边的配置是错误的，因为有缩进，导致连不上oracle服务。
```
    orac1 =  
    (DESCRIPTION =  
      (ADDRESS = (PROTOCOL = TCP)(HOST = www.hohode.com)(PORT = 1521))  
      (CONNECT_DATA =  
        (SERVER = DEDICATED)  
        (SERVICE_NAME = orac1)  
       )  
    )
```
### 登录oracle
```
sqlplus /nolog
conn username/password@service_name
```