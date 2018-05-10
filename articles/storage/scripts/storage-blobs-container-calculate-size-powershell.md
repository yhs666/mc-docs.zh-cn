---
title: Azure PowerShell 脚本示例 - 计算 blob 容器大小 | Microsoft Docs
description: 通过计算容器各 blob 的总大小来计算 Azure Blob 存储中容器的大小。
services: storage
documentationcenter: na
author: forester123
manager: digimobile
editor: tysonn
ms.assetid: ''
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: powershell
ms.topic: sample
origin.date: 11/07/2017
ms.date: 05/07/2017
ms.author: v-johch
ms.openlocfilehash: eaf37294cee31708e41228ff52c1e9cab0c07dbc
ms.sourcegitcommit: 0b63440e7722942ee1cdabf5245ca78759012500
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/07/2018
---
# <a name="calculate-the-size-of-a-blob-storage-container"></a>计算 Blob 存储容器的大小

此脚本通过计算容器中 blob 的总大小来计算 Azure Blob 存储中容器的大小。

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

> [!IMPORTANT]
> 此 PowerShell 脚本提供容器的估计大小，不应该用于计费计算。 有关为计费目的计算容器大小的脚本，请参阅[为计费目的计算 Blob 存储容器的大小](../scripts/storage-blobs-container-calculate-billing-size-powershell.md)。 

## <a name="sample-script"></a>示例脚本
```powershell
# this script will show how to get the total size of the blobs in a container
# before running this, you need to create a storage account, create a container,
#    and upload some blobs into the container 
# note: this retrieves all of the blobs in the container in one command 
#       if you are going to run this against a container with a lot of blobs
#       (more than a couple hundred), use continuation tokens to retrieve
#       the list of blobs. We will be adding a sample showing that scenario in the future.

# these are for the storage account to be used
$resourceGroup = "bloblisttestrg"
$storageAccountName = "contosobloblisttest"
$containerName = "listtestblobs"

# get a reference to the storage account and the context
$storageAccount = Get-AzureRmStorageAccount `
  -ResourceGroupName $resourceGroup `
  -Name $storageAccountName
$ctx = $storageAccount.Context 

# get a list of all of the blobs in the container 
$listOfBLobs = Get-AzureStorageBlob -Container $ContainerName -Context $ctx 

# zero out our total
$length = 0

# this loops through the list of blobs and retrieves the length for each blob
#   and adds it to the total
$listOfBlobs | ForEach-Object {$length = $length + $_.Length}

# output the blobs and their sizes and the total 
Write-Host "List of Blobs and their size (length)"
Write-Host " " 
$listOfBlobs | select Name, Length
Write-Host " "
Write-Host "Total Length = " $length
```

## <a name="clean-up-deployment"></a>清理部署 

运行以下命令来删除资源组、容器和所有相关资源。

```powershell
Remove-AzureRmResourceGroup -Name bloblisttestrg
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令来计算 Blob 存储容器的大小。 表中的每一项均链接到命令特定的文档。

| 命令 | 注释 |
|---|---|
| [Get-AzureRmStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.storage/get-azurermstorageaccount) | 获取资源组或订阅中的指定存储帐户或所有存储帐户。 |
| [Get-AzureStorageBlob](https://docs.microsoft.com/powershell/module/azure.storage/get-azurestorageblob) | 列出容器中的 Blob。 ||

## <a name="next-steps"></a>后续步骤

有关为计费目的计算容器大小的脚本，请参阅[为计费目的计算 Blob 存储容器的大小](../scripts/storage-blobs-container-calculate-billing-size-powershell.md)。

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure 存储的 PowerShell 示例](../blobs/storage-samples-blobs-powershell.md)中找到其他存储 PowerShell 脚本示例。
