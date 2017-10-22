---
title: "Azure PowerShell 脚本示例 - 根据前缀删除容器 | Microsoft Docs"
description: "根据容器名称前缀删除 Azure 存储 blob 容器。"
services: storage
documentationcenter: na
author: forester123
manager: digimobile
editor: tysonn
ms.assetid: 
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: sample
origin.date: 06/13/2017
ms.date: 10/23/2017
ms.author: v-johch
ms.openlocfilehash: 9b3117e62a398714297fe9c60656f290bddddcce
ms.sourcegitcommit: fea4940a09cecbae36256410227e701e5f0aab6d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/18/2017
---
# <a name="delete-containers-based-on-container-name-prefix"></a>根据容器名称前缀删除容器

此脚本根据容器名称的前缀删除 Azure Blob 存储中的容器。

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>示例脚本

```powershell
# this script will show how to delete containers with a specific prefix 
# the prefix this will search for is "image". 
# before running this, you need to create a storage account, create some containers,
#    some having the same prefix so you can test this
# note: this retrieves all of the matching containers in one command 
#       if you are going to run this against a storage account with a lot of containers
#       (more than a couple hundred), use continuation tokens to retrieve
#       the list of containers. We will be adding a sample showing that scenario in the future.

# these are for the storage account to be used
#   and the prefix for which to search
$resourceGroup = "containerdeletetestrg"
$location = "China East"
$storageAccountName = "containerdeletetest"
$prefix = "image"

# get a reference to the storage account and the context
$storageAccount = Get-AzureRmStorageAccount `
  -ResourceGroupName $resourceGroup `
  -Name $storageAccountName
$ctx = $storageAccount.Context 

# list all containers in the storage account 
Write-Host "All containers"
Get-AzureStorageContainer -Context $ctx | select Name

# retrieve list of containers to delete
$listOfContainersToDelete = Get-AzureStorageContainer -Context $ctx -Prefix $prefix

# write list of containers to be deleted 
Write-Host "Containers to be deleted"
$listOfContainersToDelete | select Name

# delete the containers; this pipes the result of the listing of the containers to delete
#    into the Remove-AzureStorageContainer command. It handles all of the containers in the list.
Write-Host "Deleting containers"
$listOfContainersToDelete | Remove-AzureStorageContainer -Context $ctx 

# show list of containers not deleted 
Write-Host "All containers not deleted"
Get-AzureStorageContainer -Context $ctx | select Name
```

## <a name="clean-up-deployment"></a>清理部署 

运行以下命令，删除资源组、其余容器和所有相关资源。

```powershell
Remove-AzureRmResourceGroup -Name containerdeletetestrg
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令根据容器名称前缀删除容器。 表中的每一项均链接到命令特定的文档。

| 命令 | 说明 |
|---|---|
| [Get-AzureRmStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.storage/get-azurermstorageaccount) | 获取资源组或订阅中的指定存储帐户或所有存储帐户。 |
| [Get-AzureStorageContainer](https://docs.microsoft.com/powershell/module/azure.storage/get-azurestoragecontainer) | 列出与存储帐户关联的存储容器。 |
| [Remove-AzureStorageContainer](https://docs.microsoft.com/powershell/module/azure.storage/remove-azurestoragecontainer) | 删除指定的存储容器。 |

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

有关其他存储 PowerShell 脚本示例，可参阅 [Azure Blob 存储的 PowerShell 示例](../blobs/storage-samples-blobs-powershell.md)。
