title: Linux在脚本中切换目录和用户
author: peace
tags:
  - Linux
categories:
  - 编程
date: 2018-05-17 11:37:00
---
```
cd `dirname $0`
pwd
dir=$(pwd)
echo $dir
su - hdfs <<EOF

echo "start inner $(date)"
pwd
cd $dir
pwd
ls
#./sync.py
echo "end inner $(date)"

exit;
EOF
```

cd `dirname $0` 切换到脚本文件所在的目录。  
dir=$(pwd) 将当前目录赋值给dir。  
```
su - hdfs <<EOF
exit;
EOF
```
切换用户进行执行。  
cd $dir 切换目录。  