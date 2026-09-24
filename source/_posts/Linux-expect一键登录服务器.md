title: Linux expect一键登录服务器
author: peace
tags:
  - Linux
  - Linux
categories:
  - 编程
date: 2018-09-04 22:26:00
---
之前每次登录服务器都是这样的。    
1.先登录堡垒机    
2.选择服务器    
3.输入服务器名或编号    
4.先切换到root用户    
5.再切换到hdfs用户    
具体登录过程如下图(红框的位置需要手动输入内容)：
![具体登录过程](/css/images/003.png)
如此这样的话，每一次登录服务器，进行hadoop操作，都需要进行5步操作，如果密码不记得，还得拷贝密码，甚是麻烦，也非常的耽误时间，影响工作效率。  

今天研究了一些Linux的expect自动登录功能，非常好用，在这里可以大显身手。先贴一下代码，其中的秘钥，用户名和密码需要替换成自己的。还有expect的内容和send的内容也要根据自己的实际情况进行替换。
```

#!/usr/bin/expect
set user wangjunbo
set host 13.6.6.9
set password RFCQQ
spawn ssh -i /Users/Documents/company/xxx.pem  $user@$host
expect "*passphrase*"
send "$password\r"
if { $argc == 0 } {
	 set seq 1
}
if { $argc == 1 } {
     #puts "not two"
     set seq [lindex $argv 0]
}

if { $seq != "lll" } {

    expect "*ID>*"
    if { $seq == 1 || $seq == 2 } {
        send "hadoop00$seq\n"
    } else {
        send "$seq\n"
    }

    expect "ops*"
    send "sudo su -\n"

    if { $seq == 1 || $seq == 2 } {
        expect "*root@hadoop*"
        send "su - hdfs\n"
        if { $seq == 1 } {
            expect "*hdfs@hadoop*"
            send "cd shell/new/\n"
        } elseif { $seq == 2 } {
            expect "*hdfs@hadoop*"
            send "cd /data/work/shell/\n"
        }
    } elseif { $seq == "app001" || $seq == "app002" } {
        expect "root*"
        send "cd /data/apps/recommend/\n"
    }
}

interact
expect eof
```
$argc == 0的意思是如果没有参数，那么就默认登录hadoop001服务器。   
$argc == 1的意思是如果有一个参数，就去第一个参数[lindex $argv 0]作为登录服务器名。  
" if { $argc == 0 } { " 后边的}{之间一定要有一个空格，否则会报错“extra characters after close-brace”。
将上边的内容修改后保存为/usr/local/bin/lg文件,可能还需要使切换到root用户(Mac用户使用sudo su -切换到root用户)。  
修改权限
```
chmod 777 /usr/local/bin/lg
```
如果不放到/usr/local/bin/目录下，在其它目录下执行，需要使用如下的方式
```
./lg
```
而不能用如下的方式
```
sh lg
```

然后使用直接在命令行使用lg进行登录：  
 
下边是具体的效果：
![具体登录过程](/css/images/006.png)

整个过程就输入了一个lg，然后回车就可以了，是不是很方便。