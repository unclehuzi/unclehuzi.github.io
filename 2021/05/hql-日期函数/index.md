# Spark, Hive QL-日期函数汇总


分场景汇总日期函数
<!--more-->

工作中总会遇到处理时间的问题，参考营销理论中基于利益细分的市场细分理论，我从使用场景的角度出发，将常用的日期函数分为四大类：

* 时间计算
* 时间提取
* 格式转换
* 当前时间

![日期函数](https://i.loli.net/2021/05/31/pjrM6VtP2EF9gHY.png)

{{< admonition type=tip title="Tip" open=true >}}
本文重点在于**整合**日期函数
{{< /admonition >}}


## 时间计算

这部分主要是计算时间差（`datediff(end_date,start_date)`, `months_between(date1,date2)`）、时间加减（`date_add()`,`date_sub()`,`add_months()`）等



## 时间提取

提取时间戳的年、季度、月、周、日、小时、分钟、秒

可以直接调用对应的函数，也可使用`extract(field from column_name)` 函数指定 `field`，其中`field` 支持day, dayofweek, hour, minute, month, quarter, second, week and  year.



## 格式转换

有时为了适应不同时间格式的需求，需要做个转换，如`yyyy-MM-dd` 或 `yyyy-MM-dd HH:mm:ss`的形式转为`yyyyMMdd` 等

常用：

- `to_date()`

  返回 `date` 形式的日期，即`yyyy-MM-dd`

- `date_format()`

  转为指定格式的时间，如 `date_format('2015-04-08','y')` => `'2015'`



## 当前时间

若需要时间戳格式，则用`current_timestamp`

若只需要精确到天，即`date`格式，则用`current_date`



## 总结

本文重点在于从使用场景的角度出发，整合了`hive`, `spark` 环境下常用的日期函数。最后再以表格的形式简单汇总

| 场景     | 函数                                                         | 返回值类型 | 描述                                                         | 示例                                                         |
| :------- | ------------------------------------------------------------ | ---------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 当前时间 | unix_timestamp()                                             | bigint     | 当前 Unix时间戳（e.g. 1622451519 ），但因查询优化问题推荐使用 `current_timestamp` |                                                              |
| 当前时间 | current_date                                                 | date       | 当前日期                                                     | 2021-05-31                                                   |
| 当前时间 | current_timestamp                                            | timestamp  | 当前时间戳                                                   | 2021-05-31 17:12:14.968                                      |
| -        | -                                                            | -          | -                                                            | -                                                            |
| 格式转换 | from_unixtime(bigint unixtime[, string format])              | string     | 数字转为格式形如 2021-05-31 17:12:14 的字符串                |                                                              |
| 格式转换 | to_date(string timestamp)                                    | date       | to_date(yyyy-MM-dd HH:mm:ss) => yyyy-MM-dd                   |                                                              |
| 格式转换 | date_format(date/timestamp/string ts, string fmt)            | string     | 得到指定格式的时间                                           | date_format('2015-04-08', 'y') => '2015'                     |
| 格式转换 | trunc(string date, string format)                            | string     | 得到被指定`format`截断的日期，`format`支持MONTH/MON/MM, YEAR/YYYY/YY | trunc('2015-03-17', 'MM') => 2015-03-01                      |
| -        | -                                                            | -          | -                                                            | -                                                            |
| 时间提取 | year(string date)                                            | int        | 年                                                           |                                                              |
| 时间提取 | quarter(date/timestamp/string)                               | int        | 季度                                                         |                                                              |
| 时间提取 | weekofyear(string date)                                      | int        | 该年的第几周                                                 |                                                              |
| 时间提取 | month(string date)                                           | int        | 月                                                           |                                                              |
| 时间提取 | day(string date) dayofmonth(date)                            | int        | 日                                                           |                                                              |
| 时间提取 | hour(string date)                                            | int        | 小时                                                         |                                                              |
| 时间提取 | minute(string date)                                          | int        | 分钟                                                         |                                                              |
| 时间提取 | second(string date)                                          | int        | 秒                                                           |                                                              |
| 时间提取 | extract(field FROM source)                                   | int        | field 支持day, dayofweek, hour, minute, month, quarter, second, week and  year. |                                                              |
| -        | -                                                            | -          | -                                                            | -                                                            |
| 时间计算 | datediff(string enddate, string startdate)                   | int        | 天数差                                                       | datediff('2009-03-01', '2009-02-27') => 2                    |
| 时间计算 | date_add(date/timestamp/string startdate,  tinyint/smallint/int days) | date       | 加（减）x天后的日期                                          | date_add('2008-12-31', 1) => 2009-01-01, date_add('2008-12-31', -1)  => 2008-12-30 |
| 时间计算 | date_sub(date/timestamp/string startdate,  tinyint/smallint/int days) | date       | 加（减）x天后的日期                                          | date_sub('2008-12-31', 1) => 2008-12-30, date_sub('2008-12-31', -1)  => 2009-01-01 |
| 时间计算 | add_months(string start_date, int num_months,  output_date_format) | string     | x月后。如果start_date是该月的最后一天 或者 x月后的天数不是“大月”则结果为x月后该月的最后一天 | add_months('2017-12-31 14:15:16', 2, 'YYYY-MM-dd HH:mm:ss') =>  '2018-02-28 14:15:16' |
| 时间计算 | last_day(string date)                                        | string     | 该月最后一天的日期                                           | last_day(2021-05-11) => '2021-05-31'                         |
| 时间计算 | next_day(string start_date, string day_of_week)              | string     | 返回大于 start_date 的日期中最近的一个周x                    | next_day('2021-05-31','Monday')                              |
| 时间计算 | months_between(date1, date2)                                 | double     | date1-date2 月数差                                           | months_between('1997-02-28 10:30:00', '1996-10-30') => 3.94959677 |
