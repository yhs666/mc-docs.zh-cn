---
title: Azure PowerShell 脚本 - Azure Cosmos DB 更新 MongoDB API 的 RU/秒
description: Azure PowerShell 脚本 - Azure Cosmos DB 更新 MongoDB API 的 RU/秒
author: rockboyfor
ms.service: cosmos-db
ms.topic: samples
origin.date: 05/18/2019
ms.date: 06/17/2019
ms.author: v-yeche
ms.openlocfilehash: 64bffa6b06f85ae8e842d9f6996b3c3472fd9b95
ms.sourcegitcommit: 43eb6282d454a14a9eca1dfed11ed34adb963bd1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/17/2019
ms.locfileid: "67151367"
---
# <a name="update-rus-for-a-database-or-collection-for-azure-cosmos-db---mongodb-api"></a>更新 Azure Cosmos DB 的数据库或集合的 RU/秒 - MongoDB API

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>示例脚本

```powershell
# Update RU for an Azure Cosmos MongoDB API database or collection
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$collectionName = "collection1"
$databaseResourceName = $accountName + "/mongodb/" + $databaseName + "/throughput"
$collectionResourceName = $accountName + "/mongodb/" + $databaseName + "/" + $collectionName + "/throughput"
$throughput = 500
$updateResource = "database" # or "collection"

$properties = @{
    "resource"=@{"throughput"=$throughput}
}

if($updateResource -eq "database"){
    Set-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/settings" `
        -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
        -Name $databaseResourceName -PropertyObject $properties
}
elseif($updateResource -eq "collection"){
    Set-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/collections/settings" `
        -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
        -Name $collectionResourceName -PropertyObject $properties
}
else {
    Write-Host("Must select database or collection")    
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

<!--Update_Description: new articles on ps mongodb ru update -->
<!--ms.date: 06/03/2019-->