# Time Series学习笔记

!> Kaggle课程[Time Series](https://www.kaggle.com/learn/time-series)

## 第一节 Linear Regression with Time Series

```python
import pandas as pd

df = pd.read_csv(
    "../input/ts-course-data/book_sales.csv",
    index_col='Date',
    parse_dates=['Date'],
).drop('Paperback', axis=1)

df.head()
```

parse_datas：将字段格式化为日期格式，可参考：https://www.jianshu.com/p/f80586446151



```python
%matplotlib inline
%config InlineBackend.figure_format = 'retina'
```

为了将图片嵌入notebook及提高分辨率



### Time-step features

- time dummy：不知道中文怎么翻译，但就是从日期中转换过来的时间序列吧，因为按照日期的格式的话，比如

| date      |
| --------- |
| 2022-1-11 |
| 2022-1-12 |
| 2022-1-13 |

像这样的数据肯定是没有办法直接作为输入数据的，所以需要转换为数值类型，而且还要保持时间的先后顺序，那么就可以转换为：

| date      | time |
| --------- | ---- |
| 2022-1-11 | 0    |
| 2022-1-12 | 1    |
| 2022-1-13 | 2    |

像这样的一个0 1 2的时间序列，这也就是所说的**time dummy**。

- lag feature：应该叫滞后特征吧，字面意思，将特征滞后

```python
df['Lag_1'] = df['Hardcover'].shift(1)
df = df.reindex(columns=['Hardcover', 'Lag_1'])

df.head()
```

![image-20220205220246510](https://gitee.com/y255413580/img/raw/master/noteimg/image-20220205220246510.png)

这里用到的是1步之后，也就是都往下挪了一行，也可以选择多挪几行，滞后特征后就可以让它在较晚的时候才发生，这样就可以尝试寻找Lag_1和Hardcover之间的关系，也就是滞后的观察值和先前的观察值间的关系。说白了，在这个例子中，就是有可能找到前一天与后一天之间的关系，然后就能够用前一天预测后一天了。

- 总结：**时间索引(time index)和滞后特征(lag feature)是时间序列(Time Series)问题中可提取的最普遍使用最广泛的两个特征**

