title: Pycharm绘制图形
author: peace
tags:
  - Python
  - 机器学习
categories:
  - 编程
date: 2018-03-09 14:23:00
---
##### Example 1
```
  import matplotlib.pyplot as plt
  x = range(10)
  plt.plot(x, x)
  plt.show()

```
![](http://s11.sinaimg.cn/mw690/001On4Dlzy78ZqoVUW6aa&690)
<!-- more -->
##### Example 2 
```
import pandas as pd
import matplotlib.pyplot as plt
print(pd.__version__)
city_names = pd.Series(['San Francisco', 'San Jose', 'Sacramento'])
population = pd.Series([3, 6, 9])

df = pd.DataFrame({ 'City name': city_names, 'Population': population })

population.plot()
plt.show()

california_housing_dataframe = pd.read_csv("/Users/Downloads/california_housing_train.csv", sep=",")
# print(california_housing_dataframe.describe())
# print(california_housing_dataframe.head())
california_housing_dataframe.hist('housing_median_age')

plt.show()
```
![series image](/img/series.png)
![dataframe image](/img/dataframe.png)