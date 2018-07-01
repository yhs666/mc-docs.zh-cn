---
title: 作为键值存储的 Azure Cosmos DB - 费用概述 | Azure
description: 了解将 Azure Cosmos DB 用作键值存储时的低成本。
keywords: 键值存储
services: cosmos-db
author: rockboyfor
manager: digimobile
tags: ''
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
origin.date: 11/15/2017
ms.date: 07/02/2018
ms.author: v-yeche
ms.openlocfilehash: e1429188a1393a93d220dfe11abf4c94e33f8fe0
ms.sourcegitcommit: 4ce5b9d72bde652b0807e0f7ccb8963fef5fc45a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/28/2018
ms.locfileid: "37070163"
---
# <a name="azure-cosmos-db-as-a-key-value-store---cost-overview"></a>用作键值存储的 Azure Cosmos DB - 费用概述

Azure Cosmos DB 是一个多区域分布式多模型数据库服务，用于轻松构建高度可用的大规模应用程序。 默认情况下，Azure Cosmos DB 会自动为其引入的所有数据高效编制索引。 因此，可针对任何类型的数据执行快速一致的 [SQL](sql-api-sql-query.md)（和 [JavaScript](programming.md)）查询。 
<!-- Notice: 全球 to 多个区域 -->

本文介绍使用 Azure Cosmos DB 作为键/值存储执行简单写入和读取操作时产生的成本。 写入操作包括文档的插入、替换、删除和更新插入。 除了针对所有采用宽松一致性的单区域帐户和多区域帐户提供有保证的 99.99% 可用性 SLA，针对所有多区域数据库帐户提供 99.999% 读取可用性以外，Azure Cosmos DB 还保证读取延迟小于 10 毫秒，（索引）写入延迟小于 15 毫秒，SLA 高达 99%。 

## <a name="why-we-use-request-units-rus"></a>为何使用请求单位 (RU)

Azure Cosmos DB 的性能基于分区的预配[请求单位](request-units.md) (RU) 数量。 预配属于另一种粒度，根据每秒 RU 数（[请不要与每小时计费相混淆](https://www.azure.cn/pricing/details/cosmos-db/)）购买。 应将 RU 视为一种货币，用于简化应用程序所需吞吐量的预配过程。 客户无需考虑读取和写入容量单位之间的差异。 RU 的单一货币模型能够有效地在读取和写入之间分享预配的容量。 这种预配的容量模型使服务能够提供可预测且一致的吞吐量，保证低延迟、高可用性。 最后，我们使用 RU 来为吞吐量建模，但每个预配的 RU 还具有定义数量的资源（内存、核心）。 每秒 RU 数不仅仅是 IOPS。

作为一种多区域分布式数据库系统，Cosmos DB 是唯一除提供高可用性外还在延迟、吞吐量和一致性方面提供 SLA 的 Azure 服务。 预配的吞吐量应用到与 Cosmos DB 数据库帐户关联的每个区域。 对于读取，Cosmos DB 提供多个妥善定义的[一致性级别](consistency-levels.md)供用户选择。 
<!-- Notice: 全球 to 多个区域 -->

下表显示基于 1 KB 和 100 KB 自定义文档大小执行读取和写入事务所需的 RU 数量。

|项大小|1 次读取|1 次写入|
|-------------|------|-------|
|1 KB|1 RU|5 RU|
|100 KB|10 RU|50 RU|

<!-- Not Available ## Cost of reads and writes -->

## <a name="next-steps"></a>后续步骤

请持续关注有关优化 Azure Cosmos DB 资源预配的新文章。 同时，欢迎使用我们的 [RU 计算器](https://www.documentdb.com/capacityplanner)。

<!--Update_Description: update meta properties, wording update-->