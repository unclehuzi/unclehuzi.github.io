# pandas.qcut及一个很奇怪的bug


最近用 `pandas.qcut` 分箱遇到了个很奇怪的bug，先Mark一波

<!--more-->

在此之前也顺便记录下 `pandas.qcut`, `pandas.Series.value_counts` 和 `pandas.Series.value_counts`  的用法

## 相关函数

### `pandas.qcut`

基于样本数据排序或分位数分箱

```Python
pandas.qcut(x, q, labels=None, retbins=False, precision=3, duplicates='raise')
```

|  Parameter   |             type             | comment                                             |
| :----------: | :--------------------------: | :-------------------------------------------------- |
|     `x`      |    一维 ndarray 或 Series    |                                                     |
|     `q`      |  int or list-like of float   | 如4，或 [0,0.2,0.8,1]                               |
|   `labels`   | array or False, default None | 对分箱后的区间重命名，要求和bins等数量              |
|  `retbins`   |        bool, optional        | 是否返回边界值                                      |
| `precision`  |        int, optional         | 用于调整精度，一般默认                              |
| `duplicates` |      'raise' or 'drop'       | 'raise': 若边界值重复则报错<br />'drop': 删除重复的 |

 ```python
import pandas as pd

pd.qcut(range(5), 4)

#[(-0.001, 1.0], (-0.001, 1.0], (1.0, 2.0], (2.0, 3.0], (3.0, 4.0]]
#Categories (4, interval[float64, right]): [(-0.001, 1.0] < (1.0, 2.0] ...

pd.qcut(range(5), 3, labels=["good", "medium", "bad"])

#[good, good, medium, bad, bad]
#Categories (3, object): [good < medium < bad]
 ```



### `pandas.Series.value_counts`

> Return a Series containing counts of unique values.

返回的结果默认是根据频数降序排序，且排除 `NaN`

```python
Series.value_counts(normalize=False, sort=True, ascending=False, bins=None, dropna=True)
```

| Parameter   | type                | comment                      |
| ----------- | ------------------- | ---------------------------- |
| `normalize` | bool, default False | If Ture 则返回频率           |
| `sort`      | bool, default True  | 按照频数排序                 |
| `ascending` | bool, default False | If True，从小到大排序        |
| `bins`      | int, optional       | 仅适用于数值型数据，分箱计数 |
| `dropna`    | bool, default True  | If False， 包含 `NaN`        |

```python
>>> pd.Series(range(1,101)).value_counts(bins=4)
(75.25, 100.0]    25
(50.5, 75.25]     25
(25.75, 50.5]     25
(0.9, 25.75]      25
dtype: int64
 
 ###############################################
 >>> pd.Series(range(100)).value_counts(normalize=True,bins=4)
(74.25, 99.0]    0.25
(49.5, 74.25]    0.25
(24.75, 49.5]    0.25
(-0.1, 24.75]    0.25
dtype: float64
```

### `pandas.Series.unique`

> Return unique values of Series object.

```python
Series.unique()
```

```python
>>> pd.Series([3, 1, 2, 3, 4, np.nan]).unique()
array([ 3.,  1.,  2.,  4., nan])
```



## 那个奇怪的bug

`Python`: 3.6.4|Anaconda, Inc. 

`pandas`: 1.1.5

背景是对数据进行等频分箱

```python
pdf['qcut'] = pd.qcut(pdf['prob'],20,duplicates='drop')
```

之后通过`value_counts`计数发现有个区间频数为0，且为17个区间

![image-20211216144937371](https://s2.loli.net/2021/12/16/ndiIZ4wYfvEpbAP.png)

此时，再通过 `unique` 去重，就只有16个区间，上图中的 `(22.0,22.2]` 那个区间就没了

![unique](https://s2.loli.net/2021/12/16/4Ortz5xodCLZqQ1.png)

根本原因还是数据分布的问题，因为本来是要分20个bin，存在较多边界值重复的情况

![valueError](https://s2.loli.net/2021/12/16/Er1q8z4DZKNjOiM.png)

## 解决方案

源码方面的原因暂时不得而知

分箱后得到的数据类型是 `Categorical`将其转换为 `String`后再进行后续的分组统计计算

```python
pdf['qcut'] = pdf['qcut'].astype(str)
```





## Reference

1. https://pandas.pydata.org/docs/reference/api/pandas.qcut.html
2. https://pandas.pydata.org/docs/reference/api/pandas.Series.value_counts.html
3. https://pandas.pydata.org/docs/reference/api/pandas.Series.unique.html
