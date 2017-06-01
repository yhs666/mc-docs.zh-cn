---
title: "如何查询 Azure Cosmos DB 中的表数据？ | Azure"
description: "了解如何查询 Azure Cosmos DB 中的表数据"
services: cosmosdb
documentationcenter: 
author: kanshiG
manager: jhubbard
editor: 
tags: 
ms.assetid: 14bcb94e-583c-46f7-9ea8-db010eb2ab43
ms.service: cosmosdb
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/10/2017
wacn.date: 
ms.author: govindk
ms.translationtype: Human Translation
ms.sourcegitcommit: 4a18b6116e37e365e2d4c4e2d144d7588310292e
ms.openlocfilehash: 7b710e4e56e57960123d3f5ffa0e9ddf94a8fc49
ms.contentlocale: zh-cn
ms.lasthandoff: 05/19/2017


---

# <a name="azure-cosmos-db-how-to-query-with-the-table-api-preview"></a>Azure Cosmos DB：如何使用表 API（预览版）进行查询？

Azure Cosmos DB [表 API](table-introduction.md)（预览版）支持对键/值（表）数据进行 OData 和 [LINQ](https://docs.microsoft.com/rest/api/storageservices/fileservices/writing-linq-queries-against-the-table-service) 查询。  

本文涵盖以下任务： 

> [!div class="checklist"]
> * 使用表 API 查询数据

## <a name="sample-table"></a>示例表

本文中的查询使用如下示例 `People` 表：

| PartitionKey | RowKey | Email | PhoneNumber |
| --- | --- | --- | --- |
| Harp | Walter | Walter@contoso.com| 425-555-0101 |
| Smith | Walter | Ben@contoso.com| 425-555-0102 |
| Smith | Jeff | Jeff@contoso.com| 425-555-0104 | 

## <a name="about-the-table-api-preview"></a>关于表 API（预览版）

由于 Azure Cosmos DB 兼容 Azure 表存储 API，因此若要详细了解如何使用 Table API 进行查询，请参阅[查询表和实体] (https://docs.microsoft.com/rest/api/storageservices/fileservices/querying-tables-and-entities)。 

若要详细了解 Azure Cosmos DB 所提供的高级功能，请参阅 [Azure Cosmos DB：表 API](table-introduction.md)和[使用 .NET 通过表 API 进行开发](tutorial-develop-table-dotnet.md)。 

## <a name="prerequisites"></a>先决条件

若要使这些查询生效，必须拥有 Azure Cosmos DB 帐户，且容器中必须包含实体数据。 没有这些内容？ 请学习 [5 分钟快速入门](https://aka.ms/acdbtnetqs)或[开发人员教程](https://aka.ms/acdbtabletut)，创建帐户并填充数据库。

## <a name="querying-on-partition-key-and-row-key"></a>按分区键和行健查询
由于 PartitionKey 和 RowKey 属性构成实体的主键，因此可使用特殊语法确定实体，如下所示： 

**查询**

```
https://<mytableendpoint>/People(PartitionKey='Harp',RowKey='Walter')  
```
**结果**

| PartitionKey | RowKey | Email | PhoneNumber |
| --- | --- | --- | --- |
| Harp | Walter | Walter@contoso.com| 425-555-0104 |

或者，可将这些属性指定为 `$filter` 选项的一部分，如以下部分中所示。 请注意，键属性名称和常量值区分大小写。 PartitionKey 和 RowKey 属性皆为 String 类型。 

## <a name="querying-with-an-odata-filter"></a>使用 ODATA 筛选器查询
构造筛选器字符串时，请记住以下规则： 

* 使用由 OData 协议规范定义的逻辑运算符比较属性和值。 请注意不能比较属性和动态值；表达式的一侧必须是常量。 
* 必须使用 URL 编码空格分隔属性名称、运算符和常量值。 空格经 URL 编码后为 `%20`。 
* 筛选器字符串的所有部分都区分大小写。 
* 常量值的数据类型必须与属性的类型相同，这样筛选器才能返回有效的结果。 有关支持的属性类型的详细信息，请参阅 [Understanding the Table Service Data Model](https://docs.microsoft.com/rest/api/storageservices/understanding-the-table-service-data-model)（了解表服务数据模型）。 

以下示例查询演示如何使用 ODATA `$filter` 按 PartitionKey 和 Email 属性进行筛选。

**查询**

```
https://<mytableapi-endpoint>/People()?$filter=PartitionKey%20eq%20'Smith'%20and%20Email%20eq%20'Ben@contoso.com'
```

有关如何对不同数据类型构造筛选表达式的详细信息，请参阅 [Querying Tables and Entities](https://docs.microsoft.com/rest/api/storageservices/querying-tables-and-entities)（查询表和实体）

**结果**

| PartitionKey | RowKey | Email | PhoneNumber |
| --- | --- | --- | --- |
| Ben |Smith | Ben@contoso.com| 425-555-0102 |

## <a name="querying-with-linq"></a>使用 LINQ 进行查询 
还可使用 LINQ 进行查询，这会转换为相应的 ODATA 查询表达式。 以下示例演示如何使用 .NET SDK 生成查询。

```csharp
CloudTableClient tableClient = account.CreateCloudTableClient();
CloudTable table = tableClient.GetTableReference("people");

TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>()
    .Where(
        TableQuery.CombineFilters(
            TableQuery.GenerateFilterCondition(PartitionKey, QueryComparisons.Equal, "Smith"),
            TableOperators.And,
            TableQuery.GenerateFilterCondition(Email, QueryComparisons.Equal,"Ben@contoso.com")
    ));

await table.ExecuteQuerySegmentedAsync<CustomerEntity>(query, null);
```

## <a name="next-steps"></a>后续步骤

在本教程中，已完成以下内容：

> [!div class="checklist"]
> * 已了解如何使用表 API（预览）进行查询 

现可继续学习下一教程，了解如何全局分发数据。

> [!div class="nextstepaction"]
> [全局分发数据](tutorial-global-distribution-documentdb.md)