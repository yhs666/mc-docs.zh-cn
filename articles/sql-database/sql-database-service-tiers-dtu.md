---
title: Azure SQL 数据库服务层 - DTU | Azure
description: 了解单一数据库和池数据库的服务层以提供性能级别和存储大小。
services: sql-database
author: WenJason
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: article
origin.date: 06/28/2018
ms.date: 08/06/2018
manager: digimobile
ms.author: v-nany
ms.openlocfilehash: dd22b2d532fd739db43b16644288089959c39548
ms.sourcegitcommit: 7ea906b9ec4f501f53b088ea6348465f31d6ebdc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/03/2018
ms.locfileid: "39486701"
---
# <a name="choosing-a-dtu-based-service-tier-performance-level-and-storage-resources"></a>选择基于 DTU 的服务层、性能级别和存储资源 

服务层根据一系列具有固定随附存储量、固定备份保留期和固定价格的性能级别进行区分。 所有服务层允许灵活更改性能级别，而不会造成停机。 单一数据库和弹性池根据服务层和性能级别按小时计费。

> [!IMPORTANT]
> SQL 数据库托管实例（目前以公共预览版提供）不支持基于 DTU 的购买模型。 有关详细信息，请参阅 [Azure SQL 数据库托管实例](sql-database-managed-instance.md)。 

## <a name="choosing-a-dtu-based-service-tier"></a>选择的基于 DTU 的服务层

选择服务层首要考虑的是业务连续性、存储和性能需求。
||基本|标准|高级|
| :-- | --: |--:| --:| --:| 
|目标工作负荷|开发和生产|开发和生产|开发和生产||
|运行时间 SLA|99.99%|99.99%|99.99%|在预览版中不适用|
|备份保留|7 天|35 天|35 天|
|CPU|低|低、中、高|中、高|
|IO 吞吐量（近似） |每个 DTU 2.5 IOPS| 每个 DTU 2.5 IOPS | 每个 DTU 48 IOPS|
|IO 延迟（近似）|5 毫秒（读取），10 毫秒（写入）|5 毫秒（读取），10 毫秒（写入）|2 毫秒（读取/写入）|
|列存储索引 |不适用|S3 及更高版本|支持|
|内存中 OLTP|不适用|不适用|支持|
|||||

## <a name="single-database-dtu-and-storage-limits"></a>单一数据库 DTU 和存储限制

单一数据库的性能级别以数据库事务单位 (DTU) 表示，弹性池则以弹性数据库事务单位 (eDTU) 表示。 有关 DTU 和 eDTU 的详细信息，请参阅[什么是 DTU 和 eDTU？](sql-database-service-tiers.md#what-are-database-transaction-units-dtus)

||基本|标准|高级|
| :-- | --: | --: | --: | --: |
| 最大存储大小 | 2 GB | 1 TB | 4 TB  | 
| 最大 DTU | 5 | 3000 | 4000 | |
||||||

## <a name="elastic-pool-edtu-storage-and-pooled-database-limits"></a>弹性池 eDTU、存储和已共用数据库限制

| | **基本** | **标准** | **高级** | 
| :-- | --: | --: | --: | --: |
| 每个数据库的最大存储大小  | 2 GB | 1 TB | 1 TB | 
| 每个池的最大存储大小 | 156 GB | 4 TB | 4 TB | 
| 每个数据库的最大 eDTU 数 | 5 | 3000 | 4000 | 
| 每个池的最大 eDTU 数 | 1600 | 3000 | 4000 | 
| 每个池的数据库数目上限 | 500  | 500 | 100 | 
||||||

## <a name="next-steps"></a>后续步骤

- 有关适用于单一数据库的特定性能级别和存储大小选项的详细信息，请参阅[适用于单一数据库的 SQL 数据库基于 DTU 的资源限制](sql-database-dtu-resource-limits-single-databases.md#single-database-storage-sizes-and-performance-levels)。
- 有关适用于弹性池的特定性能级别和存储大小选项的详细信息，请参阅 [SQL 数据库基于 DTU 的资源限制](sql-database-dtu-resource-limits-elastic-pools.md#elastic-pool-storage-sizes-and-performance-levels)。
