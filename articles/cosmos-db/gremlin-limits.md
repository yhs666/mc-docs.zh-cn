---
title: Azure Cosmos DB Gremlin 的限制
description: 有关 Graph 引擎运行时限制的参考文档
author: rockboyfor
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: reference
origin.date: 09/10/2019
ms.date: 09/30/2019
ms.author: v-yeche
ms.openlocfilehash: 8b20399a1af88ce5f3505e585f7697fd4345586c
ms.sourcegitcommit: 0d07175c0b83219a3dbae4d413f8e012b6e604ed
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/26/2019
ms.locfileid: "71306848"
---
# <a name="azure-cosmos-db-gremlin-limits"></a>Azure Cosmos DB Gremlin 限制
本文介绍 Azure Cosmos DB Gremlin 引擎的限制，并说明他们可能会如何影响客户遍历。

Cosmos DB Gremlin 基于 Cosmos DB 基础结构构建，因此，[Azure Cosmos DB](/cosmos-db/concepts-limits) 中的所有限制仍适用。 

## <a name="limits"></a>限制

达到 Gremlin 限制时，遍历将会取消，并会显示 **x-ms-status-code** = 429 以指示存在限制错误。

**资源**    | **默认限制** | **解释**
--- | --- | ---
每个请求的内存  | **2 GB** | 请求在处理过程中可以占用的最大内存。 需要计算大数据集的请求会消耗额外的内存。 请考虑将请求划分为较小的数据集，以避免超出此限制或使用 OLAP 解决方案。
脚本长度  | **64 KB** | 每个请求的 Gremlin 遍历脚本的最大长度。
运算符深度  | **400** |  遍历中的唯一步骤的总数。 例如，```g.V().out()``` 的运算符计数为 2：V() 和 out()，```g.V('label').repeat(out()).times(100)``` 的运算符深度为 3：V()、repeat() 和 out()，因为 ```.times(100)``` 为 ```.repeat()``` 运算符的参数。
*并行度* | **32** | 发往存储层的单个请求中查询的存储分区的最大数。 具有数百个分区的图将受到此限制的影响。
重复限制  | **32** | ```.repeat()``` 运算符可执行的最大迭代次数。 在大多数情况下，```.repeat()``` 步骤的每次迭代都会运行广度优先的遍历，这意味着任何遍历都限于顶点之间最多 32 个跃点。
遍历超时  | **30 秒** | 超过此时间时，遍历将取消。 Cosmos DB Graph 是一种 OLTP 数据库，其中绝大部分遍历都在几毫秒内完成。 若要对 Cosmos DB Graph 运行 OLAP 查询，请使用 [Apache Spark](https://www.azure.cn/home/features/cosmos-db/)（带 [Graph Data Frames](https://spark.apache.org/docs/latest/sql-programming-guide.html#datasets-and-dataframes)）和 [Cosmos DB Spark 连接器](https://github.com/Azure/azure-cosmosdb-spark)。
谓词限制  | **20** | 应用于单个顶点或边的 ```.has()``` 或 ```.hasNot()``` 步骤的计数。 当达到此限制时，显示给应用程序的错误为 ```The SQL query exceeded the maximum number of joins. The allowed limit is 20```。 这只是暂时的不便，因为团队正在努力提升此限制。 
空闲连接超时  | **5小时** | 一段时间，Graph 服务器会在这段时间内保持 Websocket 连接处于打开状态，但其中没有流量。 TCP keep-alive 数据包或 HTTP keep-alive 请求的连接持续时间不会超出此限制，但如果它们没有发送出去，则底层的 Azure 基础结构甚至会提前关闭该连接。 如果没有 Gremlin 遍历在 Cosmos DB Graph 引擎上运行，系统会将该引擎视为空闲。
每小时的资源令牌  | **100** | Gremlin 客户端在连接到某个区域中的 Gremlin 帐户时使用的唯一资源令牌的数目。 当应用程序超出每小时唯一令牌限制时，系统会针对下一次的身份验证请求返回 `"Exceeded allowed resource token limit of 100 that can be used concurrently"`。

## <a name="next-steps"></a>后续步骤
* [Azure Cosmos DB Gremlin 响应标头](gremlin-headers.md) 
* [使用 Gremlin 的 Azure Cosmos DB 资源令牌](how-to-use-resource-tokens-gremlin.md)

<!-- Update_Description: new article about gremlin limits -->
<!--ms.date: 09/30/2019-->