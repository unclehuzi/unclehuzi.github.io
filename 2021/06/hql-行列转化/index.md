# 行转列、列转行


SQL, Python 中解决行转列、列转行的问题

<!--more-->



---



在日常工作中总会遇到类似下图中的问题 👇

![行转列](https://raw.githubusercontent.com/unclehuzi/pic/master/img/image-20210615175136301.png)

{{< admonition type=note title="备注" open=true >}}
我把这种情况称为 **行转列**

*《Python for Data Analysis》* 书中将其称为 ***Pivoting "Wide" to "Long" Format***
{{< /admonition >}}

还有这种问题 👇

![列转行](https://raw.githubusercontent.com/unclehuzi/pic/master/img/image-20210615175345538.png)

{{< admonition type=note title="备注" open=true >}}
我把这种情况称为 **列转行**

*《Python for Data Analysis》* 书中将其称为 ***Pivoting "Long" to "Wide" Format***
{{< /admonition >}}

那么，接下来将针对此类问题，汇总SQL、Python中的实现方式



## 行转列 / "Wide" to "Long"

![行转列](https://raw.githubusercontent.com/unclehuzi/pic/master/img/image-20210615175136301.png)

### SQL

{{< admonition type=warning title="运行环境" open=true >}}
基于 ***spark-2.4.5U3*** 及以上版本
{{< /admonition >}}

```sql
select  score
        ,ym
        ,score_group_value["r1"]  as r1
        ,score_group_value["r2"]  as r2
        ,score_group_value["r3"]  as r3
from
    (
    select  score
            ,ym
            ,str_to_map(concat_ws(',', collect_set(concat_ws(':', range_label, nums)))) as score_group_value
    from    score_group_nums
    group by 1,2
    )
```

基本思路是将表中 `range_label`和`nums`转化为类似`json`的格式，之后通过 `key` 索引得到对应的`value`

这里用的是`collect_set()`，得到的是**聚合**、**去重**后无序的`array`，若需要有序则可用`sort_array()`

### Python

#### 构造数据

```python
df_wide = pd.DataFrame({'score_name': ['ScoreA']*3
                 		,'ym': ['202012']*3
                 		,'range_label': ['r1','r2','r3']
                        ,'nums': [1110,1105,1054]})
df_wide
```

![image-20210615202957219](https://raw.githubusercontent.com/unclehuzi/pic/master/img/image-20210615202957219.png)

#### [pivot()](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.pivot.html) 函数

```python
df_wide.pivot(index=['score_name','ym']
             ,columns='range_label'
             ,values=['nums'])
```

![image-20210615203202914](https://raw.githubusercontent.com/unclehuzi/pic/master/img/image-20210615203202914.png)



## 列转行 / "Long" to "Wide"

![列转行](https://raw.githubusercontent.com/unclehuzi/pic/master/img/image-20210615175345538.png)

### SQL

{{< admonition type=warning title="运行环境" open=true >}}
基于 ***spark-2.4.5U3*** 及以上版本
{{< /admonition >}}

```sql
select  biz_no
        ,ym
        ,range_label
        ,nums
from    score
lateral view outer explode(map('r1',r1,'r2',r2,'r3',r3)) t as range_label,nums
```

`map` 之后，结合 `lateral view explode`[^1] 实现列转行的问题

### Python

#### 构造数据

```python
df_long = pd.DataFrame({'score_name': ['ScoreA']
                 		,'ym': ['202012']
                 		,'r1': [1110]
                        ,'r2': [1105]
                        ,'r3':[1054]})

df_long
```

![image-20210615203711372](https://raw.githubusercontent.com/unclehuzi/pic/master/img/image-20210615203711372.png)

#### [stack()](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.stack.html)函数

```python
df_long.set_index(['score_name','ym']).stack(dropna=False).reset_index().rename(columns={"level_2": "range_label",0:"nums"})
```

![image-20210615204651755](https://raw.githubusercontent.com/unclehuzi/pic/master/img/image-20210615204651755.png)

## Reference

[^1]: https://blog.csdn.net/oopsoom/article/details/26001307




<head> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/all.js"></script> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/v4-shims.js"></script> 
</head> 
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.13/css/all.css">
