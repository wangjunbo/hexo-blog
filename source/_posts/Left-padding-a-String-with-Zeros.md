title: Left padding a String with Zeros
tags:
  - Java
categories: []
date: 2016-03-14 18:49:00
---
http://stackoverflow.com/questions/4469717/left-padding-a-string-with-zeros  

将数字格式化为10位,如果数字的长度不够10, 在数字的左边补0.
```java
String.format("%010d", Integer.parseInt(mystring));
```