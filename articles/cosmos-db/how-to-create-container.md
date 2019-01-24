---
title: 在 Azure Cosmos DB 中创建容器
description: 了解如何在 Azure Cosmos DB 中创建容器
author: rockboyfor
ms.service: cosmos-db
ms.topic: sample
origin.date: 11/06/2018
ms.date: 01/21/2019
ms.author: v-yeche
ms.openlocfilehash: c3126816300cb4e0225af05a1af719011f8d9373
ms.sourcegitcommit: bbd2a77feeb7e5b7b4c6161687d60cc2b7315b5b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2019
ms.locfileid: "54857403"
---
# <a name="create-a-container-in-azure-cosmos-db"></a>在 Azure Cosmos DB 中创建容器

本文介绍如何通过不同方式来创建容器（集合、表、图形）。 可以通过 Azure 门户、Azure CLI 或支持的 SDK 来创建容器。 本文演示如何创建容器、指定分区键和预配吞吐量。

## <a name="create-a-container-using-azure-portal"></a>使用 Azure 门户创建容器

<a name="portal-sql"></a>
### <a name="sql-api"></a>SQL API

1. 登录到 [Azure 门户](https://portal.azure.cn/)。

1. [新建 Cosmos DB 帐户](create-sql-api-dotnet.md#create-a-database-account)或选择现有的帐户。

1. 打开“数据资源管理器”窗格，然后选择“新建集合”。 接下来，在窗体中填写以下详细信息：

   * 创建新数据库或使用现有数据库。
   * 输入集合 ID。
   * 输入分区键。
   * 输入吞吐量，例如 1000 RU。
   * 选择“确定” 。

![SQL API 会创建一个集合。](./media/how-to-create-container/partitioned-collection-create-sql.png)

<a name="portal-mongodb"></a>
### <a name="azure-cosmos-dbs-api-for-mongodb"></a>Azure Cosmos DB 的用于 MongoDB 的 API

1. 登录到 [Azure 门户](https://portal.azure.cn/)。

1. [新建 Cosmos DB 帐户](create-mongodb-dotnet.md#create-a-database-account)或选择现有的帐户。

1. 打开“数据资源管理器”窗格，然后选择“新建集合”。 接下来，在窗体中填写以下详细信息：

   * 创建新数据库或使用现有数据库。
   * 输入集合 ID。
   * 对于“无限制”存储容量。
   * 输入分片键。
   * 输入吞吐量，例如 1000 RU。
   * 选择“确定” 。

![Azure Cosmos DB 的用于 MongoDB 的 API 创建集合](./media/how-to-create-container/partitioned-collection-create-mongodb.png)

<a name="portal-cassandra"></a>
<!-- Not Available on ### Cassandra API-->

<a name="portal-gremlin"></a>
<!-- Not Available on ### Gremlin API-->

<a name="portal-table"></a>
<!-- Not Available on ### Table API-->
## <a name="create-a-container-using-azure-cli"></a>使用 Azure CLI 创建容器

<a name="cli-sql"></a>
### <a name="sql-api"></a>SQL API

```azurecli
# Create a container with a partition key and provision 1000 RU/s throughput.

az cosmosdb collection create \
    --resource-group $resourceGroupName \
    --collection-name $containerName \
    --name $accountName \
    --db-name $databaseName \
    --partition-key-path /myPartitionKey \
    --throughput 1000
```

<a name="cli-mongodb"></a>
### <a name="azure-cosmos-dbs-api-for-mongodb"></a>Azure Cosmos DB 的用于 MongoDB 的 API

```azurecli
# Create a collection with a shard key and provision 1000 RU/s throughput.
az cosmosdb collection create \
    --resource-group $resourceGroupName \
    --collection-name $collectionName \
    --name $accountName \
    --db-name $databaseName \
    --partition-key-path /myShardKey \
    --throughput 1000
```

<a name="cli-cassandra"></a>
<!-- Not Available on ### Cassandra API-->

<a name="cli-gremlin"></a>
<!-- Not Available on ### Gremlin API-->

<a name="cli-table"></a>
<!-- Not Available on ### Table API-->

## <a name="create-a-container-using-net-sdk"></a>使用 .NET SDK 创建容器

<a name="dotnet-sql-graph"></a>
### <a name="sql-api"></a>SQL API

<!-- Not Available on Gremlin API-->
```csharp
// Create a container with a partition key and provision 1000 RU/s throughput.
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "myContainerName";
myCollection.PartitionKey.Paths.Add("/myPartitionKey");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("myDatabaseName"),
    myCollection,
    new RequestOptions { OfferThroughput = 1000 });
```

<a name="dotnet-mongodb"></a>
### <a name="azure-cosmos-dbs-api-for-mongodb"></a>Azure Cosmos DB 的用于 MongoDB 的 API

```csharp
// Create a collection with a partition key by using Mongo Shell:
db.runCommand( { shardCollection: "myDatabase.myCollection", key: { myShardKey: "hashed" } } )
```

> [!Note]
> MongoDB 线路协议没有请求单位的概念。 若要创建新的包含吞吐量的集合，请使用 Azure 门户或 SQL API，如前面的示例所示。

<a name="dotnet-cassandra"></a>
<!-- Not Available on ### Cassandra API-->

## <a name="next-steps"></a>后续步骤

请参阅以下文章，了解 Cosmos DB 中的分区：

- [Azure Cosmos DB 中的分区](partitioning-overview.md)

<!-- Update_Description: update meta properties, wording update  -->
