title: CDH集群安装
author: peace
tags:
  - 大数据
  - ''
categories:
  - 编程
date: 2018-12-07 17:05:00
---
1.安装工具
```
mkdir -p /data/software/
cd /data/software/
wget http://www.theether.org/pssh/pssh-1.4.3.tar.gz
tar -zxvf pssh-1.4.3.tar.gz
cd pssh-1.4.3/
python setup.py install
```

### 准备工作
* 查看服务器系统版本
```
[root@xxx software]#  cat /etc/redhat-release
CentOS Linux release 7.2.1511 (Core)
```

* 首先修改各服务器的主机名
```
hostnamectl set-hostname  hadoop1
localectl set-locale LANG=zh_CN.utf8
.....
```
* 在各服务器生成秘钥
```
ssh-keygen -t rsa
```

* 修改/etc/hosts
```
vim /etc/hosts
```
* 配置免密码登录
将Cloudera Manager Server的秘钥放到追加到各服务器/root/.ssh/authorized_keys


* 同步时间,不是必须的(用的腾讯云)
```
# crontab -e
*/20 * * * * /usr/sbin/ntpdate ntpupdate.tencentyun.com >/dev/null &
```



* 添加用户和组
```
useradd --system --home=/data/work/cm-5.7.2 --no-create-home --shell=/bin/false --comment "Cloudera SCM User" cloudera-scm
查看
id cloudera-scm
以下为可选操作
useradd sqoop2 -g sqoop
groupadd hdfs
groupadd hadoop
useradd hdfs -g hadoop -d /var/lib/hadoop-hdfs/ -c 'Hadoop HDFS'
```

* 修改部分配置[应该不是必须的]  
以root 用户执行命令
```
echo 10 > /proc/sys/vm/swappiness
echo never > /sys/kernel/mm/transparent_hugepage/defrag
echo never > /sys/kernel/mm/transparent_hugepage/enabled
```

* 安装JDK8并配置好环境
```
root@hadoop001 work]# tar -cvf jdk8.tar /data/work/java1.8
root@hadoop001 work]# scp jdk8.tar root@hadoop019:/data/software/
[root@hadoop019 work]# vim /etc/profile
export JAVA_HOME=/data/work/jdk/jdk1.8.0_111/
export JRE_HOME=/data/work/jdk/jdk1.8.0_111//jre
export CLASSPATH=.:$CLASSPATH:$JAVA_HOME/lib:$JRE_HOME/lib
export LD_LIBRARY_PATH=/usr/lib/oracle/11.2/client64/lib/:$LD_LIBRARY_PATH
export PATH=$PATH:$JAVA_HOME/bin:$MVN_HOME/bin:$JRE_HOME/bin
[root@hadoop019 work]# source /etc/profile
[root@hadoop019 work]# java -version
```


### 挂载磁盘
查看所有磁盘 fdisk -l
查看现有磁盘信息 df -hT
格式化磁盘 mkfs.xfs /dev/vdb
挂载磁盘
	mkdir /data
    mount /dev/vdb /data
   
### 下载Cloudera Manager
下载相应的cloudera-manager-centos7-cm*.tar.gz到/opt/cloudera-manager/
下载地址：http://archive.cloudera.com/cm5/cm/5/
```
wget https://archive.cloudera.com/cm5/cm/5/cloudera-manager-centos7-cm5.16.1_x86_64.tar.gz
wget http://archive.cloudera.com/cdh5/parcels/5.16.1/CDH-5.16.1-1.cdh5.16.1.p0.3-el7.parcel
wget http://archive.cloudera.com/cdh5/parcels/5.16.1/CDH-5.16.1-1.cdh5.16.1.p0.3-el7.parcel.sha1
wget http://archive.cloudera.com/cdh5/parcels/5.16.1/manifest.json
```



### 安装第三方依赖
```
yum install chkconfig python bind-utils psmisc libxslt zlib sqlite fuse fuse-libs redhat-lsb cyrus-sasl-plain cyrus-sasl-gssapi
```

### 创建数据库
    如下命令是创建部署各个服务所需的数据库，我本人倾向用不用先创建好，用的时候就可以直接部署服务了，不必再来数据库进行创建。
```
create database hive DEFAULT CHARSET utf8 COLLATE utf8_general_ci;
create database amon DEFAULT CHARSET utf8 COLLATE utf8_general_ci;
create database hue DEFAULT CHARSET utf8 COLLATE utf8_general_ci;
create database monitor DEFAULT CHARSET utf8 COLLATE utf8_general_ci;
create database oozie DEFAULT CHARSET utf8 COLLATE utf8_general_ci;
```


### 解压并修改配置文件
```
tar -zxvf cloudera-manager-centos7-cm5.16.1_x86_64.tar.gz

vim cm-5.16.1/etc/cloudera-scm-agent/config.ini
修改server_host为Cloudera Manager Server的地址
server_host=hadoop1
```
### 配置仓库目录
```
[root@hadoop1 cloudera-manager]# mkdir -p /opt/cloudera/parcel-repo
[root@hadoop1 cloudera-manager]# chown cloudera-scm:cloudera-scm /opt/cloudera/parcel-repo
[root@hadoop1 cloudera-manager]# cd /opt/cloudera/parcel-repo
[root@hadoop1 parcel-repo]# cp /data/work/cloudera-manager/
CDH-5.16.1-1.cdh5.16.1.p0.3-el7.parcel           cloudera/                                        cm-5.16.1/
CDH-5.16.1-1.cdh5.16.1.p0.3-el7.parcel.sha1      cloudera-manager-centos7-cm5.16.1_x86_64.tar.gz  manifest.json
[root@hadoop1 parcel-repo]# cp /data/work/cloudera-manager/CDH-5.16.1-1.cdh5.16.1.p0.3-el7.parcel*
cp：是否覆盖"/data/work/cloudera-manager/CDH-5.16.1-1.cdh5.16.1.p0.3-el7.parcel.sha1"？ ^C
[root@hadoop1 parcel-repo]# cp /data/work/cloudera-manager/CDH-5.16.1-1.cdh5.16.1.p0.3-el7.parcel* ./
[root@hadoop1 parcel-repo]# ls
CDH-5.16.1-1.cdh5.16.1.p0.3-el7.parcel  CDH-5.16.1-1.cdh5.16.1.p0.3-el7.parcel.sha1
[root@hadoop1 parcel-repo]# cp /data/work/cloudera-manager/manifest.json ./
[root@hadoop1 parcel-repo]# ls
CDH-5.16.1-1.cdh5.16.1.p0.3-el7.parcel  CDH-5.16.1-1.cdh5.16.1.p0.3-el7.parcel.sha1  manifest.json
```

初始化数据库
    此操作在主节点上进行，初始脚本配置数据库scm_prepare_database.sh，操作命令如下：

[root@cdh01 ~]/opt/cloudera-manager/cm-5.7.2/share/cmf/schema/scm_prepare_database.sh mysql -hcdh01 -uroot -proot --scm-host cdh01 scmdbn scmdbu scmdbp
    说明：这个脚本就是用来创建和配置CMS需要的数据库的脚本。各参数是指：

    mysql：数据库用的是mysql，如果安装过程中用的oracle，那么该参数就应该改为oracle。

    -hcdh01：数据库建立在cdh01主机上面。也就是主节点上面。

    -uroot：root身份运行mysql。-proot：mysql的root密码是root。

    --scm-host cdh01：CMS的主机，一般是和mysql安装的主机是在同一个主机上。

    最后三个参数是：数据库名，数据库用户名，数据库密码。

    执行完成命令正常如下：


    在这个地方就可以解释上面为什么要改jar名了。

### 配置CDH从节点目录
    在所有的节点上创建parcels目录，操作如下：

mkdir -p /opt/cloudera/parcels
chown cloudera-scm:cloudera-scm /opt/cloudera/parcels
    解释：Clouder-Manager将CDH从主节点的/opt/cloudera/parcel-repo目录中抽取出来，分发解压激活到各个节点的/opt/cloudera/parcels目录中。

### 启动Agent
```
/opt/cloudera-manager/cm-5.11.0/etc/init.d/cloudera-scm-agent start
```

### 在管理页面进行配置
* 登录http://hadoop1:7180
* 点击“主机”->“所有主机” ，可以看到所有节点，包括刚才添加的节点。这个时候新添加节点的状态是红的，正常情况下，过2分钟，节点的状态变为绿色正常。
![](/css/images/cdh1.png)
* 点击右上角“向集群添加主机”，然后点击 继续
* 输入hadoop05,点击 搜索 ,出现如下界面
![](/css/images/cdh2.png)
* 点击 当前管理的主机(1)
* 按照提示进行下边的操作

### 为新节点分配角色
例如：回到首页面，点击 HDFS ，点击 实例 ，选中新添加节点，点击 添加角色实例，然后按照提示依次进行。