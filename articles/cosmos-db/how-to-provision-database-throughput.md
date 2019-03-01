---
title: 在 Azure Cosmos DB 中预配数据库吞吐量
description: 了解如何在 Azure Cosmos DB 中预配数据库级别的吞吐量
author: rockboyfor
ms.service: cosmos-db
ms.topic: sample
origin.date: 11/06/2018
ms.date: 03/04/2019
ms.author: v-yeche
ms.openlocfilehash: b220e6c8386fb4a7e7fab77f1575ffed70492d97
ms.sourcegitcommit: b56dae931f7f590479bf1428b76187917c444bbd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2019
ms.locfileid: "56987898"
---
# <a name="provision-throughput-for-a-database-in-azure-cosmos-db"></a>在 Azure Cosmos DB 中为数据库预配吞吐量

本文介绍如何在 Azure Cosmos DB 中为数据库预配吞吐量。 可以为单个[容器](how-to-provision-container-throughput.md)预配吞吐量，也可以为数据库预配吞吐量，并在数据库中的容器之间共享吞吐量。 可以使用 Azure 门户或 Azure Cosmos DB SDK 来预配数据库级别吞吐量。

## <a name="provision-throughput-by-using-azure-portal"></a>使用 Azure 门户预配吞吐量

<a name="portal-sql"></a>
### <a name="sql-core-api"></a>SQL（核心）API

1. 登录到 [Azure 门户](https://portal.azure.cn/)。

1. [创建新的 Azure Cosmos DB 帐户](create-sql-api-dotnet.md#create-a-database-account)或选择现有的帐户。

1. 打开“数据资源管理器”窗格，然后选择“新建数据库”。 提供以下详细信息：

    * 输入数据库 ID。 
    * 选择“预配吞吐量”。
    * 输入吞吐量（例如 1000 RU）。
    * 选择“确定” 。

    ![“新建数据库”对话框屏幕截图](./media/how-to-provision-database-throughput/provision-database-throughput-portal-all-api.png)

## <a name="provision-throughput-by-using-net-sdk"></a>使用 .NET SDK 预配吞吐量

> [!Note]
> 使用 SQL API 为所有 API 预配吞吐量。 也可以选择将以下示例用于 Cassandra API。

<a name="dotnet-all"></a>
### <a name="all-apis"></a>所有 API

```csharp
//set the throughput for the database
RequestOptions options = new RequestOptions
{
    OfferThroughput = 10000
};

//create the database
await client.CreateDatabaseIfNotExistsAsync(
    new Database {Id = databaseName},  
    options);
```

<a name="dotnet-cassandra"></a>
### <a name="cassandra-api"></a>Cassandra API

```csharp
// Create a Cassandra keyspace and provision throughput of 10000 RU/s
session.Execute(CREATE KEYSPACE IF NOT EXISTS myKeySpace WITH cosmosdb_provisioned_throughput=10000);
```

## <a name="next-steps"></a>后续步骤

请参阅以下文章，了解如何在 Azure Cosmos DB 中预配吞吐量：

* [如何为容器预配吞吐量](how-to-provision-container-throughput.md)
* [Azure Cosmos DB 中的请求单位和吞吐量](request-units.md)

<!-- Update_Description: update meta properties, wording update -->
