---
title: "Azure PowerShell 脚本示例 - 从快照创建托管磁盘 | Azure"
description: "Azure PowerShell 脚本示例 - 从快照创建托管磁盘"
services: virtual-machines-windows
documentationcenter: storage
author: hayley244
manager: digimobile
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
origin.date: 06/05/2017
ms.date: 08/28/2017
ms.author: v-haiqya
ms.openlocfilehash: 4db0266900cc02db9734b0bcf146cc417be69899
ms.sourcegitcommit: 0f2694b659ec117cee0110f6e8554d96ee3acae8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/25/2017
---
# <a name="create-a-managed-disk-from-a-snapshot-with-powershell"></a>使用 PowerShell 从快照创建托管磁盘

此脚本从快照创建托管磁盘。 使用它从 OS 和数据磁盘的快照还原虚拟机。 从各自的快照创建 OS 和数据托管磁盘，然后通过附加托管磁盘创建新的虚拟机。 还可以通过附加从快照创建的数据磁盘还原现有 VM 的数据磁盘。

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>示例脚本

```powershell
#Provide the subscription Id
$subscriptionId = 'yourSubscriptionId'

#Provide the name of your resource group
$resourceGroupName ='yourResourceGroupName'

#Provide the name of the snapshot that will be used to create Managed Disks
$snapshotName = 'yourSnapshotName'

#Provide the name of the Managed Disk
$diskName = 'yourManagedDiskName'

#Provide the size of the disks in GB. It should be greater than the VHD file size.
$diskSize = '128'

#Provide the storage type for Managed Disk. PremiumLRS or StandardLRS.
$storageType = 'PremiumLRS'

#Provide the Azure region (e.g. China North) where Managed Disks will be located.
#This location should be same as the snapshot location
#Get all the Azure location using command below:
#Get-AzureRmLocation
$location = 'China North'

#Set the context to the subscription Id where Managed Disk will be created
Select-AzureRmSubscription -SubscriptionId $SubscriptionId

$snapshot = Get-AzureRmSnapshot -ResourceGroupName $resourceGroupName -SnapshotName $snapshotName 

$diskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location $location -CreateOption Copy -SourceResourceId $snapshot.Id

New-AzureRmDisk -Disk $diskConfig -ResourceGroupName $resourceGroupName -DiskName $diskName

```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令从快照创建托管磁盘。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 说明 |
|---|---|
| [Get-AzureRmSnapshot](https://docs.microsoft.com/powershell/module/azurerm.compute/Get-AzureRmSnapshot) | 获取快照属性。  |
| [New-AzureRmDiskConfig](https://docs.microsoft.com/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | 创建用于磁盘创建的磁盘配置。 包括父快照的资源 ID、与父快照位置相同的位置以及存储类型。  |
| [New-AzureRmDisk](https://docs.microsoft.com/powershell/module/azurerm.compute/New-AzureRmDisk) | 使用磁盘配置、磁盘名称和作为参数传递的资源组名称创建磁盘。 |

## <a name="next-steps"></a>后续步骤

[从托管磁盘创建虚拟机](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure Windows VM 文档](../../virtual-machines/windows/powershell-samples.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)中找到其他虚拟机 PowerShell 脚本示例。
