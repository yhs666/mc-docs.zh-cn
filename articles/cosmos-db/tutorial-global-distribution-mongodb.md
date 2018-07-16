---
title: MongoDB API 的 Azure Cosmos DB 多区域分发教程 | Azure
description: 了解如何使用 MongoDB API 设置 Azure Cosmos DB 多区域分发。
services: cosmos-db
keywords: 多区域分发, MongoDB
author: rockboyfor
manager: digimobile
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.devlang: na
ms.topic: tutorial
origin.date: 05/10/2017
ms.date: 07/02/2018
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: 882870552aa1a739c61499a44faf2387f69aae89
ms.sourcegitcommit: 00c8a6a07e6b98a2b6f2f0e8ca4090853bb34b14
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/11/2018
ms.locfileid: "38939822"
---
# <a name="set-up-azure-cosmos-db-multiple-region-distribution-using-the-mongodb-api"></a>使用 MongoDB API 设置 Azure Cosmos DB 多区域分发

本文介绍如何使用 Azure 门户设置 Azure Cosmos DB 多区域分发，然后使用 MongoDB API 进行连接。

本文涵盖以下任务： 

> [!div class="checklist"]
> * 使用 Azure 门户配置多区域分发
> * 使用 [MongoDB API](mongodb-introduction.md) 配置多区域分发

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]

## <a name="verifying-your-regional-setup-using-the-mongodb-api"></a>使用 MongoDB API 验证区域设置
仔细检查 MongoDB 的 API 中的全局配置的最简单方法是从 Mongo Shell 运行 *isMaster()* 命令。

从 Mongo Shell：

   ```
      db.isMaster()
   ```

示例结果：

   ```JSON
      {
         "_t": "IsMasterResponse",
         "ok": 1,
         "ismaster": true,
         "maxMessageSizeBytes": 4194304,
         "maxWriteBatchSize": 1000,
         "minWireVersion": 0,
         "maxWireVersion": 2,
         "tags": {
            "region": "China North "
         },
         "hosts": [
            "vishi-api-for-mongodb-chinaeast.documents.azure.cn:10255",
            "vishi-api-for-mongodb-chinanorth.documents.azure.cn:10255",
         ],
         "setName": "globaldb",
         "setVersion": 1,
         "primary": "vishi-api-for-mongodb-chinanorth.documents.azure.cn:10255",
         "me": "vishi-api-for-mongodb-chinanorth.documents.azure.cn:10255"
      }
   ```

## <a name="connecting-to-a-preferred-region-using-the-mongodb-api"></a>使用 MongoDB API 连接到首选区域

使用 MongoDB API，可以为多区域分布式数据库指定集合的读取首选项。 为实现低延迟读取和全局高可用性，建议将集合的读取首选项设置为“就近”。 当读取首选项配置为“就近”时，将从最近的区域进行读取。

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.Nearest));
```

对于具有主读取/写入区域和用于灾难恢复 (DR) 方案的辅助区域的应用程序，建议将集合的读取首选项设置为“辅助优先”。 当读取首选项配置为“辅助优先”时，如果主区域不可用，将从辅助区域进行读取。

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.SecondaryPreferred));
```

最后，如果愿意，可以手动指定读取区域。 可以在读取首选项内设置区域标记。

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
var tag = new Tag("region", "China East");
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.Secondary, new[] { new TagSet(new[] { tag }) }));
```

本教程到此结束。 阅读 [Azure Cosmos DB 中的一致性级别](consistency-levels.md)，了解如何管理全局复制帐户的一致性。 有关 Azure Cosmos DB 中多区域数据库复制工作原理的详细信息，请参阅[使用 Cosmos DB 多区域分配数据](distribute-data-globally.md)。

## <a name="next-steps"></a>后续步骤

在本教程中已完成以下操作：

> [!div class="checklist"]
> * 使用 Azure 门户配置多区域分发
> * 使用 SQL API 配置多区域分发

现可继续学习下一个教程，了解如何使用 Azure Cosmos DB 本地模拟器在本地开发。

> [!div class="nextstepaction"]
> [通过模拟器在本地开发](local-emulator.md)

<!--Update_Description: update meta properties,wording update-->