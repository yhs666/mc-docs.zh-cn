---
title: Azure PowerShell 脚本 - 创建 Azure Cosmos DB Cassandra API 帐户
description: Azure PowerShell 脚本示例 - 创建 Azure Cosmos DB Cassandra API 帐户
author: rockboyfor
ms.author: v-yeche
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.devlang: PowerShell
ms.topic: sample
origin.date: 08/08/2018
ms.date: 04/15/2019
ms.reviewer: sngun
ms.openlocfilehash: 97cdd844e7e14fd465d81e6d60e792251813ce71
ms.sourcegitcommit: f85e05861148b480d6c9ea95ce84a17145872442
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/16/2019
ms.locfileid: "59615180"
---
<!--Verify sucessfully-->
# <a name="azure-cosmos-db-create-a-cassandra-api-account-using-powershell"></a>Azure Cosmos DB：使用 PowerShell 创建 Cassandra API 帐户

此示例 PowerShell 脚本创建一个 Azure Cosmos DB Cassandra API 帐户。 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>示例脚本

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

```powershell
# Set the Azure resource group name and location
$resourceGroupName = "sparkcdb1"
$resourceGroupLocation = "China East 2"

# Create the resource group
New-AzResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation

# Database name
$DBName = "testcassandra"

# Write and read locations and priorities for the database
$locations = @(@{"locationName"="China East 2"; 
                 "failoverPriority"=0}, 
               @{"locationName"="China North"; 
                  "failoverPriority"=1})

# IP addresses that can access the database through the firewall
$iprangefilter = "10.0.0.1"

# Consistency policy
$consistencyPolicy = @{"defaultConsistencyLevel"="BoundedStaleness";
                       "maxIntervalInSeconds"="10"; 
                       "maxStalenessPrefix"="200"}

# Create an Azure Cosmos DB Cassandra API account
$Capability= "EnableCassandra"

$capabilities= @(@{"name"=$Capability})

# DB properties
$DBProperties = @{"databaseAccountOfferType"="Standard"; 
                          "locations"=$locations; 
                          "consistencyPolicy"=$consistencyPolicy;
                          "capabilities"=$capabilities; 
                          "ipRangeFilter"=$iprangefilter}

# Create the database
New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
                    -ApiVersion "2015-04-08" `
                    -ResourceGroupName $resourceGroupName `
                    -Location $resourceGroupLocation `
                    -Name $DBName `
                    -PropertyObject $DBProperties

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
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | 删除资源组，包括所有嵌套的资源。 |
|||

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/)。

可以在 [Azure Cosmos DB PowerShell 脚本](../powershell-samples.md)中找到其他 Azure Cosmos DB PowerShell 脚本示例。

<!--Update_Description: update meta properties, wording update-->