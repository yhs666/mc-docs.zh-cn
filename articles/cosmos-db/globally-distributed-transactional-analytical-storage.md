---
title: Azure Cosmos 容器的多区域分布式事务和分析存储
description: 了解 Azure Cosmos 容器的事务和分析存储及其配置选项。
author: rockboyfor
ms.author: v-yeche
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/30/2019
ms.date: 10/28/2019
ms.reviewer: sngun
ms.openlocfilehash: 4c2d2cd8afedc2c32096b5fa3c12bddbc5ff0398
ms.sourcegitcommit: 73f07c008336204bd69b1e0ee188286d0962c1d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2019
ms.locfileid: "72914798"
---
# <a name="multiple-regionally-distributed-transactional-and-analytical-storage-for-azure-cosmos-containers"></a>Azure Cosmos 容器的多区域分布式事务和分析存储

Azure Cosmos 容器在内部以两个存储引擎（事务存储引擎和可更新的分析存储引擎）为后盾。 这两个存储引擎采用结构化的日志，并已经过写入优化，可加快更新速度。 但是，两者的编码方式有所不同：

* **事务存储引擎** - 以行导向格式进行编码，可实现快速事务读取和查询。

* **分析存储引擎** - 以列格式进行编码，可实现快速分析查询和扫描。

![存储引擎和 Azure Cosmos DB API 映射](./media/globally-distributed-transactional-analytical-storage/storage-engines-api-mapping.png)

<!--MOONCAKE: CORRECT LINE 21 ON (./media/globally-distributed-xxxx.png)-->

事务存储引擎以本地 SSD 为后盾，而分析存储则存储在群集外部的经济型 SSD 存储上。 下表汇总了事务存储与分析存储之间的重要差异。

|功能  |事务存储  |分析存储 |
|---------|---------|---------|
|每个 Azure Cosmos 容器的最大存储空间 |   无限制      |    无限制     |
|每个逻辑分区键的最大存储空间   |   10 GB      |   无限制      |
|存储编码  |   行导向，使用内部格式。   |   列导向，使用 Apache Parquet 格式。 |
|存储区域性 |   以本地/群集内部 SSD 为后盾的复制存储。 |  以经济型远程/群集外部 SSD 为后盾的复制存储。       |
|持续性  |    99.99999（7-9 秒）     |  99.99999（7-9 秒）       |
|用于访问数据的 API  |   SQL、MongoDB、Cassandra、Gremlin 和表。       | Apache Spark         |
|保留期（生存时间或 TTL）   |  由策略驱动，使用 `DefaultTimeToLive` 属性在 Azure Cosmos 容器中配置。       |   由策略驱动，使用 `ColumnStoreTimeToLive` 属性在 Azure Cosmos 容器中配置。      |
|每 GB 价格    |   2\.576 元/GB      |       |
|存储事务的价格    | 预配吞吐量按每 100 RU/秒 0.008 美元收费，按小时计费。        |  基于消耗的吞吐量按 10,000 个写入事务 0.05 美元、10,000 个读取事务 0.004 美元收费。       |

<!--Not Avaialble on Line 34 and Etcd-->

## <a name="benefits-of-transactional-and-analytical-storage"></a>事务存储和分析存储的优势

### <a name="no-etl-operations"></a>无 ETL 操作

传统的分析管道比较复杂，它们包含多个阶段，而每个阶段都需要与计算和存储层之间来回执行提取-转换-加载 (ETL) 操作。 这会导致数据处理体系结构变得复杂。 这也意味着，存储和计算层的多个阶段会产生较高的成本，而且在存储和计算层的各个阶段之间传输大量数据会造成较高的延迟。  

对 Azure Cosmos DB 执行 ETL 操作不会产生任何开销。 每个 Azure Cosmos 容器以事务和分析存储引擎为后盾，事务与分析存储引擎之间的数据传输是在数据库引擎内部执行的，无需任何网络跃点。 与传统的分析解决方案相比，产生的延迟和成本要低得多。 单个多区域分布式存储系统即可同时满足事务和分析工作负荷的需求。  

### <a name="store-multiple-versions-update-and-query-the-analytical-storage"></a>存储多个版本并可更新和查询分析存储

分析存储完全可更新，其中包含 Azure Cosmos 容器中发生的所有事务更新的完整版本历史记录。

保证在 30 秒内，分析存储就能看到对事务存储所做的任何更新。 

### <a name="multiple-regionally-distributed-multi-master-analytical-storage"></a>多区域分布式多主数据库分析存储

如果 Azure Cosmos 帐户的范围限定为单个区域，则容器中存储的数据（存储在事务存储和分析存储中）的范围也限定为单个区域。 另一方面，如果 Azure Cosmos 帐户是多区域分布式的，则容器中存储的数据也是多区域分布式的。

对于配置了多个写入区域的 Azure Cosmos 帐户，始终在本地区域中执行对容器的写入操作（写入事务存储和分析存储），因此写入速度很快。

对于单区域或多区域 Azure Cosmos 帐户，无论使用单个写入区域（单主数据库）还是多个写入区域（也称为多主数据库），都会在给定区域的本地执行事务和分析读取/查询操作。

### <a name="performance-isolation-between-transactional-and-analytical-workloads"></a>事务和分析工作负荷之间的性能隔离

在给定的区域中，事务工作负荷针对容器的事务/行存储运行， 而分析工作负荷针对容器的分析/列存储运行。 这两个存储引擎独立运行，在工作负荷之间提供严格的性能隔离。

事务工作负荷消耗预配的吞吐量 (RU)。 与事务工作负荷不同，分析工作负荷的吞吐量基于实际消耗量。 分析工作负荷按需消耗资源。

### <a name="on-demand-snapshots-and-time-travel-analytics"></a>按需快照和时光穿越分析

随时可以通过对容器调用 `CreateSnapshot (name, timestamp)` 命令，来为 Azure Cosmos 容器的分析存储中存储的数据创建快照。 快照在容器曾经发生的更新历史记录中命名为“书签”。

![按需快照和时光穿越分析](./media/globally-distributed-transactional-analytical-storage/ondemand-analytical-data-snapshots.png)

<!--MOONCAKE: CORRECT LINE 72 ON  (./media/globally-distributed-xxxx.png)-->

创建快照时，除了名称以外，还可以指定时间戳来定义更新历史记录中容器的状态。 然后，可将快照数据载入 Spark 并执行查询。

目前，只能随时对容器创建按需快照；尚不支持基于计划或自定义策略自动创建快照。

### <a name="configure-and-tier-data-between-transactional-and-analytical-storage-independently"></a>在事务存储与分析存储之间单独配置和分层数据

根据具体的方案，可以单独启用或禁用这两个存储引擎中的每一个。 下面是每种方案的配置：

|方案 |事务存储设置  |分析存储设置 |
|---------|---------|---------|
|仅运行分析工作负荷（使用无限保留期） |  DefaultTimeToLive = 0       |  ColumnStoreTimeToLive = -1       |
|仅运行事务工作负荷（使用无限保留期）  |   DefaultTimeToLive = -1      |  ColumnStoreTimeToLive = 0       |
|同时运行事务工作负荷与分析工作负荷（使用无限保留期）   |   DefaultTimeToLive = -1      | ColumnStoreTimeToLive = -1        |
|同时运行事务工作负荷与分析工作负荷（使用不同的保留时间间隔，也称为存储分层）  |  DefaultTimeToLive = <Value1>       |     ColumnStoreTimeToLive = <Value2>    |

1. **仅为分析工作负荷配置容器（使用无限保留期）**

    可以仅为分析工作负荷配置 Azure Cosmos 容器。 此配置的优势是无需为事务存储付费。 如果你的目标是仅将容器用于分析工作负荷，可以通过在 Cosmos 容器中将 `DefaultTimeToLive` 设置为 0 来禁用事务存储，并可以通过将 `ColumnStoreTimeToLive` 设置为 -1 来启用使用无限保留期的分析存储。

    ![使用无限保留期的分析工作负荷](./media/globally-distributed-transactional-analytical-storage/analytical-workload-configuration.png)
    
    <!--MOONCAKE: CORRECT LINE 96 ON  (./media/globally-distributed-xxxx.png)-->
    
1. **仅为事务工作负荷配置容器（使用无限保留期）**

    可以仅为事务工作负荷配置 Azure Cosmos 容器。 可以通过在 Cosmos 容器中将 `ColumnStoreTimeToLive` 设置为 0 来禁用分析存储，并可以通过将 `DefaultTimeToLive` 设置为 -1 来启用使用无限保留期的分析存储。

    ![使用无限保留期的事务工作负荷](./media/globally-distributed-transactional-analytical-storage/transactional-workload-configuration.png)
    
    <!--MOONCAKE: CORRECT LINE 102 ON  (./media/globally-distributed-xxxx.png)-->
    
1. **同时为事务工作负荷与分析工作负荷配置容器（使用无限保留）**

    可以同时为事务工作负荷与分析工作负荷配置 Azure Cosmos 容器，并在它们之间实现完全的性能隔离。 可以通过将 `ColumnStoreTimeToLive` 设置为 -1 来启用分析存储，并通过将 `DefaultTimeToLive ` 设置为 -1 来启用使用无限保留期的事务存储。

    ![使用无限保留期的事务工作负荷与分析工作负荷](./media/globally-distributed-transactional-analytical-storage/analytical-transactional-configuration-infinite-retention.png)
    
    <!--MOONCAKE: CORRECT LINE 110 ON  (./media/globally-distributed-xxxx.png)-->
    
1. **同时为事务工作负荷与分析工作负荷配置容器（使用存储分层）**

    可以同时为事务工作负荷与分析工作负荷配置 Azure Cosmos 容器，并使用不同的保留时间间隔在它们之间实现完全的性能隔离。 Azure Cosmos DB 会强制使分析存储始终保留比事务存储更长的持续时间。

    可以通过将 `DefaultTimeToLive` 设置为 <Value 1> 来启用使用无限保留期的事务存储，并通过将 `ColumnStoreTimeToLive` 设置为 <Value 2> 来启用分析存储。 Azure Cosmos DB 会强制 <Value 2> 始终大于 <Value 1>。

    ![使用存储分层的事务工作负荷与分析工作负荷](./media/globally-distributed-transactional-analytical-storage/analytical-transactional-configuration-specified-retention.png)
    
    <!--MOONCAKE: CORRECT LINE 122 ON  (./media/globally-distributed-xxxx.png)-->
    
## <a name="next-steps"></a>后续步骤

* [Azure Cosmos DB 中的生存时间](time-to-live.md)

<!--Update_Description: new articles on global-distributed transactional analytical storage  -->
<!--New.date: 10/28/2019-->