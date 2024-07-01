# 窗口函数-聚合


“抛” **计算组内占比** “引” 聚合窗口函数

<!--more-->

窗口函数中 求和（`sum`）、均值（`avg`）、极值（`max`, `min`）、计数（`count`）等结合聚合函数使用的场景也较多。

数据分析过程中经常会遇到**计算组内占比**的情况。


计算 **多个模型分以及多个时间段** 的 psi 时，（等频/等距）分箱之后计算各箱样本占总样本数的百分比




示例如下表所示，


|model|ym|bucket | act_rate|
|---|---|---|---|
|A|202103|1	|0.1209|
|A|202103|2	|0.1148|
|A|202103|3	|0.1089|
|A|202103|4	|0.1041|
|A|202103|5	|0.1004|
|A|202103|6	|0.0983|
|A|202103|7	|0.0984|
|A|202103|8	|0.0937|
|A|202103|9	|0.0892|
|A|202103|10|0.0714|

比较方便的操作方式就是结合 `sum() over()` 函数计算组内占比。

```sql
select  model
        ,ym
        ,bucket
        ,( nums / sum(nums) over(partition by model,ym) ) as act_rate
from    model_bucket_nums
```

其他几个聚合函数只是实现的功能不同，最后还是要各取所需了。



<head> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/all.js"></script> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/v4-shims.js"></script> 
</head> 
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.13/css/all.css">
