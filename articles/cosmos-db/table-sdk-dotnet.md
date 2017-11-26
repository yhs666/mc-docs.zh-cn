---
title: "Azure CosmosDB 表 API .NET SDK 和资源 | Azure"
description: "了解有关 Azure Cosmos DB 表 API 的全部信息，包括发布日期、停用日期和各版本之间进行的更改。"
services: cosmos-db
documentationcenter: .net
author: rockboyfor
manager: digimobile
editor: cgronlun
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
origin.date: 11/20/2017
ms.date: 11/27/2017
ms.author: v-yeche
ms.openlocfilehash: c83f4072d4a6908ac81f1a7955e2517576431790
ms.sourcegitcommit: 9f55dd21ae5f1183ea2d95fdafc9b3327cdb0996
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2017
---
# <a name="azure-cosmos-db-table-net-api-download-and-release-notes"></a>Azure Cosmos DB 表 .NET API：下载和发行说明
> [!div class="op_single_selector"]
> * [.NET](table-sdk-dotnet.md)
> * [Java](table-sdk-java.md)
> * [Node.js](table-sdk-nodejs.md)
> * [Python](table-sdk-python.md)

|   |   |
|---|---|
|**SDK 下载**|[NuGet](https://aka.ms/acdbtablenuget)|
|**API 文档**|[ 参考文档](https://aka.ms/acdbtableapiref)|
|快速入门|[Azure Cosmos DB：使用表 API 生成 .NET 应用程序](create-table-dotnet.md)|
|**教程**|[Azure Cosmos DB：在 .NET 中使用表 API 进行开发](tutorial-develop-table-dotnet.md)|
|**当前受支持的框架**|[Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)|

> [!IMPORTANT]
> 如果已在预览期间创建表 API 帐户，请创建[新的表 API 帐户](create-table-dotnet.md#create-a-database-account)以使用正式发布的表 API SDK。
>

<!-- Not Avaialble ## Release notes -->
<!-- Not Avaialble ## Release and Retirement dates --》

## <a name="troubleshooting"></a> Troubleshooting

If you get the error 

```
Unable to resolve dependency 'Microsoft.Azure.Storage.Common'. Source(s) used: 'nuget.org', 
'CliFallbackFolder', 'Microsoft Visual Studio Offline Packages', 'Azure Service Fabric SDK'`
```

when attempting to use the Microsoft.Azure.CosmosDB.Table NuGet package, you have two options to fix the issue:

* Use Package Manage Console to install the Microsoft.Azure.CosmosDB.Table package and its dependencies. To do this, type the following in the Package Manager Console for your solution. 
    ```
    Install-Package Microsoft.Azure.CosmosDB.Table -IncludePrerelease
    ```

* Using your preferred Nuget package management tool, install the Microsoft.Azure.Storage.Common Nuget package before installing Microsoft.Azure.CosmosDB.Table.

## FAQ

[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## See also
To learn more about the Azure Cosmos DB Table API, see [Introduction to Azure Cosmos DB Table API](table-introduction.md).

<!-- Update_Description: update meta properties, add Troubleshooting content -->