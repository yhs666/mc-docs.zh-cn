---
title: 优化 Azure Cosmos DB 中的读取和写入成本
description: 本文介绍如何在对数据执行读取和写入操作时降低 Azure Cosmos DB 成本。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 12/07/2018
ms.date: 12/31/2018
ms.author: v-yeche
ms.openlocfilehash: 880b02ceeb634f0484dabf573c340e0f19ef174c
ms.sourcegitcommit: 54ddd3dc2452d7af3a6fa66dae908ad0c4ef99dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/29/2018
ms.locfileid: "53814802"
---
# <a name="optimize-the-cost-required-to-read-and-write-data-from-azure-cosmos-db"></a>优化从 Azure Cosmos DB 读取和写入数据所需的成本

本文介绍如何计算从 Azure Cosmos DB 读取和写入数据所需的成本。 读取操作包括对项的 Get 操作；写入操作包括项的插入、替换、删除和 upsert。  

## <a name="cost-of-reads-and-writes"></a>读取和写入成本

Azure Cosmos DB 通过使用预配置吞吐量模型，在吞吐量和延迟方面保证可预测的性能。 预配的吞吐量以每秒[请求单位](request-units.md)或 RU/秒表示。 请求单位 (RU) 是对计算 CPU、内存、IO 等资源的逻辑抽象，这是执行请求所必需的。 预留的预配吞吐量 (RU) 专用于容器或数据库，以提供可预测的吞吐量和延迟。 预配置吞吐量使 Azure Cosmos DB 能够以任何规模提供可预测且一致的性能，保证低延迟和高可用性。 请求单位表示规范化货币，简化了对应用程序需要多少资源的推理。 

无需考虑在读取和写入之间区分请求单位。 请求单位使用统一的货币模型，可交替使用相同的吞吐量容量进行读取和写入，提高了工作效率。 下表以 RU/秒表示 1 KB 和 100 KB 大小项的读取和写入成本。

|**项大小**  |**一次读取成本** |**一次写入成本**|
|---------|---------|---------|
|1 KB |1 RU |5 RU |
|100 KB |10 RU |50 RU |

读取 1 KB 大小的项花费一个 RU。 写入 1 KB 的项花费五个 RU。 使用默认会话[一致性级别](consistency-levels.md)时，会同时计算读取和写入成本。  RU 的考虑因素包括：项大小、属性计数、数据一致性、索引属性、索引和查询模式。

<!--Not Available on ## Normalized cost for 1 million reads and writes-->
## <a name="number-of-regions-and-the-request-units-cost"></a>区域数量和请求单位成本

无论与 Azure Cosmos 帐户关联的区域数量如何，写入成本都是恒定的。 换句话说，1 KB 写入将花费五个 RU，而与该帐户关联的区域数量无关。 在复制、接受和处理每个区域的复制流量方面，花费了大量的资源。 有关多区域成本优化的详细信息，请参阅[优化多区域 Cosmos 帐户成本](optimize-cost-regions.md)一文。

## <a name="optimize-the-cost-of-writes-and-reads"></a>优化写入和读取成本

执行写入操作时，应预配足够的容量以支持每秒所需的写入次数。 可在执行写入之前使用 SDK、门户网站、CLI 来增加预配置的吞吐量，然后在写入完成后降低吞吐量。 写入期间的吞吐量是给定数据所需的最小吞吐量，加上插入工作负荷所需的吞吐量（假设没有其他工作负荷正在运行）。 

如果同时运行其他工作负荷（例如，查询/读取/更新/删除），则还应添加执行这些操作所需的其他请求单位。 如果写入操作速率受到限制，可使用 Azure Cosmos DB SDK 自定义重试/回退策略。 例如，可以增加负载，直到一小部分请求的速率受到限制。 如果发生速率限制，则客户端应用程序应在指定重试间隔的速率限制请求时退出。 重试写入之前，重试之间应有最小的时间间隔。 重试策略支持包含在 SQL.NET、Java、Node.js 和 Python SDK 和所有受支持的 .NET Core SDK 版本中。 

<!-- Not Available on You can also bulk insert data into Azure Cosmos DB or copy data from any supported source data store to Azure Cosmos DB by using [Azure Data Factory](../data-factory/connector-azure-cosmos-db.md). Azure Data Factory natively integrates with the Azure Cosmos DB Bulk API to provide the best performance, when you write data.-->

## <a name="next-steps"></a>后续步骤

接下来，可通过以下文章详细了解 Azure Cosmos DB 中的成本优化：

* 详细了解[开发和测试优化](optimize-dev-test.md)
<!--Not Available on * Learn more about [Understanding your Azure Cosmos DB bill](understand-your-bill.md)-->
* 详细了解如何[优化吞吐量成本](optimize-cost-throughput.md)
* 详细了解如何[优化存储成本](optimize-cost-storage.md)
* 详细了解如何[优化查询成本](optimize-cost-queries.md)
* 详细了解[优化多区域 Azure Cosmos 帐户的成本](optimize-cost-regions.md)