---
title: Azure PowerShell 脚本 - 更新 Azure Cosmos 帐户
description: Azure PowerShell 脚本示例 - 使用添加的区域更新 Azure Cosmos 帐户
author: rockboyfor
ms.service: cosmos-db
ms.topic: samples
origin.date: 05/06/2019
ms.date: 06/03/2019
ms.author: v-yeche
ms.openlocfilehash: 4d0884be7eb50ae56bcffd1dd5e6e51cbc50be55
ms.sourcegitcommit: 10458f9a72d4648fd5c9953136bb9581bb216015
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66424292"
---
# <a name="update-an-azure-cosmos-account-and-add-a-region-using-powershell"></a>使用 PowerShell 更新 Azure Cosmos 帐户并添加区域

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>示例脚本

```powershell
# Update an Azure Cosmos Account and add a region
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

$account = Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName

$locations = @(
    @{ "locationName"="China North"; "failoverPriority"=0 },
    @{ "locationName"="China East"; "failoverPriority"=1 },
    @{ "locationName"="China East 2"; "failoverPriority"=2 }
)

$account.Properties.locations = $locations
$CosmosDBProperties = $account.Properties

Set-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName -PropertyObject $CosmosDBProperties
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
| [Get-AzResource](https://docs.microsoft.com/powershell/module/az.resources/get-azresource) | 获取资源。 |
| [Set-AzResource](https://docs.microsoft.com/powershell/module/az.resources/set-azresource) | 更新资源。 |
|**Azure 资源组**| |
| [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) | 创建用于存储所有资源的资源组。 |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | 删除资源组，包括所有嵌套的资源。 |
|||

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/)。

可以在 [Azure Cosmos DB PowerShell 脚本](../../../powershell-samples.md)中找到其他 Azure Cosmos DB PowerShell 脚本示例。

<!--Update_Description: new articles on ps account update -->
<!--ms.date: 06/03/2019-->