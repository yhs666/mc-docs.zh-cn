---
title: Azure Cosmos DB：批量执行程序 .NET API、SDK 和资源 | Azure
description: 了解有关批量执行程序 .NET API 和 SDK 的所有信息，包括发布日期、停用日期和 Azure Cosmos DB 批量执行程序 .NET SDK 各版本之间所做的更改。
services: cosmos-db
author: rockboyfor
manager: digimobile
editor: cgronlun
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: dotnet
ms.topic: reference
origin.date: 09/14/2018
ms.date: 11/05/2018
ms.author: v-yeche
ms.openlocfilehash: 5f14f7621a0e172ee8fb9fbdec4aa0bd199b5039
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52659257"
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
> * [SQL](../cosmos-db/sql-api-sql-query-reference.md)
> * [批量执行程序 - .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [批量执行程序 - Java](sql-api-sdk-bulk-executor-java.md)

<table>

<tr><td>**说明**</td><td>批量执行程序库允许客户端应用程序在 Azure Cosmos DB 帐户中执行批量操作。 批量执行程序库提供 BulkImport、BulkUpdate 和 BulkDelete 命名空间。 BulkImport 模块可以批量以优化方式引入文档，以便最大程度地使用为集合配置的吞吐量。 BulkUpdate 模块可以作为修补程序批量更新 Azure Cosmos DB 容器中的现有数据。 BulkDelete 模块可以批量以优化方式删除文档，以便最大程度地使用为集合配置的吞吐量。</td></tr>

<tr><td>**SDK 下载**</td><td>[NuGet](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.BulkExecutor/)</td></tr>

<tr><td>**GitHub 中的 BulkExecutor 库**</td><td>[GitHub](https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started)</td></tr>

<tr><td>**API 文档**</td><td>[.Net API 参考文档](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor?view=azure-dotnet)</td></tr>

<tr><td>**入门**</td><td>[批量执行程序库 .NET SDK 入门](bulk-executor-dot-net.md)</td></tr>

<tr><td>**当前受支持的框架**</td><td><ul><li>[Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/)（版本 >= 2.0.0）</li><li>
[Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/)（版本 >= 9.0.1）
</li></ul></td></tr>
</table></br>

<!-- Not Available on ## Release notes-->
<!-- Update_Description: update meta properties, wording update -->
