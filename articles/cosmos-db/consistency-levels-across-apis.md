---
title: 一致性级别和 Azure Cosmos DB API | Azure
description: 了解 Azure Cosmos DB 中 API 的一致性级别。
keywords: 一致性, azure cosmos db, azure, 模型, mongodb, cassandra, 图, 表, 21Vianet Azure
services: cosmos-db
author: rockboyfor
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
origin.date: 10/23/2018
ms.date: 12/03/2018
ms.author: v-yeche
ms.openlocfilehash: 94023af54f77fe293c91b356f431cddd43d9439e
ms.sourcegitcommit: 59db70ef3ed61538666fd1071dcf8d03864f10a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52676801"
---
# <a name="consistency-levels-and-azure-cosmos-db-apis"></a>一致性级别和 Azure Cosmos DB API

Cosmos DB SQL API 原生支持 Azure Cosmos DB 提供的五个一致性模型，Cosmos DB SQL API 是使用 Cosmos DB 时的默认 API。 除 SQL API 外，Cosmos DB 还为常用数据库（例如 MongoDB）提供了对线路协议兼容的 API 的本机支持。 这些数据库既没有提供准确定义的一致性模型，也没有为一致性级别提供由 SLA 支持的保证。 这些数据库通常仅提供 Cosmos DB 提供的五个一致性模型的一个子集。 对于 SQL API，将使用你在 Cosmos 帐户上配置的默认一致性级别。


<!-- Not Available on Apache Cassandra, Gremlin, and Azure Tables-->
<!-- Not Available on Gremlin API and Table API--> 以下各部分显示了当使用 MongoDB API 时，MongoDB 3.4 的 OSS 客户端驱动程序所请求的数据一致性与相应的 Cosmos DB 一致性级别之间的映射。

<!-- Not Available on Apache Cassandra 4.x which use Cassandra API-->
<a name="cassandra-mapping"></a>
<!-- Not Available on ## Mapping between Apache Cassandra and Cosmos DB consistency levels-->
<a name="mongo-mapping"></a>
## <a name="mapping-between-mongodb-34-and-cosmos-db-consistency-levels"></a>MongoDB 3.4 与 Cosmos DB 一致性级别之间的映射

针对多区域和单区域部署，下表显示了 MongoDB 3.4 与 Cosmos DB 中的默认一致性级别之间的“读取关注点”映射。

| **MongoDB 3.4** | **Cosmos DB（多区域）** | **Cosmos DB（单区域）** |
| - | - | - |
| 线性化 | 强 | 强 |
| 多数 | 有限过期 | 强 |
| Local | 一致前缀 | 一致前缀 |

## <a name="next-steps"></a>后续步骤

参阅以下文章，详细了解 Cosmos DB API 与开源 API 之间的一致性级别和兼容性：

* [各种一致性级别的可用性和性能权衡](consistency-levels-tradeoffs.md)
* [Cosmos DB MongoDB API 支持的 MongoDB 功能](mongodb-feature-support.md)

<!-- Not Available on * [Apache Cassandra features supported by Cosmos DB Cassandra API](cassandra-support.md)-->
<!-- Update_Description: new articles on cosmos db consisiency levels across apis -->
<!--ms.date: 12/03/2018-->