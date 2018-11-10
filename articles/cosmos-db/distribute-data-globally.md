---
title: 使用 Azure Cosmos DB 在多个区域分配数据 | Azure
description: 了解如何通过多区域分配式多模型数据库服务 Azure Cosmos DB，使用多区域数据库进行多区域范围的异地复制、多主数据库故障转移和数据恢复。
services: cosmos-db
author: rockboyfor
manager: digimobile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
origin.date: 09/24/2018
ms.date: 11/05/2018
ms.author: v-yeche
ms.openlocfilehash: b2f4ef53f591849615498ded57dccfeaae662d82
ms.sourcegitcommit: c1020b13c8810d50b64e1f27718e9f25b5f9f043
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2018
ms.locfileid: "50204816"
---
<!-- Notice in meta: 全球分布 to 多个区域分布 -->
<!-- Notice in meta: 全球范围 to 多个数据中心范围 -->
# <a name="multiple-region-data-distribution-with-azure-cosmos-db"></a>使用 Azure Cosmos DB 多区域分配数据
Azure 无所不在 - 遍布多个区域（跨中国的多个地理区域）并且仍在持续扩展中。 凭借其多区域存在和多主数据库支持，Azure 为其开发人员提供的差异化功能之一是能够轻松构建、部署和管理多区域分配式应用程序。

<!-- Notice: 全球 to 多个区域分布 -->
[Azure Cosmos DB](../cosmos-db/introduction.md) 是 21Vianet 针对任务关键型应用程序提供的多区域分配式多模型数据库服务。 Azure Cosmos DB 在多个区域提供统包多区域分配、[吞吐量和存储弹性缩放](../cosmos-db/partition-data.md)、99% 情况下低至个位数的读写毫秒级延迟、[妥善定义的一致性模型](consistency-levels.md)，以及得到保证的高可用性，所有这些均由[行业领先的综合 SLA](https://www.azure.cn/support/sla/cosmos-db/) 提供支持。 Azure Cosmos DB [自动为所有数据编制索引](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf)，无需客户管理架构或索引。

<!-- Not Available on Key/Value and Graph -->
<!-- Notice: 全球分布 to 多个区域分布 -->
## <a name="multiple-region-distribution-with-multi-master"></a>通过多主数据库功能实现多区域分配

作为云服务，Azure Cosmos DB 经过精心设计，支持将多租户、多区域分配和多主数据库功能用于文档和列系列数据模型。

<!-- Not Available on key-value, graph-->
![跨 3 个区域进行分区和分布的 Azure Cosmos DB 容器](./media/distribute-data-globally/global-apps.png)

**跨多个 Azure 区域进行分区和分布的一个 Azure Cosmos DB 容器**

正如我们构建 Azure Cosmos DB 时所了解到的那样，添加多区域分发不能事后才进行规划。 不能将其“直接附加”到“单一站点”数据库系统之上。 多区域分布式数据库提供的功能超越了“单一站点”数据库提供的传统地理灾难恢复 (GEO-DR) 提供的功能。 提供异地灾难恢复功能的单站点数据库是分布在多个区域的数据库的严格子集。

<!-- Notice: 全球分布 to 多个区域分布 --> 
<!-- Notice: 全球各地 to 各个区域 --> 

使用 Azure Cosmos DB 的统包多区域分配功能，开发人员可以通过数据库日志采用 Lambda 模式，或跨多个区域执行“双写”，而无需构建自己的复制基架。 不建议采用这些方法，因为无法确保此类方法的正确性并提供合理正确的 SLA。

## <a name="Next Steps"></a>后续步骤

* [Azure Cosmos DB 如何启用统包式多区域分配](distribute-data-globally-turnkey.md)

* [Azure Cosmos DB 多区域分配的主要优点](distribute-data-globally-benefits.md)

* [如何配置 Azure Cosmos DB 多区域数据库复制](tutorial-global-distribution-sql-api.md)

* [如何为 Azure Cosmos DB 帐户启用多主数据库](enable-multi-master.md)

* [Azure Cosmos DB 中的自动和手动故障转移的工作原理](regional-failover.md)

* [了解 Azure Cosmos DB 中的冲突解决](multi-master-conflict-resolution.md)

* [将多主数据库与开放源代码 NoSQL 数据库结合使用](multi-master-oss-nosql.md)

<!--Update_Description: update meta properties, wording update, update link -->