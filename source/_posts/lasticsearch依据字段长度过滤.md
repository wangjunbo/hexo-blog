title: java正则表达式去掉标点符号
tags:
  - Java
categories: []
date: 2016-01-29 16:06:00
---
参考http://blog.csdn.net/telnetor/article/details/6041323
```java
			String str = ",.!，，D_NAME。！；‘’”“**dfs  #$%^&()-+1431221\"\"中           国123漢字かどうかのjavaを決定";
			str = str.replaceAll("[\\pP\\pS\\pZ]", "");
			System.out.println(str);
```
```java
DNAMEdfs1431221中国123漢字かどうかのjavaを決定
```
Unicode 编码并不只是为某个字符简单定义了一个编码，而且还将其进行了归类。  
/pP 其中的小写 p 是 property 的意思，表示 Unicode 属性，用于 Unicode 正表达式的前缀。

P：标点字符  
L：字母；  
M：标记符号（一般不会单独出现）；  
Z：分隔符（比如空格、换行等）；  
S：符号（比如数学符号、货币符号等）；  
N：数字（比如阿拉伯数字、罗马数字等）；  
C：其他字符