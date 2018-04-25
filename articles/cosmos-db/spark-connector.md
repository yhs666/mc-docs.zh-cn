---
title: 将 Apache Spark 连接到 Azure Cosmos DB | Azure
description: 使用本教程来了解 Azure Cosmos DB Spark 连接器：使用该连接器可将 Apache Spark 连接到 Azure Cosmos DB，以便在 Microsoft 提供的针对云设计的多租户多区域分布式数据库系统中执行分布式聚合和数据科学。
keywords: apache spark
services: cosmos-db
documentationcenter: ''
author: rockboyfor
manager: digimobile
ms.assetid: c4f46007-2606-4273-ab16-29d0e15c0736
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 01/08/2018
ms.date: 04/23/2018
ms.author: v-yeche
ms.openlocfilehash: f5975add36a2fd9d5872196b95c7a1100ba17cf4
ms.sourcegitcommit: c4437642dcdb90abe79a86ead4ce2010dc7a35b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2018
---
<!-- Notice in meta : 全球 to 多个区域 -->
# <a name="accelerate-real-time-big-data-analytics-with-the-spark-to-azure-cosmos-db-connector"></a>使用 Spark 到 Azure Cosmos DB 的连接器加速实时大数据分析

Spark 到 Azure Cosmos DB 的连接器能使 Azure Cosmos DB 充当 Apache Spark 作业的输入源或输出接收器。 将 [Spark](http://spark.apache.org/) 连接到 [Azure Cosmos DB](https://www.azure.cn/home/features/cosmos-db/) 后，可以使用 Azure Cosmos DB 快速保存和查询数据，更快地解决瞬息万变的数据科学问题。 Spark 到 Azure Cosmos DB 的连接器有效利用本机 Azure Cosmos DB 托管的索引。 针对快速变化的多区域分布的数据执行分析和向下推送谓词筛选时，这些索引可用于实现可更新的列（范围从物联网 (IoT) 到数据科学以及分析方案）。
<!-- Notice: 全球 to 多个区域 -->
<!-- Not Available  [Perform graph analytics using Spark and Apache TinkerPop Gremlin](spark-connector-graph.md) -->
## <a name="download"></a>下载

若要开始使用，请从 GitHub 上的 [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark/) 存储库中下载 Spark 到 Azure Cosmos DB 连接器。

## <a name="connector-components"></a>连接器组件

连接器利用以下组件：

* 使用 [Azure Cosmos DB](http://documentdb.com)，客户可跨任意数量的地理区域弹性缩放吞吐量与存储。 该服务提供：
   * 统包式[多区域分布](distribute-data-globally.md)和水平缩放
<!-- Notice: 全球 to 多个区域 -->
   * 准确率达 99% 的有保证单一数位延迟
   * [多个妥善定义的一致性模型](consistency-levels.md)
   * 具有多宿主功能的有保证高可用性
   * 所有功能都由行业领先的全面的[服务级别协议](https://www.azure.cn/support/sla/cosmos-db) (SLA) 提供支持。

* [Apache Spark](http://spark.apache.org/) 是一个强大开源处理引擎，专为速度、易用性和复杂分析而打造。

* 通过 [Apache Spark on Azure HDInsight](../hdinsight/spark/apache-spark-jupyter-spark-sql.md)，可以使用 [Azure HDInsight](https://www.azure.cn/home/features/hdinsight/) 在云中为任务关键型部署方案部署 Apache Spark。

官方支持的版本：

| 组件 | 版本 |
|---------|-------|
|Apache Spark|2.0.2, 2.1.0, 2.2.0|
| Scala| 2.10, 2.11|
| Azure Cosmos DB SQL Java SDK | 1.14.0, 1.15.0 |

本文将帮助你使用 Python（通过 pyDocumentDB）和 Scala 接口运行一些简单的示例。

可使用两种方法连接 Apache Spark 和 Azure Cosmos DB：
- 通过 [Azure Cosmos DB SQL Python SDK](https://github.com/Azure/azure-documentdb-python) 使用 pyDocumentDB。
- 利用 [Azure Cosmos DB SQL Java SDK](https://github.com/Azure/azure-documentdb-java) 创建基于 Java 的 Spark 到 Azure Cosmos DB 的连接器。

## <a name="pydocumentdb-implementation"></a>pyDocumentDB 实现
使用最新的 [pyDocumentDB SDK](https://github.com/Azure/azure-documentdb-python) 可将 Spark 连接到 Azure Cosmos DB，如下图所示：

![通过 pyDocumentDB DB 从 Spark 流到 Azure Cosmos DB 的数据流](./media/spark-connector/spark-pydocumentdb.png)

### <a name="data-flow-of-the-pydocumentdb-implementation"></a>pyDocumentDB 实现的数据流

数据流如下所示：

1. Spark 主节点通过 pyDocumentDB 连接到 Azure Cosmos DB 网关节点。 用户只需指定 Spark 和 Azure Cosmos DB 连接。 到各自的主节点和网关节点的连接对用户而言是透明的。
2. 网关节点向 Azure Cosmos DB 发出查询，并针对数据节点中的集合分区运行查询。 这些查询的响应发回到网关节点，结果集返回到 Spark 主节点。
3. 后续查询（例如，针对 Spark 数据框架执行的查询）将发送到 Spark 工作节点进行处理。

Spark 与 Azure Cosmos DB 之间的通信仅限于 Spark 主节点和 Azure Cosmos DB 网关节点。  查询速度与这两个节点之间的传输层允许的速度相同。

### <a name="install-pydocumentdb"></a>安装 pyDocumentDB
可以使用 **pip** 在驱动程序节点上安装 pyDocumentDB，例如：

```bash
pip install pyDocumentDB
```

### <a name="connect-spark-to-azure-cosmos-db-via-pydocumentdb"></a>通过 pyDocumentDB 将 Spark 连接到 Azure Cosmos DB
由于通信传输的简单性，使用 pyDocumentDB 执行从 Spark 到 Azure Cosmos DB 的查询相对简单。

以下代码片段演示了如何在 Spark 上下文中使用 pyDocumentDB。

```python
# Import Necessary Libraries
import pydocumentdb
from pydocumentdb import document_client
from pydocumentdb import documents
import datetime

# Configuring the connection policy (allowing for endpoint discovery)
connectionPolicy = documents.ConnectionPolicy()
connectionPolicy.EnableEndpointDiscovery
connectionPolicy.PreferredLocations = ["China North", "China East"]

# Set keys to connect to Azure Cosmos DB
masterKey = 'le1n99i1w5l7uvokJs3RT5ZAH8dc3ql7lx2CG0h0kK4lVWPkQnwpRLyAN0nwS1z4Cyd1lJgvGUfMWR3v8vkXKA=='
host = 'https://doctorwho.documents.azure.cn:443/'
client = document_client.DocumentClient(host, {'masterKey': masterKey}, connectionPolicy)
```

代码片段指明：

* Azure Cosmos DB Python SDK (`pyDocumentDB`) 包含所有必需的连接参数。 例如，首选的位置参数选择读取副本和优先级顺序。
*  导入所需的库，并配置 masterKey 和主机来创建 Azure Cosmos DB 客户端 (**pydocumentdb.document_client**)。

### <a name="execute-spark-queries-via-pydocumentdb"></a>通过 pyDocumentDB 执行 Spark 查询
以下示例使用前面代码片段中通过指定的只读键创建的 Azure Cosmos DB 实例。 以下代码片段在前面指定的 DoctorWho 帐户中连接到 **airports.codes** 集合，并运行一个查询来提取华盛顿州的机场城市信息。

```python
# Configure Database and Collections
databaseId = 'airports'
collectionId = 'codes'

# Configurations the Azure Cosmos DB client will use to connect to the database and collection
dbLink = 'dbs/' + databaseId
collLink = dbLink + '/colls/' + collectionId

# Set query parameter
querystr = "SELECT c.City FROM c WHERE c.State='WA'"

# Query documents
query = client.QueryDocuments(collLink, querystr, options=None, partition_key=None)

# Query for partitioned collections
# query = client.QueryDocuments(collLink, query, options= { 'enableCrossPartitionQuery': True }, partition_key=None)

# Push into list `elements`
elements = list(query)
```

通过 **query** 执行查询后，返回已转换为 Python 列表的 **query_iterable.QueryIterable** 结果。 可以使用以下代码轻松将 Python 列表转换为 Spark 数据框架：

```python
# Create `df` Spark DataFrame from `elements` Python list
df = spark.createDataFrame(elements)
```

### <a name="why-use-the-pydocumentdb-to-connect-spark-to-azure-cosmos-db"></a>为何使用 pyDocumentDB 将 Spark 连接到 Azure Cosmos DB？
在以下场合，通常使用 pyDocumentDB 将 Spark 连接到 Azure Cosmos DB：

* 你希望使用 Python。
* 要将 Azure Cosmos DB 中相对较小的结果集返回到 Spark。 请注意，Azure Cosmos DB 中的基础数据集可能很大。 要对 Azure Cosmos DB 源应用筛选器，即运行谓词筛选器。  

## <a name="spark-to-azure-cosmos-db-connector"></a>Spark 到 Azure Cosmos DB 的连接器

Spark 到 Azure Cosmos DB 的连接器使用 [Azure Cosmos DB SQL Java SDK](https://github.com/Azure/azure-documentdb-java)，并在 Spark 工作节点与 Azure Cosmos DB 之间移动数据，如下图所示：

![Spark 到 Azure Cosmos DB 的连接器中的数据流](./media/spark-connector/spark-connector.png)

数据流如下所示：

1. Spark 主节点连接到 Azure Cosmos DB 网关节点，以获取分区映射。 用户只需指定 Spark 和 Azure Cosmos DB 连接。 到各自的主节点和网关节点的连接对用户而言是透明的。
2. 此信息返回给 Spark 主节点。  此时，应该能够分析查询，确定需要访问 Azure Cosmos DB 中的哪些分区及其位置。
3. 此信息将传送到 Spark 辅助角色节点。
4. Spark 工作节点直接连接到 Azure Cosmos DB 分区以提取数据，并将数据返回到 Spark 工作节点中的 Spark 分区。

Spark 与 Azure Cosmos DB 之间的通信速度会大幅提高，因为数据在 Spark 工作节点与 Azure Cosmos DB 数据节点（分区）之间移动。

### <a name="build-the-spark-to-azure-cosmos-db-connector"></a>构建 Spark 到 Azure Cosmos DB 的连接器
目前，连接器项目使用 maven。 若要构建不带依赖项的连接器，可以运行：
```
mvn clean package
```
也可以从 [*releases* 文件夹](https://github.com/Azure/azure-cosmosdb-spark/releases) 中下载最新版本的 JAR。

### <a name="include-the-azure-cosmos-db-spark-jar"></a>包含 Azure Cosmos DB Spark JAR
在执行任何代码之前，都需要包含 Azure Cosmos DB Spark JAR。  如果使用 **spark-shell**，则可以使用 **-jar** 选项包含 JAR。  

```bash
spark-shell --master $master --jars /$location/azure-cosmosdb-spark_2.1.0_2.11-1.0.0-uber.jar
```

如果想要执行不带依赖项的 JAR，请使用以下代码：

```bash
spark-shell --master $master --jars /$location/azure-cosmosdb-spark_2.1.0_2.11-1.0.0.jar,/$location/azure-documentdb-1.14.0.jar,/$location/azure-documentdb-rx-0.9.0-rc2.jar,/$location/json-20140107.jar,/$location/rxjava-1.3.0.jar,/$location/rxnetty-0.4.20.jar 
```

如果正在使用笔记本服务（例如 Azure HDInsight Jupyter 笔记本服务），可以使用 **spark magic** 命令：

```
%%configure
{ "name":"Spark-to-Cosmos_DB_Connector", 
  "jars": ["wasb:///example/jars/1.0.0/azure-cosmosdb-spark_2.1.0_2.11-1.0.0.jar", "wasb:///example/jars/1.0.0/azure-documentdb-1.14.0.jar", "wasb:///example/jars/1.0.0/azure-documentdb-rx-0.9.0-rc2.jar", "wasb:///example/jars/1.0.0/json-20140107.jar", "wasb:///example/jars/1.0.0/rxjava-1.3.0.jar", "wasb:///example/jars/1.0.0/rxnetty-0.4.20.jar"],
  "conf": {
    "spark.jars.excludes": "org.scala-lang:scala-reflect"
   }
}
```

使用 **jars** 命令可以包含 **azure-cosmosdb-spark** 所需的两个 JAR（该 JAR 本身以及 Azure Cosmos DB SQL Java SDK）并排除 **scala-reflect**，使其不会干扰 Livy 调用（Jupyter 笔记本 > Livy > Spark）。

### <a name="connect-spark-to-azure-cosmos-db-using-the-connector"></a>使用连接器将 Spark 连接到 Azure Cosmos DB
尽管通信传输有点复杂，但使用连接器执行从 Spark 到 Azure Cosmos DB 的查询要快得多。

以下代码片段演示如何在 Spark 会话中使用连接器；有关 Python 示例，请参阅`azure-cosmosdb-spark` [GitHub 存储库](https://github.com/Azure/azure-cosmosdb-spark)。

```
// Import Necessary Libraries
import org.joda.time._
import org.joda.time.format._
import com.microsoft.azure.cosmosdb.spark.schema._
import com.microsoft.azure.cosmosdb.spark._
import com.microsoft.azure.cosmosdb.spark.config.Config

// Configure connection to your collection
val baseConfig = Config(Map("Endpoint" -> "https://doctorwho.documents.azure.cn:443/",
"Masterkey" -> "le1n99i1w5l7uvokJs3RT5ZAH8dc3ql7lx2CG0h0kK4lVWPkQnwpRLyAN0nwS1z4Cyd1lJgvGUfMWR3v8vkXKA==",
"Database" -> "DepartureDelays",
"preferredRegions" -> "China North;China East;",
"Collection" -> "flights_pcoll",
"SamplingRatio" -> "1.0"))

// Create collection connection
val coll = spark.sqlContext.read.cosmosDB(baseConfig)
coll.createOrReplaceTempView("c")
```

代码片段指明：

- **azure-cosmosdb-spark** 包含所有必需的连接参数，包括首选位置。 例如，可以选择读取副本和优先级顺序。
- 只需导入所需的库，并配置 masterKey 和主机来创建 Azure Cosmos DB 客户端。

### <a name="execute-spark-queries-via-the-connector"></a>通过连接器执行 Spark 查询

以下示例使用前面代码片段中通过指定的只读键创建的 Azure Cosmos DB 实例。 以下代码片段连接到 DepartureDelays.flights_pcoll 集合（前面指定的 DoctorWho 帐户），并运行一个查询来提取从西雅图出发的航班的航班延迟信息。

```
// Queries
var query = "SELECT c.date, c.delay, c.distance, c.origin, c.destination FROM c WHERE c.origin = 'SEA'"
val df = spark.sql(query)

// Run DF query (count)
df.count()

// Run DF query (show)
df.show()
```

### <a name="why-use-the-spark-to-azure-cosmos-db-connector-implementation"></a>为何使用 Spark 到 Azure Cosmos DB 的连接器实现？

在以下场合，通常使用连接器将 Spark 连接到 Azure Cosmos DB：

* 要使用 Python 和/或 Scala。
* 要在 Apache Spark 与 Azure Cosmos DB 之间传输大量的数据。

若要大致了解查询性能的差异，请参阅[查询测试运行 wiki 文章](https://github.com/Azure/azure-cosmosdb-spark/wiki/Query-Test-Runs)。

## <a name="distributed-aggregation-example"></a>分布式聚合示例
本部分举例说明了如何结合使用 Apache Spark 和 Azure Cosmos DB 来执行分布式聚合与分析。 Azure Cosmos DB 已经支持聚合，[Multiple-region scale aggregates with Azure Cosmos DB](https://azure.microsoft.com/blog/planet-scale-aggregates-with-azure-documentdb/)（使用 Azure Cosmos DB 在多区域范围内聚合）博客中对此进行了讨论。 下面是可以通过 Apache Spark 将其提升到下一级别的方式。
<!-- Notice: 全球 to 多个区域 -->
<!-- Notice: Planet to Multiple-region -->

请注意，这些聚合与 [Spark 到 Azure Cosmos DB 的连接器 Notebook](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/notebooks/Spark-to-CosmosDB_Connector.ipynb) 相关。

### <a name="connect-to-flights-sample-data"></a>连接到航班示例数据
这些聚合示例访问 **DoctorWho** Azure Cosmos DB 数据库中存储的一些飞行性能数据。 若要连接到这些数据，需要利用以下代码片段：

```
// Import Spark to Azure Cosmos DB connector
import com.microsoft.azure.cosmosdb.spark.schema._
import com.microsoft.azure.cosmosdb.spark._
import com.microsoft.azure.cosmosdb.spark.config.Config

// Connect to Azure Cosmos DB Database
val readConfig2 = Config(Map("Endpoint" -> "https://doctorwho.documents.azure.cn:443/",
"Masterkey" -> "le1n99i1w5l7uvokJs3RT5ZAH8dc3ql7lx2CG0h0kK4lVWPkQnwpRLyAN0nwS1z4Cyd1lJgvGUfMWR3v8vkXKA==",
"Database" -> "DepartureDelays",
"preferredRegions" -> "China North;China East 2;",
"Collection" -> "flights_pcoll",
"SamplingRatio" -> "1.0"))

// Create collection connection
val coll = spark.sqlContext.read.cosmosDB(readConfig2)
coll.createOrReplaceTempView("c")
```

我们还要使用此代码片段运行一个基本查询，以便将 Azure Cosmos DB 中的筛选数据集传输到 Spark（后者可以执行分布式聚合）。 在本例中，我们要查询从西雅图 (SEA) 出发的航班。

```
// Run, get row count, and time query
val originSEA = spark.sql("SELECT c.date, c.delay, c.distance, c.origin, c.destination FROM c WHERE c.origin = 'SEA'")
originSEA.createOrReplaceTempView("originSEA")
```

从 Jupyter 笔记本服务运行查询后，生成了以下结果。  请注意，所有代码片段都是泛用性的，并不特定于任一服务。

### <a name="running-limit-and-count-queries"></a>运行 LIMIT 和 COUNT 查询
就像我们在 SQL/Spark SQL 中经常所做的那样，让我们从 **LIMIT** 查询开始：

![Spark LIMIT 查询](./media/spark-connector/spark-sql-query.png)

下一个查询是简单快速的 **COUNT** 查询：

![Spark COUNT 查询](./media/spark-connector/spark-count-query.png)

### <a name="group-by-query"></a>GROUP BY 查询
接下来，我们可以对 Azure Cosmos DB 数据库轻松运行 **GROUP BY** 查询：

```
select destination, sum(delay) as TotalDelays
from originSEA
group by destination
order by sum(delay) desc limit 10
```

![Spark  GROUP BY 查询图表](./media/spark-connector/group-by-query-graph.png)

### <a name="distinct-order-by-query"></a>DISTINCT, ORDER BY 查询
下面是 **DISTINCT, ORDER BY** 查询：

![Spark  GROUP BY 查询图表](./media/spark-connector/order-by-query.png)

### <a name="continue-the-flight-data-analysis"></a>继续分析航班数据
可以使用以下示例查询继续分析航班数据：

#### <a name="top-5-delayed-destinations-cities-departing-from-seattle"></a>从西雅图出发的航班延迟次数最多的 5 个目的地（城市）
```
select destination, sum(delay)
from originSEA
where delay < 0
group by destination
order by sum(delay) limit 5
```
![Spark 延迟次数最多的目的地图表](./media/spark-connector/top-delays-graph.png)

#### <a name="calculate-median-delays-by-destination-cities-departing-from-seattle"></a>根据从西雅图出发的航班的目的地城市计算平均延迟时间
```
select destination, percentile_approx(delay, 0.5) as median_delay
from originSEA
where delay < 0
group by destination
order by percentile_approx(delay, 0.5)
```

![Spark 平均延迟时间图表](./media/spark-connector/median-delays-graph.png)

## <a name="next-steps"></a>后续步骤

从 [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark) GitHub 存储库下载 Spark 到 Azure Cosmos DB 的连接器（如果尚未下载），并浏览该存储库中的其他资源：

* [分布式聚合示例](https://github.com/Azure/azure-cosmosdb-spark/wiki/Aggregations-Examples)
* [示例脚本和笔记本](https://github.com/Azure/azure-cosmosdb-spark/tree/master/samples)

此外，还可以查看文章 [Apache Spark SQL、数据框架和数据集指南](http://spark.apache.org/docs/latest/sql-programming-guide.html)以及 [Azure HDInsight 上的 Apache Spark](../hdinsight/spark/apache-spark-jupyter-spark-sql.md)。

<!--Update_Description: update meta properties, wording update, update link -->