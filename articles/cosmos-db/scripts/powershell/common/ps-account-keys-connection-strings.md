---
title: Azure PowerShell 脚本 - Azure Cosmos 帐户的帐户密钥和连接字符串操作
description: Azure PowerShell 脚本示例 - Azure Cosmos 帐户的帐户密钥和连接字符串操作
author: rockboyfor
ms.service: cosmos-db
ms.topic: samples
origin.date: 05/20/2019
ms.date: 06/03/2019
ms.author: v-yeche
ms.openlocfilehash: 3ca00d5ea136963ade3a21055ab0327b839636ae
ms.sourcegitcommit: 10458f9a72d4648fd5c9953136bb9581bb216015
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66424285"
---
# <a name="connection-string-and-account-key-operations-for-an-azure-cosmos-account-using-powershell"></a>使用 PowerShell 执行 Azure Cosmos 帐户的连接字符串和帐户密钥操作

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>示例脚本

此示例需要资源组和帐户存在。 首先使用现有的 PowerShell 创建示例来预配帐户。

```powershell
# Account keys and connection string operations for Azure Cosmos account

$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$keyKind = @{ "keyKind"="Primary" }

Read-Host -Prompt "List connection strings for an Azure Cosmos Account"

Invoke-AzResourceAction -Action listConnectionStrings `
    -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" `
    -ResourceGroupName $resourceGroupName -Name $accountName | Select-Object *

Read-Host -Prompt "List keys for an Azure Cosmos Account"

Invoke-AzResourceAction -Action listKeys `
    -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" `
    -ResourceGroupName $resourceGroupName -Name $accountName | Select-Object *

Read-Host -Prompt "Regenerate the primary key for an Azure Cosmos Account"

$keys = Invoke-AzResourceAction -Action regenerateKey `
    -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" `
    -ResourceGroupName $resourceGroupName -Name $accountName -Parameters $keyKind

Write-Host $keys

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

<!--Update_Description: new articles on ps account keys connection string -->
<!--ms.date: 06/03/2019-->