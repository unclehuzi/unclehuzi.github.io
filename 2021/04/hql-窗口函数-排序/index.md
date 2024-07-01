# 窗口函数-排序


整理排序场景常用函数，`row_number() over()`, `rank() over()`, `dense_rank() over()`, `ntile(n) over()`，并以连续登录问题为例深化理解
<!--more-->

## row_number() over()

### 实现的功能

从1开始依次排序，生成**不会重复**的编号

```sql
-- 按照nums 列，降序排序

select  id
        ,nums
        ,row_number() over(order by nums desc) as rank
from    table
```

id | nums | rank 
---|--- | ---
1x | 45 | 3
2x | 78 | 2
3x | 87 | 1
4x | 32 | 4

### 语法

```sql
row_number() over([partition by expr1, expr2, ...] 
                  order by expr1 [asc|desc], expr2 [asc|desc], ... )
```

`partition by`表示基于某（些）维度（/列）分组之后，再基于`order by`的规则实现组内排序。

```
select  id
        ,nums
        ,row_number() over(partition by id order by nums desc) as rank
from    table
```

### 应用


如何确定连续登录天数超过3天的用户



找到连续3天登录用户所表现的**数据特征**。比如，按照登录日期排序得到编号，两者作差，若连续登录则作差后的值是一样的



基于这个现象，可用`row_number`实现

```sql
select  user_id
        ,(session_date - rk) as diff
        ,count(1) as nums
from(
    select  *
            ,row_number() over(partition by user_id order by session_date) as rk
    from(
        -- 按天去重
        select  user_id
                ,date_format(session_time,'yyyyMMdd') as session_date
        from    table1
        group by 1,2
        )
    )
group by 1,2
having  nums >= 3
;
```

## rank() over()

基本语法类似于`row_number()`

```sql
rank() over([partition by expr1, expr2, ...] 
             order by expr1 [asc|desc], expr2 [asc|desc], ... )
```

**但不同的是**，当值相等时 `rank()` 排序会出现重复序号的情况，且下个序号和当前序号之差为当前相同值的个数

### 示例

```sql
select  dealer_id
        ,emp_name
        ,sales
        ,rank() over(order by sales) as rk
from    q1_sales
;
```

| dealer_id  |    emp_name     | sales  | rank  |
|------------|-----------------|--------|-------|
| 1          | Raphael Hull    | 8227   | 1     |
| 3          | May Stout       | 9308   | 2     |
| 2          | Haviva Montoya  | 9308   | 2     |
| 1          | Jack Salazar    | 9710   | 4     |
| 3          | Abel Kim        | 12369  | 5     |
| 3          | Ursa George     | 15427  | 6     |
| 2          | Beverly Lang    | 16233  | 7     |
| 2          | Kameko French   | 16233  | 7     |
| 1          | Ferris Brown    | 19745  | 9     |
| 1          | Noel Meyer      | 19745  | 9     |



## dense_rank() over()

`row_number() over()` 、 `rank() over()` 和 `dense_rank() over()` 之间的差别主要在于对相同值的序号处理方式不同。

和`rank() over()`一样，遇到相同值时序号会重复，但是`dense_rank() over()` 的下一个序号和当前序号之差依然是1，不会出现空位的情况。

### 示例

```sql
select  dealer_id
        ,emp_name
        ,sales
        ,dense_rank() over(order by sales) as denserank
from    q1_sales
;
```


| dealer_id  |    emp_name     | sales  | denserank  |
|------------|-----------------|--------|------------|
| 1          | Raphael Hull    | 8227   | 1          |
| 3          | May Stout       | 9308   | 2          |
| 2          | Haviva Montoya  | 9308   | 2          |
| 1          | Jack Salazar    | 9710   | 3          |
| 3          | Abel Kim        | 12369  | 4          |
| 3          | Ursa George     | 15427  | 5          |
| 2          | Beverly Lang    | 16233  | 6          |
| 2          | Kameko French   | 16233  | 6          |
| 1          | Ferris Brown    | 19745  | 7          |
| 1          | Noel Meyer      | 19745  | 7          |


## ntile(n) over()

`ntile(n) over()` 和之前那三个排序函数不太一样。形式来看，多了个参数`n`，是指按照顺序平均分成**n**份（/箱），返回当前所在的位置。且需要`order by` 语句。

但对于不能实现平均分的情况，会基于[约定](https://www.cnblogs.com/52xf/p/4209211.html "ntile的约定")来操作：



1. 每箱记录数不能大于上一个箱子的记录数。即第1组的记录数大于等于第2组的记录数。
2. 所有箱子的记录数要么相同。要么从某一记录数较少的箱子（命名为X）开始，后面所有箱子内的记录数都与该箱（X）的记录数相同。即如果前3箱的记录数都是9，而第4箱的记录数是8，那么第5、6箱及其之后箱子内的记录数也必须是8。


最先分出来的箱子，采取向上取整（`ceil()`）的方式




比如，53条记录，基于`ntile`的约定分到5个箱子，则每个箱子的记录数如下所示

|     bucket     | nums  |
|-----------------|--------|
| 1   | 11   |
| 2  | 11   |
| 3     | 11   |
| 4  | 10  |
| 5   | 10  |



`ntile`的方法能较好实现等频的效果，相比分位数作为分割点而言，不易受数据分布的影响。




### 示例

```sql
select  emp_mgr
        ,sales
        ,ntile(5) over(order by sales) as ntilerank
from    q1_sales
;
```

|     emp_mgr     | sales  | ntilerank  |
|-----------------|--------|------------|
| Kari Phelps     | 8227   | 1          |
| Rich Hernandez  | 9308   | 1          |
| Kari Phelps     | 9710   | 2          |
| Rich Hernandez  | 12369  | 2          |
| Mike Palomino   | 13181  | 3          |
| Rich Hernandez  | 15427  | 3          |
| Kari Phelps     | 15547  | 4          |
| Mike Palomino   | 16233  | 4          |
| Dan Brodi       | 19745  | 5          |
| Mike Palomino   | 23176  | 5          |


## 总结

|窗口函数|返回类型|描述|
| ---- | ---- | ---- |
|`row_number()`|int|从1开始依次排序，生成不会重复的序号|
|`rank()` |int|从1开始依次排序。若值相等则得到同样的序号；且下一个序号将会出现空位，即若2个相等的值序号是1，则下一个序号是3|
|`dense_rank()`|int|从1开始依次排序。若值相等则得到同样的序号；但下一个序号**不会**出现空位，即若2个相等的值序号是1，则下一个序号依然是2|
|`ntile(n)`|int|将分组数据按照顺序平均分成n箱，返回当前值所在位置，n-th|


<head> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/all.js"></script> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/v4-shims.js"></script> 
</head> 
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.13/css/all.css">
