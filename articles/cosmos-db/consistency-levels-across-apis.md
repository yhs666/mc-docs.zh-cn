---
title: 一致性级别和 Azure Cosmos DB API
description: 了解 Azure Cosmos DB 中 API 的一致性级别。
author: rockboyfor
ms.author: v-yeche
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 10/23/2018
ms.date: 03/04/2019
ms.reviewer: sngun
ms.openlocfilehash: 6ef2efa2a446040a1653dbc32568f113426653c1
ms.sourcegitcommit: b56dae931f7f590479bf1428b76187917c444bbd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2019
ms.locfileid: "56987940"
---
# <a name="consistency-levels-and-azure-cosmos-db-apis"></a>一致性级别和 Azure Cosmos DB API

SQL API 本机支持 Azure Cosmos DB 提供的五个一致性模型。 使用 Azure Cosmos DB 时，SQL API 是默认 API。 

Azure Cosmos DB 还为常用数据库提供对与线路协议兼容的 API 的本机支持。 数据库包括 MongoDB。 数据库既没有提供准确定义的一致性模型，也没有为一致性级别提供由 SLA 支持的保证。 它们通常仅提供 Azure Cosmos DB 提供的五个一致性模型的一个子集。 对于 SQL API，将使用 Azure Cosmos 帐户上配置的默认一致性级别。 

<!-- Not Available on Apache Cassandra, Gremlin, and Azure Tables-->
<!-- Not Available on Gremlin API and Table API-->

以下部分显示了 MongoDB 的 OSS 客户端驱动程序所请求的数据一致性与 Azure Cosmos DB 中的对应一致性级别之间的映射。

<!-- Not Available on Apache Cassandra-->

<a name="cassandra-mapping"></a>
<!-- Not Available on ## Mapping between Apache Cassandra and Azure Cosmos DB consistency levels-->
<a name="mongo-mapping"></a>
## <a name="mapping-between-mongodb-34-and-azure-cosmos-db-consistency-levels"></a>MongoDB 3.4 与 Azure Cosmos DB 一致性级别之间的映射

下表显示了 MongoDB 3.4 与 Azure Cosmos DB 中的默认一致性级别之间的“读取关注点”映射。 下表显示了多区域和单区域部署。

| **MongoDB 3.4** | **Azure Cosmos DB（多区域）** | **Azure Cosmos DB（单区域）** |
| - | - | - |
| 线性化 | 强 | 强 |
| 多数 | 有限过期 | 强 |
| Local | 一致前缀 | 一致前缀 |

## <a name="next-steps"></a>后续步骤

详细了解 Azure Cosmos DB API 与开源 API 之间的一致性级别和兼容性。 请参阅以下文章：

* [各种一致性级别的可用性和性能权衡](consistency-levels-tradeoffs.md)
* [Azure Cosmos DB 的用于 MongoDB 的 API 支持的 MongoDB 功能](mongodb-feature-support.md)

<!-- Not Available on * [Apache Cassandra features supported by the Azure Cosmos DB Cassandra API](cassandra-support.md)-->

<!-- Update_Description: update meta properties, wording update -->