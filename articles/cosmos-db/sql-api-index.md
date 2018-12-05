---
title: Azure Cosmos DB：SQL API 文章 | Azure
description: 关于如何在 Azure Cosmos DB 中使用 SQL API 创建文档数据库的所有文章的列表。
services: cosmos-db
author: rockboyfor
manager: digimobile
documentationcenter: ''
ms.assetid: 82bec99a-ac2b-474e-b41f-d2fb296c8feb
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 10/25/2017
ms.date: 04/23/2018
ms.author: v-yeche
ms.openlocfilehash: 18d60d3b1fbfbe0b3a0d01c072b047b81da8aac0
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52662310"
---
# <a name="azure-cosmos-db-sql-api-documentation"></a>Azure Cosmos DB：SQL API 文档

本文提供了特定于 SQL API 的所有 Azure Cosmos DB 内容的链接。

这些文章不适用于 MongoDB API。 
<!--Not Available on Graph API -->
<!--Not Available on Table API -->

## <a name="introduction-and-concepts"></a>简介和概念

若要开始了解用于 Azure Cosmos DB 的 SQL API，这些是必读的主题和资源。

- [SQL API 简介](sql-api-introduction.md)
- [数据库资源模型](sql-api-resources.md)
- 网站：[Query playground](https://www.documentdb.com/sql/demo)
<!-- Not Available on - Cheat sheet: [SQL grammar](query-cheat-sheet.md)-->（查询板块）

## <a name="quickstarts"></a>快速入门

快速入门主题是使用 Azure Cosmos DB 创建可工作的应用程序的最快捷方法。 在每个快速入门中，将了解如何使用基于 UI 的 Azure 门户和常用编程语言创建使用 Azure Cosmos DB 的数据库解决方案。 可从 GitHub 克隆的 Web 应用可用于每个快速入门。 

- [.NET + Azure 门户 + Web 应用](create-sql-api-dotnet.md)
- [Java + Azure 门户 + Web 应用](create-sql-api-java.md)
- [Node.js + Azure 门户 + Web 应用](create-sql-api-nodejs.md)
- [Python + Azure 门户 + Web 应用](create-sql-api-python.md)
<!--Not Available - [Xamarin + .NET + Azure portal + Web apps](create-sql-api-xamarin-dotnet.md)-->

## <a name="tutorials"></a>教程

这些教程的级别比快速入门更深。 在这些教程中，将从头开始构建应用，并将代码复制并粘贴到应用中。 还将学习如何导入数据、查询数据和全局分发数据库。

### <a name="create-a-web-app"></a>创建 Web 应用

- [.NET](sql-api-dotnet-application.md)
- [Node.js](sql-api-nodejs-application.md) 
- [Java](sql-api-java-application.md)
- [Python](sql-api-python-application.md)

### <a name="create-a-console-app"></a>创建控制台应用

- [.NET](sql-api-get-started.md)
- [.NET Core](sql-api-dotnetcore-get-started.md) 
- [Java](sql-api-java-get-started.md) 
- [Node.js](sql-api-nodejs-get-started.md) 
- [C++](sql-api-cpp-get-started.md)

### <a name="create-a-mobile-app"></a>创建移动应用

- [Xamarin](mobile-apps-with-xamarin.md)

### <a name="work-with-data"></a>处理数据

- [导入数据](import-data.md)
- [查询](tutorial-query-sql-api.md)
- [全球分配数据](tutorial-global-distribution-sql-api.md)
<!-- Not Available ### Work with Azure Functions -->
<!-- Not Available - [Create an Azure Cosmos DB trigger](../azure-functions/functions-create-cosmos-db-triggered-function.md) -->
<!-- Not Available - [Use Azure Cosmos DB in input and output bindings](../azure-functions/functions-integrate-store-unstructured-data-cosmosdb.md) -->

## <a name="developers-guide"></a>开发人员指南

- [SQL 查询](sql-api-sql-query.md)
- [分区](sql-api-partition-data.md)
- [服务器端编程：存储过程、数据库触发器和 UDF](programming.md)
- [性能测试](performance-testing.md)
- [性能提示](performance-tips.md)
- [多主设置](multi-region-writers.md)
- [日期时间](working-with-dates.md)
- [文档数据建模](modeling-data.md) 

## <a name="sdks"></a>SDK

Azure Cosmos DB 提供许多 SDK 来支持客户端应用程序开发。

- [Java](sql-api-sdk-java.md)
- [.NET](sql-api-sdk-dotnet.md)
- [.NET 更改源](sql-api-sdk-dotnet-changefeed.md)
- [.NET Core](sql-api-sdk-dotnet-core.md)
- [Node.js](sql-api-sdk-node.md)
- [Python](sql-api-sdk-python.md)

## <a name="reference"></a>参考

- [REST](https://docs.microsoft.com/rest/api/cosmos-db/)
- [REST 资源提供程序](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/)
- [SQL 查询参考](sql-api-sql-query-reference.md)
<!-- Not Availalbe on - [Azure Functions reference](../azure-functions/functions-bindings-cosmosdb.md) -->

## <a name="samples"></a>示例

这些示例页提供了最常见 SQL API 任务的示例代码和 API 参考内容的链接。

- [.NET](sql-api-dotnet-samples.md)
- [Node.js](sql-api-nodejs-samples.md)
- [Python](sql-api-python-samples.md)

<!-- Update_Description: update meta properties, wording update -->