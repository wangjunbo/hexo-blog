title: Linux expect自动化登录脚本
author: peace
tags:
  - Linux
categories:
  - 编程
date: 2019-12-03 10:16:00
---

之前写了一个自动化登录的脚本，最近运维升级了堡垒机，导致这段时间无法登录，经过摸索排查，发现需要把每个send后的`\n`换成`\r`，然后又可以愉快的玩耍了。
```
#!/usr/bin/expect
set user hohode
set host jump.hohode.com
set password xxxxx
spawn ssh -i /Users/hohode/Documents/company/keys/online/hohode-jumpserver.pem $user@$host
expect "*Opt>*"


if { $argc == 1 } {
     set seq [lindex $argv 0]
}

if { $seq == "nck" } {
    send "hadoop019\r"
    expect "jump@hadoop*"
    send "sudo su -\r"
    send "ssh 190.0.2.129\r"
    send "cd /data/\r"
} elseif { $seq == "test" } {
    send "190.0.2.73\r"
    expect "jump@*"
    send "sudo su -\r"
} elseif { $seq == "app001" || $seq == "app002" || $seq ==  "logserver001"  || $seq ==  "logserver002" || $seq ==  "logserver003" } {
        send "$seq \r"
        expect "jump*"
        send "sudo su -\r"
        if {  $seq == "app001" || $seq == "app002" } {
            send "cd /data/work/pre_tracker/\r"
        }

        if { $seq ==  "logserver001"  || $seq ==  "logserver002" || $seq ==  "logserver003" } {
            send "cd /data/work/\r"
        }
        
} elseif { $seq != "lll" } {

    if { $seq == 1 || $seq == 2  || $seq == 6 } {
        send "hadoop00$seq\r"
    }
    if { $seq == 19 } {
        send "hadoop0$seq\r"
    } else {
        send "$seq\r"
    }

    expect "jump@*"
    send "sudo su -\r"

    if { $seq == 1 || $seq == 2  || $seq == 19 } {
        expect "*root@hadoop*"
        send "su - hdfs\r"
        if { $seq == 1 } {
            expect "*hdfs@hadoop*"
            send "cd shell/new/\r"
        } elseif { $seq == 2 } {
            expect "*hdfs@hadoop*"
            send "cd /data/work/shell/\r"
        }  elseif { $seq == 19 } {
            expect "*hdfs@hadoop*"
            send "cd shell/\r"
        }
    } 
}

interact
expect eof

```