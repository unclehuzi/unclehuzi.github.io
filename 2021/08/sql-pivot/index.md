# Spark SQL-Pivot


类似Excel的数据透视表，分类聚合。也可以协助实现行转列，Pivoting “Wide” to “Long” Format

<!--more-->

在统计分析时总会遇到分类汇总的场景，类似Excel的数据透视表。SQL中按照 `case when` 或 `IF` 的写法往往会显得臃肿，较为方便的便是通过 `pivot` [^1]实现，但 Hive **不**支持😢

{{< admonition type=warning title="运行环境" open=true >}}
以下基于 ***spark-2.4.5U3*** 及以上版本
{{< /admonition >}}

## 基本语法

```sql
PIVOT ( { aggregate_expression [ AS aggregate_expression_alias ] } [ , ... ]
       FOR column_list IN ( expression_list ) )
```

> The `PIVOT` clause can be specified after the table name or subquery.

## 实际应用

假设有张存有各个地区、各个产品的月销量的表（sales_table），我们需要统计各个月份所有地区产品销量的加总，形如👇

![按月份分类汇总销量](https://raw.githubusercontent.com/unclehuzi/pic/master/img/image-20210810172231715.png)

```sql
select	month
				,'毛巾'
				,'肥皂'
from		sales_table
pivot
 		(
     sum(sales)
     for product in ('毛巾','肥皂')
    )
;
```

## Reference

[^1]: https://spark.apache.org/docs/3.1.2/sql-ref-syntax-qry-select-pivot.html



<head> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/all.js"></script> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/v4-shims.js"></script> 
</head> 
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.13/css/all.css">
