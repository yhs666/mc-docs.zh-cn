---
title: Azure PowerShell 脚本示例 - 轮换存储帐户访问密钥 | Microsoft Docs
description: 创建 Azure 存储帐户，然后检索并轮换其中的一个帐户访问密钥。
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
ms.devlang: azurecli
ms.topic: sample
origin.date: 06/13/2017
ms.date: 10/23/2017
ms.author: v-johch
ms.openlocfilehash: 3599af4d491ab655730f6e9537ff9ee6519fe7e4
ms.sourcegitcommit: 3583af94b935af10fcd4af3f4c904cf0397af798
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/29/2018
ms.locfileid: "37103094"
---
# <a name="create-a-storage-account-and-rotate-its-account-access-keys"></a>创建存储帐户并轮换其帐户访问密钥

此脚本会创建一个 Azure 存储帐户，显示新存储帐户的主访问密钥，然后续订（轮换）密钥。

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>示例脚本

```powershell
# this script will show how to rotate one of the access keys for a storage account

# get list of locations and pick one
Get-AzureRmLocation | select Location

# save the location you want to use  
$location = "China East"

# create a resource group
$resourceGroup = "rotatekeystestrg"
New-AzureRmResourceGroup -Name $resourceGroup -Location $location 

# create a standard general-purpose storage account 
$storageAccountName = "contosotestkeys"
New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name $storageAccountName `
  -Location $location `
  -SkuName Standard_LRS `

# retrieve the first storage account key and display it 
$storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroup -Name $storageAccountName).Value[0]

Write-Host "storage account key 1 = " $storageAccountKey

# re-generate the key
New-AzureRmStorageAccountKey -ResourceGroupName $resourceGroup `
    -Name $storageAccountName `
    -KeyName key1

# retrieve it again and display it 
$storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroup -Name $storageAccountName).Value[0]
Write-Host "storage account key 1 = " $storageAccountKey
```

## <a name="clean-up-deployment"></a>清理部署 

运行以下命令来删除资源组、存储帐户和所有相关资源。

```powershell
Remove-AzureRmResourceGroup -Name rotatekeystestrg
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建存储帐户并检索和轮换其中的一个访问密钥。 表中的每一项均链接到命令特定的文档。

| 命令 | 注释 |
|---|---|
| [Get-AzureRmLocation](/powershell/module/azurerm.resources/get-azurermlocation) | 获取所有位置以及每个位置支持的资源提供程序。 |
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | 创建 Azure 资源组。 |
| [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) | 创建存储帐户。 |
| [Get-AzureRmStorageAccountKey](/powershell/module/azurerm.storage/get-azurermstorageaccountkey) | 获取 Azure 存储帐户的访问密钥。 |
| [New-AzureRmStorageAccountKey](/powershell/module/azurerm.storage/new-azurermstorageaccountkey) | 重新生成 Azure 存储帐户的访问密钥。 |

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](/powershell/azure/overview)。

有关其他存储 PowerShell 脚本示例，可参阅 [Azure Blob 存储的 PowerShell 示例](../blobs/storage-samples-blobs-powershell.md)。
