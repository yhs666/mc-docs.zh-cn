---
title: "Azure PowerShell 脚本示例 - 在相同或不同订阅的存储帐户中从 VHD 文件创建托管磁盘 | Microsoft Docs"
description: "Azure PowerShell 脚本示例 - 在相同或不同订阅的存储帐户中从 VHD 文件创建托管磁盘"
services: managed-disks-windows
documentationcenter: storage
author: forester123
manager: digimobile
editor: ramankum
tags: azure-service-management
ms.assetid: 
ms.service: managed-disks-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
origin.date: 06/05/2017
ms.date: 06/26/2017
ms.author: v-johch
ms.openlocfilehash: ca75abc677075c4226312d767f936257ecc9a3ec
ms.sourcegitcommit: cc3f528827a8acd109ba793eee023b8c6b2b75e4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
# 使用 PowerShell 在相同或不同订阅的存储帐户中从 VHD 文件创建托管磁盘
<a id="create-a-managed-disk-from-a-vhd-file-in-a-storage-account-in-same-or-different-subscription-with-powershell" class="xliff"></a>

此脚本在相同或不同订阅的存储帐户中从 VHD 文件创建托管磁盘。 使用此脚本将专用 VHD（未通用化/未进行 sysprep）导入到托管 OS 磁盘以创建虚拟机。 同时，使用它将数据 VHD 导入到托管数据磁盘。 

请勿在短时间内从一个 VHD 文件创建多个相同的托管磁盘。 若要从 VHD 文件创建托管磁盘，需创建该 VHD 文件的 blob 快照，然后再使用快照创建托管磁盘。 一分钟内只可创建一个 blob 快照，此限制会导致磁盘创建失败。 若要避免此限制，请[从 VHD 文件创建托管快照](./../scripts/storage-windows-powershell-sample-create-snapshot-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json)，然后使用托管快照在短时间内创建多个托管磁盘。 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## 示例脚本
<a id="sample-script" class="xliff"></a>

```PowerShell
#Provide the subscription Id where Managed Disks will be created
$subscriptionId = 'yourSubscriptionId'

#Provide the name of your resource group where Managed Disks will be created. 
$resourceGroupName ='yourResourceGroupName'

#Provide the name of the Managed Disk
$diskName = 'yourDiskName'

#Provide the size of the disks in GB. It should be greater than the VHD file size.
$diskSize = '128'

#Provide the storage type for Managed Disk. PremiumLRS or StandardLRS.
$storageType = 'PremiumLRS'

#Provide the Azure region (e.g. China East) where Managed Disk will be located.
#This location should be same as the storage account where VHD file is stored
#Get all the Azure location using command below:
#Get-AzureRmLocation
$location = 'China East'

#Provide the URI of the VHD file (page blob) in a storage account. Please not that this is NOT the SAS URI of the storage container where VHD file is stored. 
#e.g. https://contosostorageaccount1.blob.core.chinacloudapi.cn/vhds/contosovhd123.vhd
#Note: VHD file can be deleted as soon as Managed Disk is created.
$sourceVHDURI = 'https://contosostorageaccount1.blob.core.chinacloudapi.cn/vhds/contosovhd123.vhd'

#Provide the resource Id of the storage account where VHD file is stored.
#e.g. /subscriptions/6472s1g8-h217-446b-b509-314e17e1efb0/resourceGroups/MDDemo/providers/Microsoft.Storage/storageAccounts/contosostorageaccount
#This is an optional parameter if you are creating managed disk in the same subscription
$storageAccountId = '/subscriptions/yourSubscriptionId/resourceGroups/yourResourceGroupName/providers/Microsoft.Storage/storageAccounts/yourStorageAccountName'

#Set the context to the subscription Id where Managed Disk will be created
Select-AzureRmSubscription -SubscriptionId $SubscriptionId

$diskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location $location -CreateOption Import -StorageAccountId $storageAccountId -SourceUri $sourceVHDURI
 
New-AzureRmDisk -Disk $diskConfig -ResourceGroupName $resourceGroupName -DiskName $diskName
 
```


## 脚本说明
<a id="script-explanation" class="xliff"></a>

此脚本使用以下命令在不同订阅中从 VHD 创建托管磁盘。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 说明 |
|---|---|
| [New-AzureRmDiskConfig](https://docs.microsoft.com/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | 创建用于磁盘创建的磁盘配置。 包括存储类型、位置、存储父 VHD 的存储帐户的资源 ID 以及父 VHD 的 VHD URI。 |
| [New-AzureRmDisk](https://docs.microsoft.com/powershell/module/azurerm.compute/New-AzureRmDisk) | 使用磁盘配置、磁盘名称和作为参数传递的资源组名称创建磁盘。 |

## 后续步骤
<a id="next-steps" class="xliff"></a>

[通过将托管磁盘附加为 OS 磁盘来创建虚拟机](./../../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure Windows VM 文档](../../virtual-machines/windows/powershell-samples.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)中找到其他虚拟机 PowerShell 脚本示例。