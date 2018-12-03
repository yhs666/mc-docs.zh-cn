---
title: Azure PowerShell 脚本 - Azure Cosmos DB 的多区域复制 | Azure
description: Azure PowerShell 脚本示例 - Azure Cosmos DB 的多区域复制
services: cosmos-db
documentationcenter: cosmosdb
author: rockboyfor
manager: digimobile
tags: azure-service-management
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: cosmosdb
ms.workload: database
origin.date: 05/10/2017
ms.date: 09/03/2018
ms.author: v-yeche
ms.openlocfilehash: 7de5926feafbad8d549073244177971d2a4c5f7b
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52662018"
---
# <a name="replicate-an-azure-cosmos-db-database-account-in-multiple-regions-and-configure-failover-priorities-using-powershell"></a>使用 PowerShell 将 Azure Cosmos DB 数据库帐户复制到多个区域中并配置故障转移优先级

此示例使用 PowerShell 将任何类型的 Azure Cosmos DB 数据库帐户复制到多个区域中并配置故障转移优先级。 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>示例脚本

```powershell
# Set the Azure resource group name and location
$resourceGroupName = "myResourceGroup"
$resourceGroupLocation = "chinanorth"

# Database name
$DBName = "testdb"
# Distribution locations
$locations = @(@{"locationName"="chinanorth"; 
                 "failoverPriority"=2},
               @{"locationName"="chinaeast"; 
                 "failoverPriority"=1},
               @{"locationName"="chinanorth2"; 
                 "failoverPriority"=0})


# Create the resource group
New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation

# Consistency policy
$consistencyPolicy = @{"maxIntervalInSeconds"="10"; 
                       "maxStalenessPrefix"="200"}

# DB properties
$DBProperties = @{"databaseAccountOfferType"="Standard";
                  "Kind"="GlobalDocumentDB"; 
                  "locations"=$locations; 
                  "consistencyPolicy"=$consistencyPolicy;}

# Create the database
New-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
                    -ApiVersion "2015-04-08" `
                    -ResourceGroupName $resourceGroupName `
                    -Location $resourceGroupLocation `
                    -Name $DBName `
                    -PropertyObject $DBProperties

# Update failoverpolicy to make China North as a write region
$NewfailoverPolicies = @(@{"locationName"="chinanorth"; "failoverPriority"=0}, @{"locationName"="chinaeast"; "failoverPriority"=1}, @{"locationName"="chinanorth2"; "failoverPriority"=2} )

Invoke-AzureRmResourceAction `
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
Set-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" `
    -ResourceGroupName $resourceGroupName `
    -Name $DBName `
    -PropertyObject $UpdateDBProperties

```

## <a name="clean-up-deployment"></a>清理部署

运行脚本示例后，可以使用以下命令删除资源组以及与其关联的所有资源。

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup) | 创建用于存储所有资源的资源组。 |
| [New-AzureRmResource](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresource?view=azurermps-3.8.0) | 创建用于托管数据库或弹性池的逻辑服务器。 |
| [Set-AzureRMResource](https://docs.microsoft.com/powershell/module/azurerm.resources/set-azurermresource?view=azurermps-3.8.0) | 修改数据库帐户。 |
| [Remove-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/remove-azurermresourcegroup) | 删除资源组，包括所有嵌套的资源。 |
|||

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/)。

可以在 [Azure Cosmos DB PowerShell 脚本](../powershell-samples.md)中找到其他 Azure Cosmos DB PowerShell 脚本示例。
<!-- Update_Description: wording update, update link -->