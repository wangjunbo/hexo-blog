title: Intellij创建scala工程
author: peace
tags:
  - Java
categories:
  - 编程
date: 2018-07-10 10:42:00
---
1. Create New Project , 之后选择Maven项目，Project SDK选择1.8 , Next
2. GroupId: hohode , Artifactid: scala_test , Version: 1 , Next
3. Finish
4. 右下角弹框点击，Enable Auto-Import
5. 在项目名scala_test上右键，Add Framework Support...
6. 选择Scala，Use library: scala-sdk-2.11.8 , OK
7. 在main下新建scala目录
8. 在新建的scala目录上右键，Mark Directory as -> Sources Root
9. 在scala目录上右键,New -> scala class -> name:FirstScala, kind: Object
10. 
```
object FirstScala {
  def main(args: Array[String]): Unit = {
    val v = 1 + 2
    print(v)
  }
}
```
11. 运行测试

参考 [IntelliJ IDEA创建Maven项目--Scala](https://blog.csdn.net/kicilove/article/details/79971966)