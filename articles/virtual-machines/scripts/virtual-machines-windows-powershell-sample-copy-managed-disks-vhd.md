---
title: Azure PowerShell 脚本示例 - 将托管磁盘的 VHD 导出/复制到不同区域中的存储帐户 | Azure
description: Azure PowerShell 脚本示例 - 将托管磁盘的 VHD 导出/复制到相同或不同区域中的存储帐户
services: virtual-machines-windows
documentationcenter: storage
author: rockboyfor
manager: digimobile
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
origin.date: 09/17/2018
ms.date: 10/22/2018
ms.author: v-yeche
ms.openlocfilehash: 4ab5ee1344ad71b33534664cb00d926ac2e32e64
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52644716"
---
# <a name="exportcopy-the-vhd-of-a-managed-disk-to-a-storage-account-in-different-region-with-powershell"></a>使用 PowerShell 将托管磁盘的 VHD 导出/复制到不同区域中的存储帐户

此脚本将托管磁盘的 VHD 导出到不同区域中的存储帐户。 它首先生成托管磁盘的 SAS URI，然后使用该 SAS URI 将基础 VHD 复制到不同区域中的存储帐户。 使用此脚本将托管磁盘复制到另一区域以进行区域扩展。  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>示例脚本

```powershell
#Provide the subscription Id of the subscription where managed disk is created
$subscriptionId = "yourSubscriptionId"

#Provide the name of your resource group where managed is created
$resourceGroupName ="yourResourceGroupName"

#Provide the managed disk name 
$diskName = "yourDiskName"

#Provide Shared Access Signature (SAS) expiry duration in seconds e.g. 3600.
#Know more about SAS here: https://docs.azure.cn/storage/storage-dotnet-shared-access-signature-part-1
$sasExpiryDuration = "3600"

#Provide storage account name where you want to copy the underlying VHD of the managed disk. 
$storageAccountName = "yourstorageaccountName"

#Name of the storage container where the downloaded VHD will be stored
$storageContainerName = "yourstoragecontainername"

#Provide the key of the storage account where you want to copy the VHD of the managed disk. 
$storageAccountKey = 'yourStorageAccountKey'

#Provide the name of the destination VHD file to which the VHD of the managed disk will be copied.
$destinationVHDFileName = "yourvhdfilename"

# Set the context to the subscription Id where managed disk is created
Select-AzureRmSubscription -SubscriptionId $SubscriptionId

#Generate the SAS for the managed disk 
$sas = Grant-AzureRmDiskAccess -ResourceGroupName $ResourceGroupName -DiskName $diskName -DurationInSecond $sasExpiryDuration -Access Read 

#Create the context of the storage account where the underlying VHD of the managed disk will be copied
$destinationContext = New-AzureStorageContext -Environment AzureChinaCloud -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey  

#Copy the VHD of the managed disk to the storage account 
Start-AzureStorageBlobCopy -AbsoluteUri $sas.AccessSAS -DestContainer $storageContainerName -DestContext $destinationContext -DestBlob $destinationVHDFileName
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令生成托管磁盘的 SAS URI，然后使用该 SAS URI 将基础 VHD 复制到存储帐户。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [Grant-AzureRmDiskAccess](https://docs.microsoft.com/powershell/module/azurerm.compute/grant-azurermdiskaccess) | 为托管磁盘生成 SAS URI，该 SAS URI 用于将基础 VHD 复制到存储帐户。 |
| [New-AzureStorageContext](https://docs.microsoft.com/powershell/module/azure.storage/New-AzureStorageContext) | 使用帐户名和密钥创建存储帐户上下文。 此上下文可用于对存储帐户执行读/写操作。 |
| [Start-AzureStorageBlobCopy](https://docs.microsoft.com/powershell/module/azure.storage/Start-AzureStorageBlobCopy) | 将快照的基础 VHD 复制到存储帐户 |

## <a name="next-steps"></a>后续步骤

[从 VHD 创建托管磁盘](virtual-machines-windows-powershell-sample-create-managed-disk-from-vhd.md)

[从托管磁盘创建虚拟机](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md)

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

<!-- Update_Description: new articles on virtual machines windows powershell sample copy managed disks vhd -->
<!--ms.date: 10/22/2018-->

可以在 [Azure Windows VM 文档](../windows/powershell-samples.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)中找到其他虚拟机 PowerShell 脚本示例。