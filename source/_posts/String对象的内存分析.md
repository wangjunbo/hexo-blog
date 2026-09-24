title: String对象的内存分析
author: peace
tags:
  - Java
  - ''
categories:
  - 编程
date: 2018-10-16 13:37:00
---
### Java中内存分析：

　　栈(Stack) ：存放基本类型的变量数据和对象的**引用**，但对象本身不存放在栈中，而是存放在堆（new 出来的对象）或者常量池中（字符串常量对象存放在常量池中）。

　　堆(heap)：存放所有new出来的对象。

　　常量池(constant pool)：在堆中分配出来的一块存储区域，存储显式的String常量和基本类型常量(float、int等)。另外，可以存储不经常改变的东西(public static final)。常量池中的数据可以共享。

　　静态存储：存放静态成员（static定义的）。

　　![](/css/images/2012012520033341.png)

1)
```
　　String a = "abc";①  
　　String b = "abc";②
```
　　分析：  
　　①代码执行后在常量池(constant pool)中创建了一个值为abc的String对象，②执行时，因为常量池中存在"abc"所以就不再创建新的String对象了。

2)
```
　　String   c   =   new   String("xyz");①  
　　String   d   =   new   String("xyz");②
```
　　分析：①Class被加载时，"xyz"被作为常量读入，在常量池(constant pool)里创建了一个共享的值为"xyz"的String对象；然后当调用到new String("xyz")的时候，会在堆(heap)里创建这个new   String("xyz")对象;②由于常量池(constant pool)中存在"xyz"所以不再创建"xyz"，然后创建新的new String("xyz")。  
3)  
```
　　String   s1   =   new   String("xyz");     //创建二个对象(常量池和堆中)，一个引用   
　　String   s2   =   new   String("xyz");     //创建一个对象(堆中)，并且以后每执行一次创建一个对象，一个引用   
  
　　String   s3   =   "xyz";     //创建一个对象(常量池中)，一个引用   
　　String   s4   =   "xyz";     //不创建对象(共享上次常量池中的数据)，只是创建一个新的引用
```
4)

```
public class TestStr {   
  public static void main(String\[\] args) {   
    // 以下两条语句创建了1个对象。"凤山"存储在字符串常量池中   
    String str1 = "凤山";   
    String str2 = "凤山";   
    System.out.println(str1==str2);//true 
    //以下两条语句创建了3个对象。"天峨"，存储在字符串常量池中，两个new String()对象存储在堆内存中   
    String str3 = new String("天峨");   
    String str4 = new String("天峨");   
    System.out.println(str3==str4);//false 
    //以下两条语句创建了1个对象。9是存储在栈内存中   
    int i = 9;   
    int j = 9;   
    System.out.println(i==j);//true   
 //由于没有了装箱，以下两条语句创建了2个对象。两个1对象存储在堆内存中   
    Integer l1 = new Integer(1);   
    Integer k1 = new Integer(1);   
    System.out.println(l1==k1);//false 

　　//以下两条语句创建了1个对象。1对象存储在栈内存中。自动装箱时对于值从1到127之间的值，使用一个实例。  
    Integer l = 20;//装箱   
    Integer k = 20;//装箱   
    System.out.println(l==k);//true 

//以下两条语句创建了2个对象。i1,i2变量存储在栈内存中，两个256对象存储在堆内存中   
    Integer i1 = 256;   
    Integer i2 = 256;   
    System.out.println(i1==i2);//false   
  }   
}   
```
参考 https://www.cnblogs.com/devinzhang/archive/2012/01/25/2329463.html