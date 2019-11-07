---
title: 将数据从 MongoDB 迁移到 Azure Cosmos DB MongoDB API 的迁移前步骤
description: 本文档概述将数据从 MongoDB 迁移到 Cosmos DB 的先决条件。
author: rockboyfor
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
origin.date: 04/17/2019
ms.date: 10/28/2019
ms.author: v-yeche
ms.openlocfilehash: 593ae95a2a0a87b7acf62a784edb78cea592b8f7
ms.sourcegitcommit: 73f07c008336204bd69b1e0ee188286d0962c1d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2019
ms.locfileid: "72914813"
---
# <a name="pre-migration-steps-for-data-migrations-from-mongodb-to-azure-cosmos-dbs-api-for-mongodb"></a>将数据从 MongoDB 迁移到 Azure Cosmos DB MongoDB API 的迁移前步骤

在将数据从 MongoDB（本地或云中 (IaaS)）迁移到 Azure Cosmos DB MongoDB API 之前，应执行以下操作：

1. [创建 Azure Cosmos DB 帐户](#create-account)
2. [估算工作负荷所需的吞吐量](#estimate-throughput)
3. [为数据选择最佳的分区键](#partitioning)
4. [了解可对数据设置的索引策略](#indexing)

如果已完成上述迁移先决条件，请参阅[将 MongoDB 数据迁移到 Azure Cosmos DB MongoDB API](../dms/tutorial-mongodb-cosmos-db.md)获取实际的数据迁移说明。 否则，请参阅本文档获取有关处理这些先决条件的说明。 

<a name="create-account"></a>
## <a name="create-an-azure-cosmos-db-account"></a>创建 Azure Cosmos DB 帐户 

在开始迁移之前，需要[使用 Azure Cosmos DB MongoDB API 创建一个 Azure Cosmos 帐户](create-mongodb-dotnet.md)。 

创建帐户时，可以选择[多区域分发](distribute-data-globally.md)数据的设置。 还可以选择启用多区域写入（或多主数据库配置），这样，每个区域既可以充当写入区域，也可以充当读取区域。

![Account-Creation](./media/mongodb-pre-migration/account-creation.png)

<a name="estimate-throughput"></a>
## <a name="estimate-the-throughput-need-for-your-workloads"></a>估算工作负荷所需的吞吐量

在使用[数据库迁移服务 (DMS)](../dms/dms-overview.md) 开始迁移之前，应估算要为 Azure Cosmos 数据库和集合预配的吞吐量。

可以针对以下任何一项预配吞吐量：

- 集合

- 数据库

> [!NOTE]
> 你还可为上述两项的组合预配吞吐量，其中，数据库中的某些集合可以拥有专用的预配吞吐量，而其他一些集合则可以共享吞吐量。 有关详细信息，请参阅[对数据库和容器设置吞吐量](set-throughput.md)页。
>

应该先决定是要预配数据库、集合级别的还是两者组合的吞吐量。 一般情况下，我们建议在集合级别配置专用吞吐量。 在数据库级预配吞吐量可使数据库中的集合共享预配的吞吐量。 不过，在使用共享的吞吐量时，不能保证每个集合获得特定的吞吐量，并且任意特定集合的性能不可预测。

如果你不确定要为每个集合预配多少专用吞吐量，可以选择数据库级吞吐量。 可将针对 Azure Cosmos 数据库配置的预配吞吐量视为在逻辑上等同于 MongoDB VM 或物理服务器的计算容量，但前者更具成本效益且支持弹性缩放。 有关详细信息，请参阅[对 Azure Cosmos 容器和数据库预配吞吐量](set-throughput.md)。

如果在数据库级别预配吞吐量，必须使用分区/分片键创建该数据库中的所有集合。 有关分区的详细信息，请参阅 [ Azure Cosmos DB 中的分区和横向缩放](partition-data.md)。 如果在迁移过程中不指定分区/分片键，Azure 数据库迁移服务将在分片键字段中自动填充为每个文档自动生成的 *_id* 属性。

### <a name="optimal-number-of-request-units-rus-to-provision"></a>要预配的最佳请求单位 (RU) 数

在 Azure Cosmos DB 中，吞吐量是提前预配的，按每秒请求单位 (RU) 数计量。 如果工作负荷在 VM 上或本地运行 MongoDB，请将 RU 视为物理资源的简单抽象，例如，表示 VM 或本地服务器的大小，以及它们拥有的资源（如内存、CPU 和 IOPs）。 

不同于 VM 或本地服务器，RU 随时可以轻松纵向缩放。 在几秒钟内即可更改预配的 RU 数，并且只需为给定的一小时时段内预配的最大 RU 数付费。 有关详细信息，请参阅 [Azure Cosmos DB 中的请求单位](request-units.md)。

下面是影响所需 RU 数的关键因素：
- **项（即文档）大小**：随着项/文档的增大，读取或写入该项/文档所要消耗的 RU 数也会增加。
- **项属性计数**：假设所有属性采用[默认索引](index-overview.md)，写入某个项所要消耗的 RU 数会随着项属性计数的增加而增加。 可以通过[限制已编制索引的属性数目](index-policy.md)，来减少写入操作的请求单位消耗量。
- **并发操作**：消耗的请求单位数还取决于执行不同 CRUD 操作（例如写入、读取、更新、删除）和更复杂查询的频率。 可以使用 [mongostat](https://docs.mongodb.com/manual/reference/program/mongostat/) 输出当前 MongoDB 数据的并发需求。
- **查询模式**：查询的复杂性会影响查询消耗的请求单位数。

如果使用 [mongoexport](https://docs.mongodb.com/manual/reference/program/mongoexport/) 导出 JSON 文件并了解每秒发生的写入、读取、更新和删除操作数量，则可以使用 [Azure Cosmos DB 容量规划器](https://www.documentdb.com/capacityplanner)来估算要预配的初始 RU 数。 容量规划器不考虑较复杂查询的成本。 因此，如果对数据运行复杂查询，将会消耗更多的 RU。 该计算器还假设所有字段已编制索引并使用会话一致性。 了解查询成本的最佳方式是将数据（或示例数据）迁移到 Azure Cosmos DB，[连接到 Cosmos DB 的终结点](connect-mongodb-account.md)，并在 MongoDB Shell 中使用 `getLastRequestStastistics` 命令运行示例查询以获取请求费用，此命令将输出消耗的 RU 数：

`db.runCommand({getLastRequestStatistics: 1})`

此命令将输出如下所示的 JSON 文档：

```{  "_t": "GetRequestStatisticsResponse",  "ok": 1,  "CommandName": "find",  "RequestCharge": 10.1,  "RequestDurationInMilliSeconds": 7.2}```

了解查询消耗的 RU 数以及该查询的并发需求后，可以调整预配的 RU 数。 优化 RU 不是一次性的事件 - 应该根据是否预期不会出现繁重的流量，而是出现繁重的工作负荷或导入数据，来持续优化或纵向扩展预配的 RU。

<a name="partitioning"></a>
## <a name="choose-your-partition-key"></a>选择分区键
分区是在迁移到 Azure Cosmos DB 等多区域分布式数据库之前需要考虑的要点。 Azure Cosmos DB 使用分区缩放数据库中的单个容器，以满足应用程序的可伸缩性和性能需求。 在分区中，可将容器中的项分割成不同的子集（称作“逻辑分区”）。 有关为数据选择适当分区键的详细信息和建议，请参阅[选择分区键](/cosmos-db/partitioning-overview#choose-partitionkey)部分。 

<a name="indexing"></a>
## <a name="index-your-data"></a>为数据编制索引
默认情况下，Azure Cosmos DB 在引入时会为所有数据字段编制索引。 随时可以在 Azure Cosmos DB 中修改[索引策略](index-policy.md)。 事实上，我们通常建议在迁移数据时关闭索引，然后在数据已进入 Cosmos DB 后重新打开索引。 有关索引的更多详细信息，请参阅 [Azure Cosmos DB 中的索引](index-overview.md)部分。 

[Azure 数据库迁移服务](../dms/tutorial-mongodb-cosmos-db.md)自动迁移具有唯一索引的 MongoDB 集合。 但是，必须在迁移之前创建唯一索引。 如果集合中已包含数据，Azure Cosmos DB 将不支持创建唯一索引。 有关详细信息，请参阅 [Azure Cosmos DB 中的唯一键](unique-keys.md)。

## <a name="next-steps"></a>后续步骤
* [使用数据库迁移服务将 MongoDB 数据迁移到 Cosmos DB](../dms/tutorial-mongodb-cosmos-db.md) 
* [对 Azure Cosmos 容器和数据库预配吞吐量](set-throughput.md)
* [Azure Cosmos DB 中的分区](partition-data.md)
* [Azure Cosmos DB 中的全局分布](distribute-data-globally.md)
* [Azure Cosmos DB 中的索引](index-overview.md)
* [Azure Cosmos DB 中的请求单位](request-units.md)

<!--Update_Description: new articles on mongodb pre migration -->
<!--New.date: 10/28/2019-->