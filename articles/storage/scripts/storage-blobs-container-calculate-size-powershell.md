---
title: Azure PowerShell 脚本示例 - 计算 Blob 容器大小 | Azure
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
ms.date: 06/11/2018
ms.author: v-johch
ms.openlocfilehash: ce1374389f842f441bd20f22cd0dfcc921ce98d0
ms.sourcegitcommit: 49c8c21115f8c36cb175321f909a40772469c47f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/08/2018
ms.locfileid: "34867526"
---
# <a name="calculate-the-size-of-a-blob-storage-container"></a>计算 Blob 存储容器的大小

此脚本通过计算容器中 blob 的总大小来计算 Azure Blob 存储中容器的大小。

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

> [!IMPORTANT]
> 此 PowerShell 脚本提供容器的估计大小，不应该用于计费计算。 有关为计费目的计算容器大小的脚本，请参阅[为计费目的计算 Blob 存储容器的大小](../scripts/storage-blobs-container-calculate-billing-size-powershell.md)。 

## <a name="sample-script"></a>示例脚本

# <a name="this-script-will-show-how-to-get-the-total-size-of-the-blobs-in-a-container"></a>此脚本将演示如何获取容器中 Blob 的总大小
# <a name="before-running-this-you-need-to-create-a-storage-account-create-a-container"></a>在运行此脚本之前，需创建存储帐户，创建容器，
#    <a name="and-upload-some-blobs-into-the-container"></a>并将某些 Blob 上传到容器中 
# <a name="note-this-retrieves-all-of-the-blobs-in-the-container-in-one-command"></a>注意：这样会通过一个命令检索容器中的所有 Blob 
#       <a name="if-you-are-going-to-run-this-against-a-container-with-a-lot-of-blobs"></a>针对容器运行此脚本时，如果容器中包含许多 Blob
#       <a name="more-than-a-couple-hundred-use-continuation-tokens-to-retrieve"></a>（超出数百个），请使用继续标记来检索
#       <a name="the-list-of-blobs-we-will-be-adding-a-sample-showing-that-scenario-in-the-future"></a>Blob 的列表。 我们会在以后添加一个演示该方案的示例。

# <a name="these-are-for-the-storage-account-to-be-used"></a>这些是针对要使用的存储帐户的
$resourceGroup = "bloblisttestrg" $storageAccountName = "contosobloblisttest" $containerName = "listtestblobs"

# <a name="get-a-reference-to-the-storage-account-and-the-context"></a>获取对存储帐户和上下文的引用
$storageAccount = Get-AzureRmStorageAccount `
  -ResourceGroupName $resourceGroup ` -Name $storageAccountName $ctx = $storageAccount.Context 

# <a name="get-a-list-of-all-of-the-blobs-in-the-container"></a>获取容器中所有 Blob 的列表 
$listOfBLobs = Get-AzureStorageBlob -Container $ContainerName -Context $ctx 

# <a name="zero-out-our-total"></a>将总计归零
$length = 0

# <a name="this-loops-through-the-list-of-blobs-and-retrieves-the-length-for-each-blob"></a>此命令循环访问 Blob 的列表，检索每个 Blob 的长度，
#   <a name="and-adds-it-to-the-total"></a>然后将其添加到总计
$listOfBlobs | ForEach-Object {$length = $length + $_.Length}

# <a name="output-the-blobs-and-their-sizes-and-the-total"></a>输出 Blob 及其大小和总计 
Write-Host "Blob 及其大小(长度)的列表" Write-Host " " $listOfBlobs | select Name, Length Write-Host " " Write-Host "总长度 = " $length

## <a name="clean-up-deployment"></a>清理部署 

运行以下命令来删除资源组、容器和所有相关资源。

```powershell
Remove-AzureRmResourceGroup -Name bloblisttestrg
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令来计算 Blob 存储容器的大小。 表中的每一项均链接到命令特定的文档。

| 命令 | 注释 |
|---|---|
| [Get-AzureRmStorageAccount](/powershell/module/azurerm.storage/get-azurermstorageaccount) | 获取资源组或订阅中的指定存储帐户或所有存储帐户。 |
| [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) | 列出容器中的 Blob。 ||

## <a name="next-steps"></a>后续步骤

有关为计费目的计算容器大小的脚本，请参阅[为计费目的计算 Blob 存储容器的大小](../scripts/storage-blobs-container-calculate-billing-size-powershell.md)。

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](/powershell/azure/overview)。

可以在 [Azure 存储的 PowerShell 示例](../blobs/storage-samples-blobs-powershell.md)中找到其他存储 PowerShell 脚本示例。
