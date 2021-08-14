# Hive SQL-条件函数汇总


已经2021年了，SQL条件函数已不局限于`case when` 了，针对常见场景已新增许多 ***conditional functions***

<!--more-->



汇总 <u>Hive SQL</u> 常用的条件函数，👇

![hive 条件函数](https://gitee.com/unclehu/pic/raw/master/img/hive条件函数.png)



## if(condition, value True, value FalseOrNull)

> This is the one of best Hive Conditional Function

类似Excel中的 `if` 函数

```sql
select	if(state = 'online','在线','离线') as state;
```



## case when then end

`case when` 较为常见，可注意执行**顺序**问题：从上至下依次执行，直到满足条件则跳出

常见写法有两种，主要取决于是否单变量作判断

```sql
select	case dayofweek when 1 then '星期一'
                       when 2 then '星期二' end as week
;
                                             
-- OR --

select	case when dayofweek = 1 then '星期一'
             when dayofweek = 2 then '星期二' end as week
;
```



## isnull(a)

判断是否为`null`（数据库特有的一种数据类型），留意字符型 `'null'`,  `'NULL'`

```sql
select	isnull('');

/*return
false
*/

```



## isnotnull(a)

非null 

```sql
select	isnotnull('');

/*return
true
*/
```



## nullif(a,b)

如果 `a=b` 则返回`null`，否则返回`a`

等同于 `case when a = b then null else a end`

```sql
select	nullif(1,1);

/*return
NULL
*/

select	nullif(4,3);

/*return
4
*/
```



## nvl(arg1,arg2)

可以理解为用`arg2`的值替换 `arg1` 中值为 `null` 的部分

```sql
select	nvl(null,999);

/*return
999
*/

select	nvl('null',999);

/*return
null
*/
```



## coalesce(value1,value2,...)

返回第一个非`null`的值

往往只想得到众多列中非 `null` 的值

```sql
select	coalesce(null,7,8);

/*return
7
*/
```



{{< admonition type=warning title="null" open=true >}}
涉及`null`的问题要留意数据库中的数据类型。譬如，某列数据格式为`string`，`'null'` 并不等同于 `null`
{{< /admonition >}}




<head> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/all.js"></script> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/v4-shims.js"></script> 
</head> 
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.13/css/all.css">

