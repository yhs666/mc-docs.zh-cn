---
title: Azure PowerShell 脚本 - 更改 Azure Cosmos 帐户的故障转移优先级
description: Azure PowerShell 脚本示例 - 更改 Azure Cosmos 帐户的故障转移优先级
author: rockboyfor
ms.service: cosmos-db
ms.topic: samples
origin.date: 05/06/2019
ms.date: 05/20/2019
ms.author: v-yeche
ms.openlocfilehash: 21d0e91b77b3f059b409e4a0b34340b170dfe415
ms.sourcegitcommit: 71172ca8af82d93d3da548222fbc82ed596d6256
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2019
ms.locfileid: "65669064"
---
# <a name="change-failover-priority-for-an-azure-cosmos-account-using-powershell"></a>使用 PowerShell 更改 Azure Cosmos 帐户的故障转移优先级

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>示例脚本

```powershell
# Change the failover priority for an Azure Cosmos Account
# Assume China North = 0 and China East = 1, the script below will flip them
# Updating location with failoverPriority = 0 will trigger a failover
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

$failoverPolicies = @(
    @{ "locationName"="China East"; "failoverPriority"=0 },
    @{ "locationName"="China North"; "failoverPriority"=1 }
)

Invoke-AzResourceAction -Action failoverPriorityChange `
    -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" `
    -ResourceGroupName $resourceGroupName -Name $accountName -Parameters $failoverPolicies
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
| [Invoke-AzResourceAction](https://docs.microsoft.com/powershell/module/az.resources/invoke-azresourceaction) | 针对资源调用操作。 |
|**Azure 资源组**| |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | 删除资源组，包括所有嵌套的资源。 |
|||

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/)。

可以在 [Azure Cosmos DB PowerShell 脚本](../../../powershell-samples.md)中找到其他 Azure Cosmos DB PowerShell 脚本示例。

<!--Update_Description: new articles on ps account failover priority update -->
<!--ms.date: 05/20/2019-->
