# Spark、Hive QL-通过正则表达式选取需要的列


【Mark】通过正则表达式选择相应的列

<!--more-->

在涉及子查询时，平时习惯直接把需要的列全写出来，也没想太多。

最近有小伙伴谈起正则选择需要的列，一顿操作后做个记录。建模取特征时，常见这种操作


Spark 和 Hive 都是支持这种操作的
只是相应的**设置不同**


|spark/hive|set|
|:--------:|---|
|hive[^1]|set hive.support.quoted.identifiers=none|
|spark[^2]|set spark.sql.parser.quotedRegexColumnNames=true|

其中，正则表达式的写法可参考 JAVA regex 语法

## 例1，查询去除某几列的所有列

`select` 去除 user_no, cust_no 的所有列

```sql
select  `(user_no|cust_no)?+.+`
from    table1
```

`select` 去除以 no 结尾的所有列

```sql
select  `(.*no)?+.+`
from    table1
```

## 例2，查询符合某特征的所有列

`select` 以 no 结尾的所有列

```sql
select  `.+no`
from    table1
```

## Reference

[^1]: https://community.cloudera.com/t5/Community-Articles/Spark-to-support-REGEX-column-specification-for-Hive-Queries/ta-p/316579
[^2]: https://stackoverflow.com/questions/52526768/does-spark-sql-supports-hive-select-all-query-with-except-columns-using-regex-sp


<head> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/all.js"></script> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/v4-shims.js"></script> 
</head> 
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.13/css/all.css">
