---
title: Azure Cosmos DB 表 API .NET SDK 和资源
description: 了解有关 Azure Cosmos DB 表 API 的全部信息，包括发布日期、停用日期和各版本之间进行的更改。
author: rockboyfor
ms.author: v-yeche
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.devlang: dotnet
ms.topic: reference
origin.date: 08/17/2018
ms.date: 09/09/2019
ms.openlocfilehash: f9e57f107627e002c2f64afc5e98bdc5eed48d2d
ms.sourcegitcommit: 66192c23d7e5bf83d32311ae8fbb83e876e73534
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/04/2019
ms.locfileid: "70254780"
---
<!--Verify sucessfully-->
# <a name="azure-cosmos-db-table-net-api-download-and-release-notes"></a>Azure Cosmos DB 表 .NET API：下载和发行说明

> [!div class="op_single_selector"]
> * [.NET](table-sdk-dotnet.md)
> * [.NET Standard](table-sdk-dotnet-standard.md)
> * [Java](table-sdk-java.md)
> * [Node.js](table-sdk-nodejs.md)
> * [Python](table-sdk-python.md)

|   |   |
|---|---|
|**SDK 下载**|[NuGet](https://aka.ms/acdbtablenuget)|
|**快速入门**|[Azure Cosmos DB：使用 .NET 和表 API 生成应用](create-table-dotnet.md)|
|**教程**|[Azure Cosmos DB：在 .NET 中使用表 API 进行开发](tutorial-develop-table-dotnet.md)|
|**当前受支持的框架**|[Microsoft .NET Framework 4.5.1](https://www.microsoft.com/download/details.aspx?id=40779)|

> [!IMPORTANT]
> .NET Framework SDK [Microsoft.Azure.CosmosDB.Table](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.Table) 目前处于维护模式，不久将被弃用。 请升级到新的 .NET Standard 库 [Microsoft.Azure.Cosmos.Table](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table) 来继续获取表 API 支持的最新功能。

> 如果已在预览期间创建表 API 帐户，请[新建表 API 帐户](create-table-dotnet.md#create-a-database-account)，这样才能使用正式版表 API SDK。
>

<!--Not Available on ## Release notes-->
<!--Not Available on ## Release and Retirement dates-->

## <a name="troubleshooting"></a>故障排除

如果在尝试使用 Microsoft.Azure.CosmosDB.Table NuGet 包时看到以下错误： 

```
Unable to resolve dependency 'Microsoft.Azure.Storage.Common'. Source(s) used: 'nuget.org', 
'CliFallbackFolder', 'Microsoft Visual Studio Offline Packages', 'Azure Service Fabric SDK'`
```

可以使用下列两种方法之一解决此问题：

* 使用包管理器控制台安装 Microsoft.Azure.CosmosDB.Table 包及其依赖项。 为此，请在解决方案的包管理器控制台中键入以下代码。 

    ```powershell
    Install-Package Microsoft.Azure.CosmosDB.Table -IncludePrerelease
    ```

* 使用首选 NuGet 包管理工具先安装 Microsoft.Azure.Storage.Common NuGet 包，再安装 Microsoft.Azure.CosmosDB.Table。

## <a name="faq"></a>常见问题

[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>另请参阅

若要了解有关 Azure Cosmos DB 表 API 的详细信息，请参阅 [Azure Cosmos DB 表 API 简介](table-introduction.md)。

<!--Update_Description: wording update -->
