# 窗口函数的“窗口”


详述“窗口”的概念，结合初中数学中区间的概念来理解`<window_frame>`，并以计算累计占比为例深化理解。此外，也分享了他人整理的窗口函数汇总表
<!--more-->

---

基于之前整理的

1. 排序
2. 聚合
3. Positional functions: lead(), lag()

在窗口函数应用场景方面算是告一段落了。但是在 “**窗口**” 这个概念上陈述较少，在窗口函数部分的里程碑之际重新 “定义” “窗口”

另外，之前在浏览网页的时候发现了[窗口函数的汇总图](https://learnsql.com/blog/sql-window-functions-cheat-sheet/)

![窗口函数](https://gitee.com/unclehu/pic/raw/master/img/%E7%AA%97%E5%8F%A3%E5%87%BD%E6%95%B0.png)


而本文与图中对应的便是 ***WINDOW FAME*** 部分


## 实现的功能

简单来说是定义窗口函数作用的范围（"FRAME"），通过下面这张图[^1]可以更好的了解 FRAME 的概念

![frame of table](https://upload.wikimedia.org/wikipedia/commons/thumb/0/09/SQLWindowFunction.svg/1280px-SQLWindowFunction.svg.png)

一般而言，

1. 一张表（`Table`）基于`WHERE`条件的作用得到图中 `Result Set` 部分；
2. 窗口函数 `over()` 语句中 `partition`（若有）得到图中 `Partition 1`...`Partition m`
3. **fram** 语句（若有）在partition基础上得到图中 `Frame 1`...`Frame n`


## 语法、规则

![Frame](https://learnsql.com/blog/sql-window-functions-cheat-sheet/window-frame.png)

```sql
<window_frame>  := [rows | range | groups ] between 
                   [ unbounded preceding | <n> preceding | current row ] and
                   [ unbounded following | <n> following | current row ]
```

{{< admonition type=bug title="批注" open=true >}}
目前，只有 `PostgreSQL 11` 及以上版本支持 `groups`
{{< /admonition >}}


`<window_frame>`语句表明，相对于当前行（`current row`）对应的值而言，还有“区间”的概念，“区间”又受到 `rows`或`range` 控制：是行数范围还是值的范围。

* `rows` 对应是行的条件

    如`rows between 1 preceding and unbounded following` 
    
    表示最终的范围是<u>排序后（若有），基于当前行的上 1 行和该`partition`本身的最后一行</u>
    
* `range` 对应是值的范围

    如`range between 1 preceding and 2 following` 
    
    这里我们遵循小学数学中**区间**的性质：<u>左区间的值小于等于右区间的值</u>
    
    因为涉及到值的范围，这里就要分两种情况讨论了，假设当前行对应的值为 `x`
    
    - 顺序排序，即从小到大，`order by column asc`
    
        `[x-1,x+2]`，<u>左区间为当前行的值减1（`x-1`）；右区间为当前行的值加2（`x+2`）</u>
    
    - 逆序排序，即从大到小，`order by column desc`
    
        `[x-2,x+1]`，<u>左区间为当前行的值减2（`x-2`）；右区间为当前行的值加1（`x+1`）</u>


最后再说明下**没有** `<window_frame>` 语句时对应的Frame，此时将取决于是否有`order by`语句，即

* 无 `<window_frame>` 语句、**有** `order by` 语句
  
    Frame 为 `range between unbounded preceding and current row`

    即Frame的第一行为该`partition`的上边界，当前行（`current row`）为下边界

* 无 `<window_frame>` 语句、**无** `order by` 语句

    Frame 为 `rows between unbounded preceding and unbounded following`
    
    即Frame的边界就是`partition`的边界
    

关于，无`<window_frame>`语句的情况，总结如下

\ | 无 `<window_frame>`
---|---
有 `order by` | `range between unbounded preceding and current row`
无 `order by` | `rows between unbounded preceding and unbounded following`



## 应用：一个例子

一堆枯涩的陈述，不如直接来个小例子：计算累计占比

实际业务中，在定义模型目标变量`y`的时候，往往也会结合数据的分布。如，风控场景中定义逾期  <u>x</u> 天以上为`bad`

假设`samples` 表中记录着一笔订单的逾期状态，`over_due_days` 表示逾期天数

希望得到的数据样式如下表所示


over_due_days | c_sum | c_sum_rate
---|---|---
0 | 1000 | 1
1 | 100 | 0.1
2 | 90 | 0.09
3 | 85 | 0.085
4 | 80 | 0.08
...|...|...

`over_due_days` 为1的那一行表示 $逾期天数 \geq 1$的订单数以及占总订单的比例，即

$$c\\_sum\\_rate=\frac{over\\_due\\_days \geq 1 的订单数}{总订单数}$$

```sql
select  over_due_days
        ,sum(nums) over(order by over_due_days nulls last
                                 range between current row 
                                               and unbounded following)      as c_sum
        ,sum(nums) over(order by over_due_days nulls last
                                 range between current row 
                                               and unbounded following)
            / (select count(1) from samples)                                 as c_sum_rate
from(-- 分逾期天数统计订单数量
    select  over_due_days
            ,count(1)  as nums
    from    samples
    group by 1
    )
;
```


## 总结

本文便是窗口函数部分的“收官之作”了。

主要是对“窗口”的概念展开了详细的陈述，结合初中数学中区间的概念来理解`<window_frame>`，并以**计算累计占比**为例深化理解。此外，也分享了他人整理的[窗口函数](https://learnsql.com/blog/sql-window-functions-cheat-sheet/ "窗口函数")

## Reference

[^1]: https://en.wikibooks.org/wiki/Structured_Query_Language/Window_functions



<head> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/all.js"></script> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/v4-shims.js"></script> 
</head> 
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.13/css/all.css">
