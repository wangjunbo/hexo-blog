title: 通过linux脚本将日志文件定时备份
author: peace
tags:
  - Linux
categories:
  - 编程
date: 2018-07-27 16:29:00
---
脚本如下，将如下代码命名为roll_log.sh
```
source_file=$1
keep_days=5
if [ -n "$2" ] ;then
	keep_days=$2
fi
log_dir=`dirname $source_file`
cd `dirname "$0"`
pwd
d=$(date "+%Y%m%d%H%M%S" -d '-1 hour')
filename=${source_file}_${d}.log
echo $filename

# file size should bigger than 1M
size=`ls -l  $source_file | awk '{print $5}'`
if [ $size -gt 1048576 ]; then
   echo "size is bigger than 1048576"
   cp -f $source_file  $filename
else
   echo "size is less than 1048576"
fi

echo a >$source_file
d1=$(date "+%Y%m%d" -d "-${keep_days} day")
oldfile=${log_dir}/*${d1}*.log
echo $oldfile
rm -f $oldfile
```

将roll.log放到/root/目录下，然后通过crontab -e将其加入定时任务
```
* * * * * sh /root/roll_log.sh /path/to/filename.log 1
```
roll_log.sh接收两个参数1.文件名，2.天数，即日志保留多长时间