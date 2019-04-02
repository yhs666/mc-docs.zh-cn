---
title: Azure Cosmos DB 表 API .NET Standard SDK 和资源
description: 了解有关 Azure Cosmos DB 表 API 和 .NET Standard SDK 的所有信息，包括发布日期、停用日期和各版本之间所做的更改。
author: rockboyfor
ms.author: v-yeche
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.devlang: dotnet
ms.topic: reference
origin.date: 10/18/2018
ms.date: 03/18/2019
ms.openlocfilehash: ff02f25253cdefc2ca44fb8f896c1e99ab4dafbd
ms.sourcegitcommit: b8fb6890caed87831b28c82738d6cecfe50674fd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/29/2019
ms.locfileid: "58626232"
---
<!--Verify sucessfully-->
# <a name="azure-cosmos-db-table-net-standard-api-download-and-release-notes"></a>Azure Cosmos DB 表 .NET Standard API：下载和发行说明
> [!div class="op_single_selector"]
> 
> * [.NET](table-sdk-dotnet.md)
> * [.NET Standard](table-sdk-dotnet-standard.md)
> * [Java](table-sdk-java.md)
> * [Node.js](table-sdk-nodejs.md)
> * [Python](table-sdk-python.md)

|   |   |
|---|---|
|**SDK 下载**|[NuGet](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table)|
|**当前受支持的框架**|[Microsoft .NET Standard 2.0](https://www.nuget.org/packages/NETStandard.Library)|

## <a name="release-notes"></a>发行说明

### <a name="a-name0110-preview0110-preview"></a><a name="0.11.0-preview"/>0.11.0-preview
* 对 CloudTableClient 的配置方式进行了更改。 它现在会在构造过程中使用 TableClientConfiguration 对象。 TableClientConfiguration 提供不同的属性来配置客户端行为，具体取决于目标终结点是 Cosmos DB 表 API 还是 Azure 存储表 API。
* 增加了对 TableQuery 的支持，可以在自定义列中按排序顺序返回结果。 只有 Cosmos DB 表终结点支持此功能。
* 增加了相关支持，可以在不同的结果类型上公开 RequestCharge。 只有 Cosmos DB 表终结点支持此功能。

### <a name="a-name0101-preview0101-preview"></a><a name="0.10.1-preview"/>0.10.1 预览版
* 针对 Azure 存储表终结点添加了对 SAS 令牌以及 TablePermissions、ServiceProperties 和 ServiceStats 的操作的支持。 
   > [!NOTE]
   > 尚不支持以前的 Azure 存储表 SDK 中的某些功能，例如客户端加密。

### <a name="a-name0100-preview0100-preview"></a><a name="0.10.0-preview"/>0.10.0 预览版
* 针对 Azure 存储表终结点添加了对核心 CRUD、批处理和查询操作的支持。 
   > [!NOTE]
   > 尚不支持以前的 Azure 存储表 SDK 中的某些功能，例如客户端加密。

### <a name="a-name091-preview091-preview"></a><a name="0.9.1-preview"/>0.9.1 预览版
* Azure Cosmos DB 表 .NET Standard SDK 是一个跨平台 .NET 库，可高效访问 Cosmos DB 上的表数据模型。 此初始版本支持完整的表和实体 CRUD + 查询功能集，其中 API 与[用于 .NET Framework 的 Cosmos DB 表 SDK](table-sdk-dotnet.md) 类似。 
   > [!NOTE]
   >  0.9.1 预览版尚不支持 Azure 存储表终结点。

## <a name="release-and-retirement-dates"></a>发布日期和停用日期
Azure 会在停用 SDK 时至少提前 12 个月发出通知，以便用户顺利转换为更高版本/受支持版本。

| 版本 | 发布日期 | 停用日期 |
| --- | --- | --- |
| [0.11.0-preview](#0.11.0-preview) |2019 年 3 月 5 日 |--- |
| [0.10.1 预览版](#0.10.1-preview) |2019 年 1 月 22 日 |--- |
| [0.10.0 预览版](#0.10.0-preview) |2018 年 12 月 18 日 |--- |
| [0.9.1 预览版](#0.9.1-preview) |2018 年 10 月 18 日 |--- |

## <a name="faq"></a>常见问题

[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>另请参阅
若要了解有关 Azure Cosmos DB 表 API 的详细信息，请参阅 [Azure Cosmos DB 表 API 简介](table-introduction.md)。

<!--Update_Description: new articles on table sdk dotnet standard -->
<!--ms.date: 03/18/2019-->