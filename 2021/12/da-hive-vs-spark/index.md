# Hive和Spark的区别


当下处理大规模数据集比较流行的两大产品：Hive和Spark。本文从历史等维度对两者进行比较
<!--more-->

🔗[原文链接](https://logz.io/blog/hive-vs-spark/)

## Hive

Hive 是一个运行在 Hadoop 分布式文件系统上的开源分布式数据仓库。Hive为查询和分析大规模（以表格形式存储的）结构化数据集而生。人们可以通过Hive内置的SQL引擎——HiveQL操作数据。将HiveSQL解析为Map-Reduce的操作。

### 简短的历史

Hive最初是由 Facebook 开发的，当时Facebook的开发人员发现他们的数据在几天内从 GB级别 呈指数增长到 TB级别。当时，Facebook 使用 Python 将他们的数据加载到关系型数据库。性能和可扩展性很快成为棘手的问题，因为关系型数据库只能纵向扩展。他们需要一个可以横向扩展并处理大量数据的数据库。那时Hadoop已经很流行了；不久之后，构建在 Hadoop 之上的 Hive 便出现了。Hive和关系型数据库很类似，但又不完全是。

### 为什么选择Hive

选择 Hive 的核心原因是因为它是一个运行在 Hadoop 上的 SQL 接口，降低了MapReduce框架的复杂性。Hive 帮助企业在 HDFS 上执行大规模数据分析，使其成为一个可横向扩展的数据库。它的 SQL 接口——HiveQL，使具有关系型数据库背景的开发人员可以更轻松地构建和开发性能更快、可扩展的数据仓库。（简单来说是上手快，方便原有DBA完成数据建模）

### Hive的特性和功能

Hive 具有企业级特性和功能，可帮助组织构建高效、高端的数据仓库解决方案。具备以下特性：

- Hive 使用 Hadoop 作为其存储引擎，并且仅在 HDFS 上运行
- 它专为数据仓库操作而构建，但不适用于 OLTP
- HiveQL 是 SQL 引擎，可为数据仓库层级的操作构建复杂的 SQL 查询。Hive 可以与其他分布式数据库（如 HBase）和 NoSQL 数据库（如 Cassandra）集成。

#### Hive的架构

有一个 Hive 接口，并使用 HDFS 跨多个服务器存储数据以进行分布式数据处理。

![Hive Architecture](https://dytvr9ot2sszz.cloudfront.net/wp-content/uploads/2019/08/Hive.png)

#### Hive在数仓中的应用

Hive 是专门为操作数据仓库而构建的数据库，尤其是那些处理TB、PB级数据的数据库。正如上文所说，Hive支持横向扩展，并结合 Hadoop 的功能，使其成为一个快速、高效及高维扩展的数据库。它可以在数千个节点上运行，充分体现硬件的性能。这使得 Hive 成为具有高性能和可扩展性的高性价比产品。

#### Hive 的集成功能

因为 HiveQL 符合 [ANSI SQL标准](https://en.wikipedia.org/wiki/SQL#:~:text=SQL%20became%20a%20standard%20of%20the%20American%20National%20Standards%20Institute)，所以 HBase 和 Cassandra 等数据库也可以集成Hive。这些数据库对 SQL 的支持有限，但可以帮助应用程序对更大的数据集进行分析。Spark, Kafka和Flume等工具也可以集成Hive。

### Hive的不足

Hive 是一个以表格形式存储数据的数据仓库，所以他只能处理使用 SQL 查询读取和写入的结构化数据。并不适用于存储非结构化数据，也不适用于OLTP。


## Spark

Spark 是一个分布式大数据框架，可处理并分析 RDD 格式的大规模数据集。简而言之，Spark 不是数据库，而是一个框架，可以将Hive和HBase等数据库中的数据加载至Spark的RDD，以享受分布式带来的快感。

### Spark Streaming

`Spark Streaming`是Spark 的一个扩展，它可以从 Web 源流式传输实时数据以进行各种分析。尽管还有其他工具（例如 Kafka 和 Flume）可以执行此操作，但 Spark 在复杂的数据分析方面更胜一筹。Spark 有自己特有的 SQL 引擎，并且在与 Kafka 和 Flume 集成时也能很好的兼容。

### 简短的历史

Spark 是 MapReduce 的替代方案。Spark通过“in-memory”的形式完成大数据分析，不依赖磁盘空间及网络带宽。

### 为什么选择Spark

Spark 的核心优势在于它能够执行复杂的“in-memory”分析和 PB 级的流数据，效能优于MapReduce。Spark 可以从运行在 Hadoop 上的任何数据存储中提取数据，并在内存中并行执行复杂的分析。此功能减少了磁盘 I/O 和网络争用，使其速度提高了十倍甚至一百倍。此外，Spark 中的数据分析框架可以使用 Java、Scala、Python、R 甚至 SQL 构建。

### Hive的特性和功能

#### Spark的架构

四大主要部分：

- **Spark SQL**
- **Spark Streaming**
- **MLlib** (machine learning)
- **GraphX** (graph

![spark](https://dytvr9ot2sszz.cloudfront.net/wp-content/uploads/2019/08/img.jpg)

#### 高效分析

Spark 从 Hadoop 中提取数据并在内存中执行分析。数据以并行方式分块进入内存，然后将生成的数据集推送到目的地。数据集也可以驻留在内存中，直到它们被消耗。

#### Spark Streaming

Spark Streaming 是 Spark 的扩展，它可以实时流式传输来自高频使用的 Web 源的大量数据。由于其执行高级分析的能力，与其他数据流工具（如 Kafka 和 Flume）相比，Spark 便脱颖而出。

#### 多个API

Spark 支持不同的编程语言，如 Java、Python 和 Scala，这些语言在大数据和数据分析领域非常流行。人们可以使用这些语言中的任何一种编写数据分析框架。

#### 处理大规模数据集

Spark也是为了分析、处理大数据的，就像上文提到的MapReduce。也是为了更加高效的大数据分析工作

## Hive v.s. Spark

Hive 和 Spark 是在大数据领域为不同目的而构建的不同产品。 Hive 是分布式数据库，Spark 是数据分析框架。

|Hive|Spark|
|:--:|:---:|
|类似关系型数据库，存储表格形式的数据|数据分析框架，不是数据库|
|通过HiveQL 取数|支持 SQL、Python、Java、R和scala|
|基于Hadoop|本身不存储数据，可以将数据加载至Spark RDDs|

简单来说，Hive是数据仓库，而Spark是个让取数更快的框架。

