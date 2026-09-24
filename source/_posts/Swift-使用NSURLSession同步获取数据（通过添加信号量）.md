title: Swift - 使用NSURLSession同步获取数据(通过添加信号量)
tags:
  - iOS
categories: []
date: 2016-01-24 22:58:00
---
首先非常感谢代码的原作者,为了查询方便我就在这里又留了一份,请见谅.

原文http://www.hangge.com/blog/cache/detail_816.html

过去通过 NSURLConnection.sendSynchronousRequest() 方法能同步请求数据。从iOS9起，苹果建议废除 NSURLConnection，使用 NSURLSession 代替 NSURLConnection。
 
如果想要 NSURLSession 也能够同步请求，即数据获取后才继续执行下面的代码，使用信号、信号量就可以实现。


```swift
//创建NSURL对象
let urlString:String="http://www.hangge.com"
let url:NSURL! = NSURL(string:urlString)
//创建请求对象
let request:NSURLRequest = NSURLRequest(URL: url)
         
let session = NSURLSession.sharedSession()
         
let semaphore = dispatch_semaphore_create(0)
         
let dataTask = session.dataTaskWithRequest(request,
    completionHandler: {(data, response, error) -> Void in
        if error != nil{
            print(error?.code)
            print(error?.description)
        }else{
            let str = NSString(data: data!, encoding: NSUTF8StringEncoding)
            print(str)
        }
                 
        dispatch_semaphore_signal(semaphore)
}) as NSURLSessionTask
         
//使用resume方法启动任务
dataTask.resume()
         
dispatch_semaphore_wait(semaphore, DISPATCH_TIME_FOREVER)
print("数据加载完毕！")
//继续执行其他代码.......
```