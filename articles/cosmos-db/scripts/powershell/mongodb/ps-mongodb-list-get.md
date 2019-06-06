---
title: Azure PowerShell 脚本 - Azure Cosmos DB MongoDB API 的列出和获取操作
description: Azure PowerShell 脚本 - Azure Cosmos DB MongoDB API 的列出和获取操作
author: markjbrown
ms.service: cosmos-db
ms.topic: samples
origin.date: 05/18/2019
ms.date: ''
ms.author: v-yeche
ms.openlocfilehash: 417bbbec83ab3ee524d9ec3f5572212941420ce5
ms.sourcegitcommit: 10458f9a72d4648fd5c9953136bb9581bb216015
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66424290"
---
# <a name="list-and-get-databases-and-graphs-for-azure-cosmos-db---mongodb-api"></a>列出和获取 Azure Cosmos DB 的数据库和图 - MongoDB API

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>示例脚本

```powershell
# List and Get operations for Azure Cosmos MongoDB API
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount" # must be lower case.
$databaseName = "database1"
$collectionName = "collection1"
$accountResourceName = $accountName + "/mongodb/"
$databaseResourceName = $accountName + "/mongodb/" + $databaseName
$collectionResourceName = $accountName + "/mongodb/" + $databaseName + "/" + $collectionName

Read-Host -Prompt "List all databases in an account. Press Enter to continue"

Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountResourceName  | Select-Object Properties

Read-Host -Prompt "Get a database in an account. Press Enter to continue"

Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $databaseResourceName | Select-Object Properties

Read-Host -Prompt "List all collections in a database. Press Enter to continue"

Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/collections" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $databaseResourceName | Select-Object Properties

Read-Host -Prompt "Get a collection in a database. Press Enter to continue"

Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/collections" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $collectionResourceName | Select-Object Properties

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

<!--Update_Description: new articles on ps mongodb list get -->
<!--ms.date: 06/03/2019-->