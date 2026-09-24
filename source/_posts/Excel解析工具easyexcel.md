title: Excel解析工具easyexcel
author: peace
tags:
  - Office
categories:
  - 编程
date: 2018-09-26 12:59:00
---
Java解析、生成Excel比较有名的框架有Apache poi、jxl。但他们都存在一个严重的问题就是非常的耗内存，poi有一套SAX模式的API可以一定程度的解决一些内存溢出的问题，但POI还是有一些缺陷，比如07版Excel解压缩以及解压后存储都是在内存中完成的，内存消耗依然很大。easyexcel重写了poi对07版Excel的解析，能够原本一个3M的excel用POI sax依然需要100M左右内存降低到KB级别，并且再大的excel不会出现内存溢出，03版依赖POI的sax模式。在上层做了模型转换的封装，让使用者更加简单方便。   
<!-- more -->
###  其他开源框架使用复杂
对POI有过深入了解的估计才知道原来POI还有SAX模式。但SAX模式相对比较复杂，excel有03和07两种版本，两个版本数据存储方式截然不同，sax解析方式也各不一样。想要了解清楚这两种解析方式，才去写代码测试，估计两天时间是需要的。再加上即使解析完，要转换到自己业务模型还要很多繁琐的代码。总体下来感觉至少需要三天，由于代码复杂，后续维护成本巨大。   
### 其他开源框架存在一些BUG修复不及时
由于我们的系统大多数都是大并发的情况下运行的，在大并发情况下，我们会发现poi存在一些bug,如果让POI团队修复估计遥遥无期了。所以我们在easyexcel对这些bug做了规避。 如下一段报错就是在大并发情况下poi抛的一个异常。
```
Caused by: java.io.IOException: Could not create temporary directory '/home/admin/dio2o/.default/temp/poifiles'
at org.apache.poi.util.DefaultTempFileCreationStrategy.createTempDirectory(DefaultTempFileCreationStrategy.java:93) ~[poi-3.15.jar:3.15]
at org.apache.poi.util.DefaultTempFileCreationStrategy.createPOIFilesDirectory(DefaultTempFileCreationStrategy.java:82) ~[poi-3.15.jar:3.15]
```
报错地方poi源码如下
```
    private void createTempDirectory(File directory) throws IOException {
        if (!(directory.exists() || directory.mkdirs()) || !directory.isDirectory()) {
            throw new IOException("Could not create temporary directory '" + directory + "'");
        }
    }
```
仔细看代码容易明白如果在并发情况下，如果2个线程同时判断directory.exists()都 为false,但执行directory.mkdirs()如果一些线程优先执行完，另外一个线程就会返回false。最终 throw new IOException("Could not create temporary directory '" + directory + "'")。针对这个问题easyexcel在写文件时候首先创建了该临时目录，避免poi在并发创建时候引起不该有的报错。
### Excel格式分析
* xls是Microsoft Excel2007前excel的文件存储格式，实现原理是基于微软的ole db是微软com组件的一种实现，本质上也是一个微型数据库，由于微软的东西很多不开源，另外也已经被淘汰，了解它的细节意义不大，底层的编程都是基于微软的com组件去开发的。
* xlsx是Microsoft Excel2007后excel的文件存储格式，实现是基于openXml和zip技术。这种存储简单，安全传输方便，同时处理数据也变的简单。
* csv 我们可以理解为纯文本文件，可以被excel打开。他的格式非常简单，解析起来和解析文本文件一样。

### 核心原理
写有大量数据的xlsx文件时，POI为我们提供了SXSSFWorkBook类来处理，这个类的处理机制是当内存中的数据条数达到一个极限数量的时候就flush这部分数据，再依次处理余下的数据，这个在大多数场景能够满足需求。 读有大量数据的文件时，使用WorkBook处理就不行了，因为POI对文件是先将文件中的cell读入内存，生成一个树的结构（针对Excel中的每个sheet，使用TreeMap存储sheet中的行）。如果数据量比较大，则同样会产生java.lang.OutOfMemoryError: Java heap space错误。POI官方推荐使用“XSSF and SAX（event API）”方式来解决。 分析清楚POI后要解决OOM有3个关键。   
##### 1、文件解压文件读取通过文件形式  
![](https://camo.githubusercontent.com/0abad8e1a498995931bd4fc48f05ca5e830002b4/687474703a2f2f617461322d696d672e636e2d68616e677a686f752e696d672d7075622e616c6979756e2d696e632e636f6d2f65336133353030303134633935663731313864386332303061353161636162342e706e67)
##### 2、避免将全部全部数据一次加载到内存
![](https://camo.githubusercontent.com/c2856ee4cdf42ec293f791da4aa00057e7f8d0bc/687474703a2f2f617461322d696d672e636e2d68616e677a686f752e696d672d7075622e616c6979756e2d696e632e636f6d2f38326262313935616336323533323936336232333634643265346461323365352e706e67)
##### 3、抛弃不重要的数据
Excel解析时候会包含样式，字体，宽度等数据，但这些数据是我们不关心的，如果将这部分数据抛弃可以大大降低内存使用。Excel中数据如下Style占了相当大的空间。

可以试用一下easyexcel,如下：

### 使用maven依赖
```
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>easyexcel</artifactId>
    <version>{latestVersion}</version>
</dependency>
```
### 读Excel
```
public void noModelMultipleSheet() {
        InputStream inputStream = getInputStream("2007NoModelMultipleSheet.xlsx");
        try {
            ExcelReader reader = new ExcelReader(inputStream, ExcelTypeEnum.XLSX, null,
                new AnalysisEventListener<List<String>>() {
                    @Override
                    public void invoke(List<String> object, AnalysisContext context) {
                        System.out.println(
                            "当前sheet:" + context.getCurrentSheet().getSheetNo() + " 当前行：" + context.getCurrentRowNum()
                                + " data:" + object);
                    }
                    @Override
                    public void doAfterAllAnalysed(AnalysisContext context) {

                    }
                });

            reader.read();
        } catch (Exception e) {
            e.printStackTrace();

        } finally {
            try {
                inputStream.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
```

### 写Excel
```
@Test
public void test1() throws FileNotFoundException {
        OutputStream out = new FileOutputStream("/Users/jipengfei/78.xlsx");
        try {
            ExcelWriter writer = new ExcelWriter(out, ExcelTypeEnum.XLSX);
            //写第一个sheet, sheet1  数据全是List<String> 无模型映射关系
            Sheet sheet1 = new Sheet(1, 0,ExcelPropertyIndexModel.class);
            writer.write(getData(), sheet1);
            writer.finish();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            try {
                out.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
```

### web下载实例写法
```
package com.alibaba.china.pte.web.seller.dingtalk.rpc;

import java.io.IOException; import java.text.SimpleDateFormat; import java.util.ArrayList; import java.util.Date; import java.util.List;

import javax.servlet.ServletOutputStream; import javax.servlet.http.HttpServletRequest; import javax.servlet.http.HttpServletResponse;

import com.alibaba.excel.ExcelWriter; import com.alibaba.excel.metadata.Sheet; import com.alibaba.excel.support.ExcelTypeEnum;

import org.springframework.web.bind.annotation.GetMapping; import org.springframework.web.bind.annotation.RequestMapping; import org.springframework.web.bind.annotation.RestController;

/**

@author jipengfei

@date 2018/05/25 */ @RequestMapping("/api/dingtalk") @RestController public class Down {

@GetMapping("/a.htm") public void cooperation(HttpServletRequest request, HttpServletResponse response) { ServletOutputStream out = null; try { out = response.getOutputStream(); } catch (IOException e) { e.printStackTrace(); } ExcelWriter writer = new ExcelWriter(out, ExcelTypeEnum.XLSX, true); try {

     String fileName = new String(("UserInfo " + new SimpleDateFormat("yyyy-MM-dd").format(new Date()))
         .getBytes(), "UTF-8");
     Sheet sheet1 = new Sheet(1, 0);
     sheet1.setSheetName("第一个sheet");
     writer.write0(getListString(), sheet1);
     response.setContentType("multipart/form-data");
     response.setCharacterEncoding("utf-8");
     response.setHeader("Content-disposition", "attachment;filename="+fileName+".xlsx");
     out.flush();

 } catch (Exception e) {
     e.printStackTrace();
 }finally {
     writer.finish();
     try {
         out.close();
     } catch (IOException e) {
         e.printStackTrace();
     }

 }
}

private List<List> getListString() { List list = new ArrayList(); list.add("ooo1"); list.add("ooo2"); list.add("ooo3"); list.add("ooo4"); List list1 = new ArrayList(); list1.add("ooo1"); list1.add("ooo2"); list1.add("ooo3"); list1.add("ooo4"); List<List> ll = new ArrayList<List>(); ll.add(list); ll.add(list1); return ll; }

}
```
参考 https://github.com/alibaba/easyexcel