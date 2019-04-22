---
title: Azure PowerShell 脚本 - Azure Cosmos DB 的多区域复制
description: Azure PowerShell 脚本示例 - Azure Cosmos DB 的多区域复制
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
author: rockboyfor
ms.author: v-yeche
ms.devlang: PowerShell
ms.topic: sample
origin.date: 05/10/2017
ms.date: 04/15/2019
ms.reviewer: sngun
ms.openlocfilehash: 0b0a10bf1e20b2cacc277e06a91746305c5cff15
ms.sourcegitcommit: f85e05861148b480d6c9ea95ce84a17145872442
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/16/2019
ms.locfileid: "59615260"
---
# <a name="replicate-an-azure-cosmos-db-database-account-in-multiple-regions-and-configure-failover-priorities-using-powershell"></a>使用 PowerShell 将 Azure Cosmos DB 数据库帐户复制到多个区域中并配置故障转移优先级

此示例使用 PowerShell 将任何类型的 Azure Cosmos DB 数据库帐户复制到多个区域中并配置故障转移优先级。 

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>示例脚本

<!--First make chinaeast2 as wirte region-->
<!--Second make chinanorth as write region-->
<!--Last add chinanorth2 new region-->

```powershell
# Set the Azure resource group name and location
$resourceGroupName = "myResourceGroup"
$resourceGroupLocation = "chinaeast2"

# Database name
$DBName = "testdb"
# Distribution locations
$locations = @(@{"locationName"="chinaeast"; 
                 "failoverPriority"=2},
               @{"locationName"="chinanorth"; 
                 "failoverPriority"=1},
               @{"locationName"="chinaeast2"; 
                 "failoverPriority"=0})


# Create the resource group
New-AzResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation

# Consistency policy
$consistencyPolicy = @{"maxIntervalInSeconds"="10"; 
                       "maxStalenessPrefix"="200"}

# DB properties
$DBProperties = @{"databaseAccountOfferType"="Standard";
                  "Kind"="GlobalDocumentDB"; 
                  "locations"=$locations; 
                  "consistencyPolicy"=$consistencyPolicy;}

# Create the database
New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
                    -ApiVersion "2015-04-08" `
                    -ResourceGroupName $resourceGroupName `
                    -Location $resourceGroupLocation `
                    -Name $DBName `
                    -PropertyObject $DBProperties

# Update failoverpolicy to make China North as a write region
$NewfailoverPolicies = @(@{"locationName"="chinanorth"; "failoverPriority"=0}, @{"locationName"="chinaeast2"; "failoverPriority"=1}, @{"locationName"="chinaeast"; "failoverPriority"=2} )

Invoke-AzResourceAction `
    -Action failoverPriorityChange `
    -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" `
    -ResourceGroupName $resourceGroupName `
    -Name $DBName `
    -Parameters @{"failoverPolicies"=$NewfailoverPolicies}

# Add a new locations with priorities
$newLocations = @(@{"locationName"="chinanorth"; 
                 "failoverPriority"=0},
               @{"locationName"="chinaeast"; 
                 "failoverPriority"=1},
               @{"locationName"="chinanorth2"; 
                 "failoverPriority"=2},
               @{"locationName"="chinaeast2";
                 "failoverPriority"=3})

# Updated properties
$updateDBProperties = @{"databaseAccountOfferType"="Standard";
                        "locations"=$newLocations;}

# Update the database with the properties
Set-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" `
    -ResourceGroupName $resourceGroupName `
    -Name $DBName `
    -PropertyObject $UpdateDBProperties

```

## <a name="clean-up-deployment"></a>清理部署

运行脚本示例后，可以使用以下命令删除资源组以及与其关联的所有资源。

```powershell
Remove-AzResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) | 创建用于存储所有资源的资源组。 |
| [New-AzResource](https://docs.microsoft.com/powershell/module/az.resources/new-azresource) | 创建用于托管数据库或弹性池的逻辑服务器。 |
| [Set-AzResource](https://docs.microsoft.com/powershell/module/az.resources/set-azresource) | 修改数据库帐户。 |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | 删除资源组，包括所有嵌套的资源。 |
|||

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/)。

可以在 [Azure Cosmos DB PowerShell 脚本](../powershell-samples.md)中找到其他 Azure Cosmos DB PowerShell 脚本示例。

<!-- Update_Description: update meta properties -->