title: Java djb调试
author: peace
tags:
  - Java
categories:
  - 编程
date: 2018-05-11 15:01:00
---
https://teaspoon-consulting.com/articles/tracing-java-method-calls.html   
https://stackify.com/java-remote-debugging/   
   
   
JVM启动时的参数     
-Xdebug   
-Djava.complier=NONE   
-Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=5000   
   
jdb -attach localhost:5000 启动调试   
   
classes 查看所有正在运行的类   
methods com.eqxiu.Test列出该类下的所有方法   
stop in com.eqxiu.Test.m1 设置断点   
threads列出所有的线程   
thread 0x23b 指定指定的线程   
locals 列出方法的参数   
dump arg 或者 print arg 可以只看某一个参数   
resume 好像是让方法继续执行直到下一个断点   
where :   The where command shows the current stack trace and allows you to see where you’re at:   
step  :   To step to the next instruction   
step up:  You can also use the step up command to run the code to the end of the current method, exit it and stop at the next line of the calling method:   
   
set message = "Goodbye, John" 改变变量的值   
cont or run : To continue execution, use the cont or the run command:   
clear  :  To see the list of available breakpoints, let’s enter the clear command   
clear com.HelloController.hello(java.lang.String) : to clear the breakpoint