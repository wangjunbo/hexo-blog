title: 使用swift解析json
tags:
  - iOS
categories: []
date: 2016-01-24 21:50:00
---
```swift
            let jsonObject = try NSJSONSerialization.JSONObjectWithData(data, options: NSJSONReadingOptions.MutableContainers)
            print(jsonObject)
            
            let array = jsonObject as! NSArray
            
            //读取数组中某个key所对应的所有值
            print(array.valueForKey("text"))
            
            //读取第一个元素
            print(array[0])
            
            //读取第一个元素的key对应的值
            let text = array[0].valueForKey("text")
            print(text)
            
            //在使用if let语句的时候，swift会自动进行拆包
            if let state = array[0].objectForKey("state") {
                print(state)
            }
```
  结果如下:
```swift
(
        {
        id = 1;
        state = closed;
        text = "Node 1";
    },
        {
        id = 2;
        state = open;
        text = "Node 2";
    },
        {
        id = 3;
        state = open;
        text = "Node 3";
    },
        {
        id = 4;
        state = open;
        text = "Node 4";
    }
)
(
    "Node 1",
    "Node 2",
    "Node 3",
    "Node 4"
)
{
    id = 1;
    state = closed;
    text = "Node 1";
}
Optional(Node 1)
closed
```