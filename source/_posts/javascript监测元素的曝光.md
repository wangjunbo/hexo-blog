title: javascript监测元素的曝光
author: peace
tags:
  - HTML
categories:
  - 编码
date: 2019-11-13 15:51:00
---

```
function printTitle(el) {  
    try {
        var t = el.children[0].children[1].children[0].children[0].innerText
        console.log("valid1 hello callback  " +new Date() + t);  
    }
    catch(err) {
        console.log(err)
    }
}
 
function beginObserve(arg,callback){
    console.log("begin add listener "+arg);
    var arr = document.querySelectorAll(arg);
    var observer = new IntersectionObserver(
        function(objs){
            for(i = 0; i < objs.length ; i++ ){
                const entry1 = objs[i];
                try {
                    if(entry1.target){
                        var element1 = entry1.target
                        const attr1 = element1.getAttribute("bigdata_view");
                        if(attr1 != null && attr1 != "2"){
                            element1.setAttribute("bigdata_view", "2");
                            callback(element1);
                        }
                    }else{
                        console.log("invalid1 hello callback  " +new Date());
                    }
                }
                catch(err) {
                    console.log(err)
                }
            }
    }, { threshold: 0.95 } );

    for (i = 0; i < arr.length; i++) {
        const entry = arr[i];
        const attr = entry.getAttribute("bigdata_view");
        if(attr == null){
            observer.observe(entry);
            entry.setAttribute("bigdata_view", "1");
        }
    }
    setTimeout(function() {
        beginObserve(arg, callback)
    },2000);
}


beginObserve('[tracking_id|="search"],[tracking_id|="re"]',printTitle);

```
制定一个printTitle方法，调用下面的代码就行了
```

beginObserve('[tracking_id|="search"],[tracking_id|="re"]',printTitle);
```
