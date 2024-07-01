# SQL计算多个变量的IV


RT，SQL计算多个变量的***IV***（Information Value）

<!--more-->

---

## 背景

变量的预测能力往往可以通过IV值来判断，类似之前的 [SQL计算PSI]({{< ref "posts/20210619-sql计算psi/index.md" >}})

IV值也有经验区间供参考，以及可通过SQL完成指标的计算

| Information Value | Predictive Power |
| :------: | :------------: |
| < 0.02 | useless for prediction |
|0.02 - 0.1	| weak predictor |
|0.1 - 0.3|	medium predictor|
|0.3 - 0.5|	strong predictor|
|> 0.5 |	suspicious or too good|

## 计算公式

关于 ***IV*** 的详细介绍，可参考[这篇文章](https://zhuanlan.zhihu.com/p/80134853)

具体计算公式如下

$$IV=\sum_{i=1}^{n}(\frac{Bad_i}{Bad_T} - \frac{Good_i}{Good_T}) \times WOE_i$$

其中，

$$WOE_i=\ln(\frac{Bad_i}{Bad_T}) - \ln(\frac{Good_i}{Good_T})$$


* Bad、Good即表示正负样本，风控场景有好、坏的称呼
* $n$ 为分箱的个数
* $Bad_i$, $Good_i$ 表示第i个箱子“坏”、“好”人数
* $Bad_T$, $Good_T$ 表示“坏”、“好”总人数


## 计算样例

分箱方式：等频（缺失值单独划为一箱）

| score_group | group_bad_i | group_good_i | woe_i    | iv_i   |
| :-----------: | :-----------: | :------------: | :--------: | :------: |
| r1          | 271         | 31882        | 0.218363 | 0.0054 |
| r2          | 225         | 30572        | 0.074301 | 0.0006 |
| r3          | 195         | 29107        | -0.01969 | 0.0000 |
| r4          | 188         | 28761        | -0.04429 | 0.0002 |
| r5          | 163         | 28400        | -0.17435 | 0.0025 |
| r6          | 182         | 27387        | -0.02778 | 0.0001 |
| r7          | 194         | 28058        | 0.01187  | 0.0000 |
| r8          | 160         | 24564        | -0.04782 | 0.0002 |
| r9          | 158         | 29625        | -0.24774 | 0.0052 |
| r10         | 70          | 17302        | -0.52404 | 0.0118 |
| missing     | 327         | 36519        | 0.270413 | 0.0098 |



以上例子最终得到 $IV=\sum_{i=1}^{11}(IV_i)=0.0358$

## SQL实现

准备好预测变量（$X$）和目标变量（$y$），`score`表形如

| ym     | no   | x1   | x2   | x3   | x4     | y    |
| :------: | :------: | :------: | :------: | :------: | :------: | :------: |
| 202101 | a1   | 617  | 481  | 773  | 671.68 | 1    |
| 202102 | a2   | 585  | 585  | 522  | 600.56 | 0    |
| 202102 | a3   | 617  | 548  | 677  | 635.68 | 1    |
| 202102 | a4   | 647  | null | 765  | 655.63 | 0    |
| 202102 | a5   | 596  | 478  | 656  | 635.3  | 0    |
| 202102 | a6   | 636  | 618  | 595  | 630    | 0    |
| 202102 | a7   | 714  | 572  | 842  | 644.28 | 0    |
| 202012 | a8   | null | 495  | 720  | 628.79 | 0    |
| 202012 | a9   | 636  | 618  | 595  | 426    | 0    |
| 202012 | a10  | 557  | 562  | null | 589    | 1    |

基于此得到各个变量在不同月份的预测能力

这里依然涉及窗口函数的应用以及行列互转

* [窗口函数-聚合]({{< ref "posts/hql-窗口函数-聚合/index.md" >}})
* [窗口函数-排序]({{< ref "posts/hql-窗口函数-排序/index.md" >}})
* [窗口函数的“窗口”]({{< ref "posts/hql-窗口函数-窗口/index.md" >}})
* [行转列、列转行]({{< ref "posts/hql-行列转化/index.md" >}})



类似PSI的计算思路，计算IV的整体思路依然参照公式，（等频）分箱后，基于数据的断点（Breakpoint Value）统计出每个箱子的好坏人数




### [1. 列转行]({{< ref "posts/hql-行列转化/index.md" >}})

将`score`表进行列转行，变为`key-value` 键值对的形式

```sql
drop table if exists score_value;
create table score_value as
select  no
        ,ym
        ,y
        ,score
        ,score_value
from(
    select  no
            ,ym
            ,x1
            ,x2
            ,x3
            ,x4
            ,y
    from    score
    )
lateral view outer explode(map('x1',x1,'x2',x2,'x3',x3,'x4',x4)) t as score,score_value
;
```

### 2. 分箱统计好坏人数

这里采用的是 __等频分箱__

```sql
drop table if exists score_group_nums;
create table score_group_nums as
select  ym
        ,score
        ,score_group
        ,group_bad_i
        ,group_good_i
        ,sum(group_bad_i) over(partition by ym,score) as group_bad_total
        ,sum(group_good_i) over(partition by ym,score) as group_good_total
from(
    select  ym
            ,score
            ,score_group
            ,count(case when y=1 then no end) as group_bad_i
            ,count(case when y=0 then no end) as group_good_i
    from(
        select  a.*
                ,case when a.score_value is null or a.score_value in ('','null','NULL') then 'missing'
                    when a.score_value <= r.score_array[0] then 'r1'
                    when a.score_value <= r.score_array[1] then 'r2'
                    when a.score_value <= r.score_array[2] then 'r3'
                    when a.score_value <= r.score_array[3] then 'r4'
                    when a.score_value <= r.score_array[4] then 'r5'
                    when a.score_value <= r.score_array[5] then 'r6'
                    when a.score_value <= r.score_array[6] then 'r7'
                    when a.score_value <= r.score_array[7] then 'r8'
                    when a.score_value <= r.score_array[8] then 'r9'
                    when a.score_value <= r.score_array[9] then 'r10' end as score_group
        from    score_value a
        left join
            (-- 等频分箱 10 bins
            select  ym
                    ,score
                    ,percentile_approx(score_value,array(0.1,0.2,0.3,0.4,0.5,0.6,0.7,0.8,0.9,1),999999) as score_array
            from    score_value
            group by 1,2
            ) r
        on  (a.ym = r.ym
            and a.score = r.score)
        )
    group by 1,2,3
    )
;
```

### 3. 计算IV

回顾下公式

$$IV=\sum_{i=1}^{n}(\frac{Bad_i}{Bad_T} - \frac{Good_i}{Good_T}) \times WOE_i$$

其中，

$$WOE_i=\ln(\frac{Bad_i}{Bad_T}) - \ln(\frac{Good_i}{Good_T})$$

```sql
select  ym
        ,score
        ,sum(iv_i) as iv
from(
    select  ym
            ,score
            ,score_group
            ,(ln(group_bad_i/group_bad_total)-ln(group_good_i/group_good_total))*(group_bad_i/group_bad_total - group_good_i/group_good_total) as iv_i
    from    score_group_nums
)
group by 1,2
```

## 总结

在非建模场景，只想大概看下（或监控）各变量的预测能力时，为省去导出数据用`Python`计算`IV`的麻烦，本文便以`IV`的计算公式出发详细记录`SQL`计算过程







<head> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/all.js"></script> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/v4-shims.js"></script> 
</head> 
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.13/css/all.css">
