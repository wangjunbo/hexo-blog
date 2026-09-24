title: 在Mac上使用iTerm2登录堡垒机2
author: peace
tags:
  - Mac
categories:
  - 编程
date: 2018-05-09 14:15:00
---
经过不断的尝试，今天终于成功了。之前还写过一篇相似的博客，内容很繁琐，今天的方法很简单。  

a 先安装expect
```
brew install expect
```

b 新建一个文件lg
内容如下,修改相关内容
```
#!/usr/bin/expect
set user aa
set host 13.06.04.19
set password KY7Fk8
spawn ssh -i /Users/jack/Documents/company/jack.pem  $user@$host
expect "*passphrase*"
send "$password\r"
interact
expect eof
```

c 给文件授权
```
chmod 777 lg
```
d 将文件移动到/usr/local/bin/目录下，然后就可以直接在命令行输入lg登录堡垒机了

### 其他
如果使用sh lg执行命令的时候，可能会发现expect报错： spawn: command not found  
[因为这个脚本不是bash脚本。](http://blog.51cto.com/2367685/1958184)

### 参考
http://www.tuijiankan.com/2015/05/15/iterm2-mac-ssh-with-no-password/
https://blog.csdn.net/Jerome_s/article/details/77351507

