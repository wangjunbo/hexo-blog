title: Python爬虫工具selenium
author: peace
tags:
  - Python
  - 爬虫
categories:
  - 编程
date: 2018-08-21 22:26:17
---

[最新chromedriver下载地址](https://chromedriver.storage.googleapis.com/index.html?path=2.41/)   

解压下载的chromedriver_mac64.zip, 得到chromedriver   
切换到root用户下   
然后将chromedriver移动到/usr/bin/目录下   
然后就可以使用webdriver了   

```
chromeOptions1 = Options()
# chromeOptions1.add_argument("--headless")
driver = webdriver.Chrome(chrome_options =chromeOptions1)
driver.get(uri)
print(driver.title)
# driver.find_element_by_id("k").get_attribute()
divs=driver.find_element_by_class_name("community-wrap").find_elements_by_class_name("list-item")
```
