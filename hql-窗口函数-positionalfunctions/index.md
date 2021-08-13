# 窗口函数-Positional Functions



以 “计算当前和上一次事件的时间间隔” 引入 positional function
<!--more-->

截止到目前，窗口函数整理了聚合、排序场景，解决了“组内占比”、“定位连续3天登录用户”等问题

在平时的分析工作中，还有个比较常见的问题：计算当前和上一次事件的时间间隔。比如，相邻两次外呼的时间间隔

这个时候，`lead()` 或 `lag()` 函数可较为方便的解决该类问题


## 实现的功能

`lead()`, `lag()` 实现的功能比较类似。

### `lead()`  上移

```sql
select  seller_name
        ,sale_value
        ,lead(sale_value) over(order by sale_value) as next_sale_value
from    sale
;
```

![lead](https://learnsql.com/blog/lead-and-lag-functions-in-sql/2.png)

### `lag()` 下移

```sql
select  seller_name
        ,sale_value
        ,lag(sale_value) over(order by sale_value) as previous_sale_value
from    sale
;
```

![lag](https://learnsql.com/blog/lead-and-lag-functions-in-sql/1.png)

## 基本语法

```sql
lag(expression [,offset[,default_value]]) over([partition by expr1, expr2, ...] 
                                               order by expr1 [asc|desc], expr2 [asc|desc], ... )
                                               
lead(expression [,offset[,default_value]]) over([partition by expr1, expr2, ...] 
                                                order by expr1 [asc|desc], expr2 [asc|desc], ... )
```

`lead()`, `lag()` 中的3个参数：

* `expression` - string

    被操作的列名

* `offset` - int

    移动的行数（/偏移量）
    
* `default_value`

    定义为空的情况赋给的默认值
    

其中，参数 `expression` 是必须的。而 `default_value`（默认是 `NULL`） 是只有当 `offset`（默认是 `1`） 有值时才能使用

`over()` 语句中，`order by` 是必须要有的

## 应用

***计算用户相邻两次登录的天数间隔***


```sql
-- 如果只有1天有登录信息，则 diff_days 为 null
select  *
        ,datediff(session_date,lag_session_date) as diff_days
from(-- 下移
    select  *
            ,lag(session_date) over(partition by user_id order by session_date asc) as lag_session_date
    from(
        -- 按天去重
        select  user_id
                ,date_format(session_time,'yyyyMMdd') as session_date
        from    table1
        group by 1,2
        )
    )
;
```

窗口函数还有俩常见的：`first_value()`, `last_value()`，在此就略过了。

有时候可以用 `row_number() over()` 结合 `having` 一起使用，如 *确定用户最后一次登录时间*

```sql
select  user_id
        ,row_number() over(order by session_date desc) as rk
from    table1
having  rk = 1
;
```





<head> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/all.js"></script> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/v4-shims.js"></script> 
</head> 
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.13/css/all.css">
