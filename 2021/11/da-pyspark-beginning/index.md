# PySpark之应用层小白视角


Hi `PySpark`，初次见面，别来无恙

<!--more-->

> PySpark is an interface for Apache Spark in Python. It not only allows you to write Spark applications using Python APIs, but also provides the PySpark shell for interactively analyzing your data in a distributed environment.

首先，我是这么来看`PySpark`的：有一波人会`Python`但不会`Java`，那就搞个接口让会`Python`的小伙伴享受`Spark`分布式环境带来的快感，更好的分析大数据。

那么对于“面向问题编程”的从业人员来说`PySpark`的作用就很明显了。当觉得现有的分析工具很慢时可以考虑下`PySpark`，当然这里是基于`Spark`环境。换句话说，“快”是分布式环境带来的快感之一。

引入 `PySpark` 后，分析工作大致流程就变成了这样 👇

{{< mermaid >}}
graph LR;
    A(数据) -->|导入Spark环境| B(Spark-DataFrame)
    B --> C{toPandas}
    C -->|N| D(PySpark计算)
    C -->|Y| E(pandas计算)
    D --> F(输出-saveAS)
    E --> F(输出-saveAS)
{{< /mermaid >}}

其实，整体还是“箱子模型” 📦 ，“喂”数据 => 处理、计算模块 => 结果

{{< mermaid >}}
graph LR;
    A(Input) --> B(Box)
    B --> C(Output)
{{< /mermaid >}}

所以，应用层角度来看 `PySpark` 也就简单了：

1. 如何读取数据？
2. 如何处理、计算得到自己想要的结果，即“面向问题编程”
3. 如何处理结果？要保存到哪儿？

## 如何读取数据

这个往往取决于数据在哪儿，譬如有些数据是以csv格式保存，有些是在数据库...

总之都是为了 ***Loading data onto Spark RDDs***，享受分布式的快感

实际操作可以基于具体情况在网上检索相应的解决方案，如 `pyspark read hive table`

```python
# A example from https://spark.apache.org/docs/latest/sql-data-sources-hive-tables.html

from os.path import abspath
from pyspark.sql import SparkSession
from pyspark.sql import Row

# warehouse_location points to the default location for managed databases and tables
warehouse_location = abspath('spark-warehouse')

spark = SparkSession \
    .builder \
    .appName("Python Spark SQL Hive integration example") \
    .config("spark.sql.warehouse.dir", warehouse_location) \
    .enableHiveSupport() \
    .getOrCreate()

# spark is an existing SparkSession
spark.sql("CREATE TABLE IF NOT EXISTS src (key INT, value STRING) USING hive")
spark.sql("LOAD DATA LOCAL INPATH 'examples/src/main/resources/kv1.txt' INTO TABLE src")

# The results of SQL queries are themselves DataFrames and support all normal functions.
sqlDF = spark.sql("SELECT key, value FROM src WHERE key < 10 ORDER BY key")
```

## 数据处理、计算

读取数据得到 `Spark DataFrame` 后，可以直接对此进行操作，除了常见的业务分析还有机器学习模块（`MLlib`）

```python
raw_data = sc.textFile("./kddcup.data.gz")
## Comma-Separated Value
csv = raw_data.map(lambda x: x.split(","))

metrics = csv.map(lambda x: [x[0], x[4], x[5]])

from pyspark.mllib.stat import Statistics

Statistics.corr(metrics, method="spearman")
Statistics.corr(metrics, method="pearson")
```

值得一提的是，**Spark DataFrame to pandas DataFrame**

可以用 `toPandas()` 方法，同时参数方面设置 `spark.sql.execution.arrow.enabled=true` 能提高效率

```python
# A example from https://docs.microsoft.com/en-us/azure/databricks/spark/latest/spark-sql/spark-pandas

import numpy as np
import pandas as pd

# Enable Arrow-based columnar data transfers
spark.conf.set("spark.sql.execution.arrow.enabled", "true")

# Generate a pandas DataFrame
pdf = pd.DataFrame(np.random.rand(100, 3))

# Create a Spark DataFrame from a pandas DataFrame using Arrow
df = spark.createDataFrame(pdf)

# Convert the Spark DataFrame back to a pandas DataFrame using Arrow
result_pdf = df.select("*").toPandas()
```

这部分再Mark一个关于 `collect()` 的小点，总之数据量比较大的时候就不要用这个方法。

> The `collect()` function **returns a list** that contains all the elements in this RDD, and should only be used if the resulting array is expected to be ==small==, as *all the data is loaded in a driver's memory*, in which case we lose the benefits of distributing the data around a cluster of Spark instances.

## 如何处理结果

处理、计算后的结果往往会再一次的落库，这个时候同 “数据读取” 的部分，🉑️ 根据具体情况进行检索。

**以落到 hive 表为例**，截止到目前整理的，大致有两种方法。

首先确保数据为 Spark DataFrame 状态（可以通过 `spark.createDataFrame(df)` 的方法将 pandas DataFrame 转为 Spark DataFrame）


```python
spark_df.write.mode("overwrite").format("hive").saveAsTable("dbName.tableName")
# 注意是 overwrite 
```

或者

```python
spark_df.createOrReplaceTempView("myTempTableName")

spark.sql("drop table if exists dbName.tableName")
spark.sql("create table dbName.tableName as select * from myTempTableName")
```

## 总结

最近工作中遇到了 `PySpark` 的使用，在此从应用层小白视角通过 📦 “箱子模型”（Input => Box => Output） 简单记录大致的使用流程，方便于新手～


## Reference

1. 360数科深圳数据组
2. Rudy Lai and Bartłomiej Potaczek.《Hands On Big Data Analytics With PySpark》
3. https://docs.microsoft.com/en-us/azure/databricks/spark/latest/spark-sql/spark-pandas
4. http://spark.apache.org/docs/latest/api/python/index.html
5. https://spark.apache.org/docs/latest/sql-data-sources-hive-tables.html
6. https://stackoverflow.com/questions/30664008/how-to-save-dataframe-directly-to-hive




<head> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/all.js"></script> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/v4-shims.js"></script> 
</head> 
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.13/css/all.css">
