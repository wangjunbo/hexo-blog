title: Java基础知识
author: peace
tags:
  - Java
categories: []
date: 2018-03-18 17:56:00
---
```
>> 带符号右移。正数右移高位补0，负数右移高位补1。比如：  
4 >> 1，结果是2；-4 >> 1，结果是-2。-2 >> 1，结果是-1。  
>>> 无符号右移。无论是正数还是负数，高位通通补0。  
对于正数而言，>>和>>>没区别。  
对于负数而言，-2 >>> 1，结果是  2147483647（Integer.MAX_VALUE），-1 >>> 1，结果是  2147483647（Integer.MAX_VALUE）。
```
<!-- more -->
for循环的过程和顺序
```java
public class Main {

    private static int index  = -1 ;

    public static int  init(int arg){
        System.out.println("init");
        return arg;
    }

    public static boolean  judge(int arg){
        System.out.println("judge");
        return arg < 3;
    }

    public static void step(){
        index++;
        System.out.println("step , index is "+ index);
    }

    public static void main(String[] args){

        for( index = init(0) ; judge(index) ; step()){
            System.out.println("in for body index is " + index);
        }
    }
}
以下是输出结果：
init
judge
in for body index is 0
step , index is 1
judge
in for body index is 1
step , index is 2
judge
in for body index is 2
step , index is 3
judge
```
