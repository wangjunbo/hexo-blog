title: Nginx的安装
author: peace
tags:
  - Nginx
categories:
  - 编程
date: 2018-02-13 18:26:00
---
Nginx是一款非常著名的HTTP和反向代理服务器，广泛被应用在互联网应用之中，本文将简要介绍其安装的过程。 

本例在Centos6.5上执行。

1 预先执行 [参考](https://my.oschina.net/qianwei4712/blog/1540454)
```
yum install -y gcc-c++
yum install -y pcre pcre-devel
yum install -y zlib zlib-devel
yum install -y openssl openssl-devel
```

这四个需要安装的原因分别是：

nginx是C语言开发，建议在linux上运行。安装nginx需要先将官网下载的源码进行编译，编译依赖gcc环境，如果没有gcc环境，需要安装gcc。

PCRE(Perl Compatible Regular Expressions)是一个Perl库，包括 perl 兼容的正则表达式库。nginx的http模块使用pcre来解析正则表达式，所以需要在linux上安装pcre库。

 zlib库提供了很多种压缩和解压缩的方式，nginx使用zlib对http包的内容进行gzip，所以需要在linux上安装zlib库。

OpenSSL 是一个强大的安全套接字层密码库，囊括主要的密码算法、常用的密钥和证书封装管理功能及SSL协议，并提供丰富的应用程序供测试或其它目的使用。nginx不仅支持http协议，还支持https（即在ssl协议上传输http），所以需要在linux安装openssl库。

2  下载安装文件 

   一般下载的是源代码文件包，需要自行进行编译和安装。  

    ```
    wget  http://nginx.org/download/nginx-1.13.8.tar.gz
    ```
     
3  解压缩文件，并进行编译安装  

    
    tar -xvf nginx-1.13.8.tar.gz
    cd nginx-1.13.8     ## 进入源代码目录
    ./configure        ## 编译源代码
    #如果需要使用https，使用如下命令
    ./configure --with-http_ssl_module
    
4  执行make命令
```
 make 
```
  
5  执行make install命令来完成安装 

```
make install 
```
6 修改端口   
	/usr/local/nginx/conf  /nginx.conf配置文件中的默认端口为80，可以根据需要设定。   
7 Nginx的启动和关闭

   确保系统的 80 端口没被其他程序占用，执行命令： 

```
/usr/local/nginx/sbin/nginx
```

   检查是否启动成功： 

```
netstat -ano|grep 80  ## 有结果输入说明启动成功 
```

   打开浏览器访问此机器的 IP，如果浏览器出现 Welcome to nginx! 则表示 Nginx 已经安装并运行成功。 

   Nginx的重启 

```
/usr/local/nginx/sbin/nginx –s reload
```

8 Nginx主要的配置选项 

   配置文件所在的路径： /usr/local/nginx/conf  
   大家可以编辑自己需要的参数，比如配置进程、连接以及虚拟主机等诸多信息。
   
From: <http://blog.csdn.net/blueheart20/article/details/49886369>