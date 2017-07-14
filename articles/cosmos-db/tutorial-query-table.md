---
title: "如何查询 Azure Cosmos DB 中的表数据？ | Azure"
description: "了解如何查询 Azure Cosmos DB 中的表数据"
services: cosmos-db
documentationcenter: 
author: rockboyfor
manager: digimobile
editor: 
tags: 
ms.assetid: 14bcb94e-583c-46f7-9ea8-db010eb2ab43
ms.service: cosmos-db
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
origin.date: 05/10/2017
ms.date: 07/17/2017
ms.author: v-yeche
ms.openlocfilehash: f0996480573ab8e28d08a0a96299147ff2576821
ms.sourcegitcommit: b15d77b0f003bef2dfb9206da97d2fe0af60365a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/07/2017
---
# Azure Cosmos DB：如何使用表 API（预览版）查询表数据？
<a id="azure-cosmos-db-how-to-query-table-data-by-using-the-table-api-preview" class="xliff"></a>

Azure Cosmos DB [表 API](table-introduction.md)（预览版）支持对键/值（表）数据进行 OData 和 [LINQ](https://docs.microsoft.com/zh-cn/rest/api/storageservices/fileservices/writing-linq-queries-against-the-table-service) 查询。  

本文涵盖以下任务： 

> [!div class="checklist"]
> * 使用表 API 查询数据

本文中的查询使用如下示例 `People` 表：

| PartitionKey | RowKey | Email | PhoneNumber |
| --- | --- | --- | --- |
| Harp | Walter | Walter@contoso.com| 425-555-0101 |
| Smith | Ben | Ben@contoso.com| 425-555-0102 |
| Smith | Jeff | Jeff@contoso.com| 425-555-0104 | 

由于 Azure Cosmos DB 兼容 Azure 表存储 API，因此若要详细了解如何使用表 API 进行查询，请参阅[查询表和实体] (https://docs.microsoft.com/zh-cn/rest/api/storageservices/fileservices/querying-tables-and-entities)。 

若要详细了解 Azure Cosmos DB 所提供的高级功能，请参阅 [Azure Cosmos DB：表 API](table-introduction.md)和[在 .NET 中使用表 API 进行开发](tutorial-develop-table-dotnet.md)。 

## 先决条件
<a id="prerequisites" class="xliff"></a>

若要使这些查询生效，必须拥有 Azure Cosmos DB 帐户，且容器中必须包含实体数据。 没有这些内容？ 请学习 [5 分钟快速入门](create-table-dotnet.md)或[开发人员教程](tutorial-develop-table-dotnet.md)，创建帐户并填充数据库。

## 根据 PartitionKey 和 RowKey 查询
<a id="query-on-partitionkey-and-rowkey" class="xliff"></a>
由于 PartitionKey 和 RowKey 属性构成实体的主键，因此可以使用以下特殊语法来识别实体： 

**查询**

```
https://<mytableendpoint>/People(PartitionKey='Harp',RowKey='Walter')  
```
**结果**

| PartitionKey | RowKey | Email | PhoneNumber |
| --- | --- | --- | --- |
| Harp | Walter | Walter@contoso.com| 425-555-0104 |

或者，可将这些属性指定为 `$filter` 选项的一部分，如以下部分中所示。 请注意，键属性名称和常量值区分大小写。 PartitionKey 和 RowKey 属性皆为 String 类型。 

## 使用 OData 筛选器进行查询
<a id="query-by-using-an-odata-filter" class="xliff"></a>
构造筛选器字符串时，请记住以下规则： 

* 使用由 OData 协议规范定义的逻辑运算符比较属性和值。 请注意，不能将属性与动态值进行比较。 表达式的一侧必须是常量。 
* 必须使用 URL 编码空格分隔属性名称、运算符和常量值。 空格经 URL 编码后为 `%20`。 
* 筛选器字符串的所有部分都区分大小写。 
* 常量值的数据类型必须与属性的类型相同，这样筛选器才能返回有效的结果。 有关支持的属性类型的详细信息，请参阅 [Understanding the Table Service Data Model](https://docs.microsoft.com/zh-cn/rest/api/storageservices/understanding-the-table-service-data-model)（了解表服务数据模型）。 

以下示例查询演示如何使用 OData `$filter` 按 PartitionKey 和 Email 属性进行筛选。

**查询**

```
https://<mytableapi-endpoint>/People()?$filter=PartitionKey%20eq%20'Smith'%20and%20Email%20eq%20'Ben@contoso.com'
```

有关如何对不同数据类型构造筛选表达式的详细信息，请参阅 [Querying Tables and Entities](https://docs.microsoft.com/zh-cn/rest/api/storageservices/querying-tables-and-entities)（查询表和实体）。

**结果**

| PartitionKey | RowKey | Email | PhoneNumber |
| --- | --- | --- | --- |
| Ben |Smith | Ben@contoso.com| 425-555-0102 |

## 使用 LINQ 进行查询
<a id="query-by-using-linq" class="xliff"></a> 
还可使用 LINQ 进行查询，这会转换为相应的 OData 查询表达式。 以下示例演示如何使用 .NET SDK 生成查询。

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

## 后续步骤
<a id="next-steps" class="xliff"></a>

在本教程中已完成以下操作：

> [!div class="checklist"]
> * 已了解如何使用表 API（预览版）进行查询 

现可继续学习下一教程，了解如何全局分发数据。

> [!div class="nextstepaction"]
> [全局分发数据](tutorial-global-distribution-documentdb.md)