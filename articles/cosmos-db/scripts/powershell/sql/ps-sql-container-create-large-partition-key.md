---
title: Azure PowerShell 脚本 - 在 Azure Cosmos 帐户中创建具有大分区键的容器
description: Azure PowerShell 脚本示例 - 在 Azure Cosmos 帐户中创建具有大分区键的容器
author: rockboyfor
ms.service: cosmos-db
ms.topic: sample
origin.date: 07/03/2019
ms.date: 07/29/2019
ms.author: v-yeche
ms.openlocfilehash: 69b1e028c9d930713caaacbd626164979c9324f8
ms.sourcegitcommit: 021dbf0003a25310a4c8582a998c17729f78ce42
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/26/2019
ms.locfileid: "68514538"
---
# <a name="create-a-container-with-a-large-partition-key-in-an-azure-cosmos-account-using-powershell"></a>使用 PowerShell 在 Azure Cosmos 帐户中创建具有大分区键的容器

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>示例脚本

```powershell
# Create a Cosmos SQL API container with large partition key support (version 2)
$apiVersion = "2015-04-08"
$location = "China North 2"
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container1"
$databaseResourceName = $accountName + "/sql/" + $databaseName
$containerResourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName
$accountResourceType = "Microsoft.DocumentDb/databaseAccounts"
$databaseResourceType = "Microsoft.DocumentDb/databaseAccounts/apis/databases"
$containerResourceType = "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers"

$locations = @(
    @{ "locationName"="China North 2"; "failoverPriority"=0 },
    @{ "locationName"="China East 2"; "failoverPriority"=1 }
)

$CosmosDBProperties = @{
    "databaseAccountOfferType"="Standard";
    "locations"=$locations
}

New-AzResource -ResourceType $accountResourceType `
    -ApiVersion $apiVersion -ResourceGroupName $resourceGroupName -Location $location `
    -Name $accountName -PropertyObject $CosmosDBProperties

#Database
$databaseProperties = @{
    "resource"=@{ "id"=$databaseName };
    "options"=@{ "Throughput"= 400 }
} 
New-AzResource -ResourceType $databaseResourceType `
    -ApiVersion $apiVersion -ResourceGroupName $resourceGroupName `
    -Name $databaseResourceName -PropertyObject $databaseProperties

# Container with large partition key support (version = 2)
$containerProperties = @{
    "resource"=@{
        "id"=$containerName; 
        "partitionKey"=@{
            "paths"=@("/myPartitionKey"); 
            "kind"="Hash";
            "version" = 2
        }; 
        "indexingPolicy"=@{
            "indexingMode"="Consistent"; 
            "includedPaths"= @(@{
                "path"="/*"
            });
            "excludedPaths"= @(@{
                "path"="/myPathToNotIndex/*"
            })
        }
    }
} 

New-AzResource -ResourceType $containerResourceType `
    -ApiVersion $apiVersion -ResourceGroupName $resourceGroupName `
    -Name $containerResourceName -PropertyObject $containerProperties

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
|**Azure 资源**| |
| [New-AzResource](https://docs.microsoft.com/powershell/module/az.resources/new-azresource) | 创建资源。 |
|**Azure 资源组**| |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | 删除资源组，包括所有嵌套的资源。 |
|||

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/)。

可以在 [Azure Cosmos DB PowerShell 脚本](../../../powershell-samples.md)中找到其他 Azure Cosmos DB PowerShell 脚本示例。

<!-- Update_Description: new article about ps sql create large partition key -->
<!--ms.date: 07/29/2019-->