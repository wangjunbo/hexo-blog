title: UITextView文字上方一段空白的解决方法
tags:
  - iOS
categories:
  - 编程
date: 2016-01-10 16:04:00
---
凡是继承UIScrollView的控件都会受到UIViewController的这个automaticallyAdjustsScrollViewInsets属性的影响
默认为YES,
当有navigationbar的时候,UITextView的表现就是上面空白
设为NO,UITextView就正常了.
也可以在storyboard上进行设置,取消 Adjust Scroll View Insets前的对号就行了.