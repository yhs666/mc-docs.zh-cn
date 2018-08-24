---
title: Azure Cosmos DB：批量执行程序 .NET API、SDK 和资源 | Azure
description: 了解有关批量执行程序 .NET API 的所有信息。
services: cosmos-db
author: rockboyfor
manager: digimobile
editor: cgronlun
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: dotnet
ms.topic: reference
origin.date: 05/07/2018
ms.date: 08/13/2018
ms.author: v-yeche
ms.openlocfilehash: c004307c9fcf2431379ca54553054dd76fa1f793
ms.sourcegitcommit: e3a4f5a6b92470316496ba03783e911f90bb2412
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/10/2018
ms.locfileid: "41705240"
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

<tr><td>**说明**</td><td>批量执行程序库允许客户端应用程序在 Azure Cosmos DB 帐户中执行批量操作。 批量执行程序库提供 BulkImport 和 BulkUpdate 命名空间。 BulkImport 模块可以批量以优化方式引入文档，以便最大程度地使用为集合配置的吞吐量。 BulkUpdate 模块可以作为修补程序批量更新 Azure Cosmos DB 容器中的现有数据。</td></tr>

<tr><td>**SDK 下载**</td><td>[NuGet](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.BulkExecutor/)</td></tr>

<tr><td>**GitHub 中的 BulkExecutor 库**</td><td>[GitHub](https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started)</td></tr>

<tr><td>**API 文档**</td><td>[.Net API 参考文档](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor?view=azure-dotnet)</td></tr>

<tr><td>**入门**</td><td>[批量执行程序库 .NET SDK 入门](bulk-executor-dot-net.md)</td></tr>

<tr><td>**当前受支持的框架**</td><td><ul><li>[Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/)（版本 >= 1.21.1）</li><li>
[Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/)（版本 >= 10.0.2）
</li></ul></td></tr>
</table></br>
<!-- Update_Description: update meta properties, wording update -->
