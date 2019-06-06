---
title: Azure PowerShell 脚本 - Azure Cosmos DB 更新 Cassandra API 的 RU/秒
description: Azure PowerShell 脚本 - Azure Cosmos DB 更新 Cassandra API 的 RU/秒
author: rockboyfor
ms.service: cosmos-db
ms.topic: samples
origin.date: 05/18/2019
ms.date: 06/03/2019
ms.author: v-yeche
ms.openlocfilehash: 36e280fe723fbd809e49059fefcf610fe07a3b4d
ms.sourcegitcommit: 10458f9a72d4648fd5c9953136bb9581bb216015
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66424245"
---
# <a name="update-rus-for-a-keyspace-or-table-for-azure-cosmos-db---cassandra-api"></a>更新 Azure Cosmos DB 的密钥空间或表的 RU/秒 - Cassandra API

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>示例脚本

```powershell
# Update RU for an Azure Cosmos Cassandra API keyspace or table
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$keyspaceName = "keyspace1"
$tableName = "table1"
$keyspaceResourceName = $accountName + "/cassandra/" + $keyspaceName + "/throughput"
$tableResourceNAme = $accountName + "/cassandra/" + $keyspaceName + "/" + $tableName + "/throughput"
$throughput = 500
$updateResource = "keyspace" # or "table"

$properties = @{
    "resource"=@{"throughput"=$throughput}
}

if($updateResource -eq "keyspace"){
    Set-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/keyspaces/settings" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $keyspaceResourceName -PropertyObject $properties
}
elseif($updateResource -eq "table"){
Set-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/keyspaces/tables/settings" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $tableResourceNAme -PropertyObject $tableProperties
}
else {
    Write-Host("Must select keyspace or table")    
}

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

<!--Update_Description: new articles on ps cassandra ru update -->
<!--ms.date: 06/03/2019-->