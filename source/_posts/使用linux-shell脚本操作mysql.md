title: 使用linux shell脚本操作mysql
author: peace
tags:
  - Linux
  - 数据库
categories:
  - 编程
date: 2018-09-01 16:55:00
---
```
mysql -hbigdata.hohode.com   -P3305    -uhohode  -p123456  -e"select * from scene where statistics_date = '2018-08-31' "
```
查询当天，前一天的日期
```
select CURDATE(),date_sub(curdate(),interval 1 day) from job_qrtz_trigger_log
```

linux shell for 循环遍历字符串
```
for t in "appnsformation" "a" "b"
do
	echo  $t
done
```

参考 http://www.w3school.com.cn/sql/func_curdate.asp
