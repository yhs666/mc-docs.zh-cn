---
title: Azure Cosmos DB：批量执行程序 .NET API、SDK 和资源
description: 了解有关批量执行程序 .NET API 的所有信息。
author: rockboyfor
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: reference
origin.date: 11/19/2018
ms.date: 06/17/2019
ms.author: v-yeche
ms.openlocfilehash: cef6258f343833020a83797ffc3dcf55cd94a386
ms.sourcegitcommit: 153236e4ad63e57ab2ae6ff1d4ca8b83221e3a1c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/18/2019
ms.locfileid: "67171320"
---
# <a name="net-bulk-executor-library-download-information"></a>.NET 批量执行程序库：下载信息 

> [!div class="op_single_selector"]
> * [.NET](sql-api-sdk-dotnet.md)
> * [.NET 更改源](sql-api-sdk-dotnet-changefeed.md)
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [Node.js](sql-api-sdk-node.md)
> * [异步 Java](sql-api-sdk-async-java.md)
> * [Java](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/cosmos-db/)
> * [REST 资源提供程序](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/)
> * [SQL](sql-api-query-reference.md)
> * [批量执行程序 - .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [批量执行程序 - Java](sql-api-sdk-bulk-executor-java.md)

| |  |
|---|---|
| **说明**| 批量执行程序库允许客户端应用程序在 Azure Cosmos DB 帐户中执行批量操作。 批量执行程序库提供了 BulkImport、BulkUpdate 和 BulkDelete 命名空间。 BulkImport 模块可以批量以优化方式引入文档，以便最大程度地使用为集合配置的吞吐量。 BulkUpdate 模块可以作为修补程序批量更新 Azure Cosmos DB 容器中的现有数据。 BulkDelete 模块可以批量以优化方式删除文档，以便最大程度地使用为集合配置的吞吐量。|
|**SDK 下载**| [NuGet](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.BulkExecutor/) |
| **GitHub 中的 BulkExecutor 库**| [GitHub](https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started)|
|**API 文档**|[ 参考文档](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor?view=azure-dotnet)|
|**入门**|[批量执行程序库 .NET SDK 入门](bulk-executor-dot-net.md)|
| **当前受支持的框架**| Azure .NET Framework 4.5.2、4.6.1 和 .NET Standard 2.0 |

<!-- Not Available on ## Release notes-->
<!-- Update_Description: update meta properties, wording update -->
## <a name="next-steps"></a>后续步骤

若要了解批量执行程序 Java 库，请参阅以下文章：

[Java 批量执行程序库 SDK 和发行信息](sql-api-sdk-bulk-executor-java.md)

<!--Update_Description: update meta properties, wording update-->