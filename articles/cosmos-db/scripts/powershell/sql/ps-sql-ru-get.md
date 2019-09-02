---
title: Azure PowerShell 脚本 - Azure Cosmos DB 获取预配吞吐量（RU/秒）- SQL (Core) API
description: Azure PowerShell 脚本 - Azure Cosmos DB 获取预配吞吐量（RU/秒）- SQL (Core) API
author: rockboyfor
ms.service: cosmos-db
ms.topic: sample
origin.date: 07/03/2019
ms.date: 07/29/2019
ms.author: v-yeche
ms.openlocfilehash: a24bfda35b9fb9b140bfd1731e8e10426f7ea2df
ms.sourcegitcommit: 021dbf0003a25310a4c8582a998c17729f78ce42
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/26/2019
ms.locfileid: "68514536"
---
# <a name="get-the-provisioned-throughput-rus-for-a-database-or-container-for-azure-cosmos-db---sql-core-api"></a>获取 Azure Cosmos DB 的数据库或容器的预配吞吐量（RU/秒）- SQL (Core) API

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>示例脚本

```powershell
# Get RU for an Azure Cosmos SQL (Core) API database or container
$apiVersion = "2015-04-08"
$resourceGroupName = "myResourceGroup"
$databaseThroughputResourceType = "Microsoft.DocumentDb/databaseAccounts/apis/databases/settings"
$containerThroughputResourceType = "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers/settings"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container1"
$databaseThroughputResourceName = $accountName + "/sql/" + $databaseName + "/throughput"
$containerThroughputResourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName + "/throughput"

# Check if throughput is set at database level (returns RU/s or error)
Get-AzResource -ResourceType $databaseThroughputResourceType `
    -ApiVersion $apiVersion -ResourceGroupName $resourceGroupName `
    -Name $databaseThroughputResourceName  | Select-Object Properties

# Check if throughput is set at container level (returns RU/s or error)
Get-AzResource -ResourceType $containerThroughputResourceType `
    -ApiVersion $apiVersion -ResourceGroupName $resourceGroupName `
    -Name $containerThroughputResourceName  | Select-Object Properties

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

<!-- Update_Description: new article about ps sql ru get-->
<!--ms.date: 07/29/2019-->