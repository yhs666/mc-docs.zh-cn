---
title: Azure PowerShell 脚本 - 获取 Azure Cosmos 帐户中某个容器的属性
description: Azure PowerShell 脚本示例 - 获取 Azure Cosmos 帐户中某个容器的属性
author: rockboyfor
ms.service: cosmos-db
ms.topic: samples
origin.date: 05/086/2019
ms.date: 05/20/2019
ms.author: v-yeche
ms.openlocfilehash: f748b1ef85cf14d1915dce3564a15f3d03a2b63a
ms.sourcegitcommit: 71172ca8af82d93d3da548222fbc82ed596d6256
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2019
ms.locfileid: "65669004"
---
# <a name="get-the-properties-of-a-container-in-an-azure-cosmos-account-using-powershell"></a>使用 PowerShell 获取 Azure Cosmos 帐户中某个容器的属性

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>示例脚本

```powershell
# Get a single Azure Cosmos container in a database
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container5"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName | Select-Object Properties
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
|**Azure 资源组**| |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | 删除资源组，包括所有嵌套的资源。 |
|||

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/)。

可以在 [Azure Cosmos DB PowerShell 脚本](../../../powershell-samples.md)中找到其他 Azure Cosmos DB PowerShell 脚本示例。

<!--Update_Description: new articles on ps container get -->
<!--ms.date: 05/20/2019-->