---
title: 一致性级别和 Azure Cosmos DB API
description: 了解 Azure Cosmos DB 中 API 的一致性级别。
keywords: 一致性, azure cosmos db, azure, 模型, mongodb, cassandra, 图, 表, 21Vianet Azure
services: cosmos-db
author: rockboyfor
ms.author: v-yeche
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 10/23/2018
ms.date: 01/07/2019
ms.openlocfilehash: 125cf650f1ede4a793e21db3ad382ec67975022d
ms.sourcegitcommit: ce4b37e31d0965e78b82335c9a0537f26e7d54cb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/04/2019
ms.locfileid: "54026834"
---
# <a name="consistency-levels-and-azure-cosmos-db-apis"></a>一致性级别和 Azure Cosmos DB API

Azure Cosmos DB SQL API 本机支持 Azure Cosmos DB 提供的五个一致性模型。 使用 Azure Cosmos DB 时，SQL API 是默认 API。 

Azure Cosmos DB 还为常用数据库提供对与线路协议兼容的 API 的本机支持。 数据库包括 MongoDB。 数据库既没有提供准确定义的一致性模型，也没有为一致性级别提供由 SLA 支持的保证。 它们通常仅提供 Azure Cosmos DB 提供的五个一致性模型的一个子集。 对于 SQL API，将使用 Azure Cosmos DB 帐户上配置的默认一致性级别。 

<!-- Not Available on Apache Cassandra, Gremlin, and Azure Tables-->
<!-- Not Available on Gremlin API and Table API--> 以下各部分介绍了由 MongoDB 3.4 的 OSS 客户端驱动程序请求的数据一致性之间的映射。 本文档还介绍了 MongoDB 对应的 Azure Cosmos DB 一致性级别。

<!-- Not Available on Apache Cassandra 4.x which use Cassandra API-->
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
<!-- Update_Description: wording update -->