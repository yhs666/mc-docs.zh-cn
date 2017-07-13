---
title: "Azure PowerShell 脚本示例 - 将托管磁盘的快照复制（移动）到相同或不同的订阅 | Microsoft Docs"
description: "Azure PowerShell 脚本示例 - 将托管磁盘的快照复制（移动）到相同或不同的订阅"
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
origin.date: 06/06/2017
ms.date: 06/26/2017
ms.author: v-johch
ms.openlocfilehash: 0d0d241a9b3d37b597db861dbceccedcf6a5a179
ms.sourcegitcommit: cc3f528827a8acd109ba793eee023b8c6b2b75e4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
# 在相同或不同订阅中通过 PowerShell 复制托管磁盘的快照
<a id="copy-snapshot-of-a-managed-disk-in-same-subscription-or-different-subscription-with-powershell" class="xliff"></a>

此脚本在相同或不同订阅中创建快照副本。 使用此脚本将快照移到其他订阅中，进行数据保留。 不同订阅中的存储快照可防止意外删除主订阅中的快照。 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## 示例脚本
<a id="sample-script" class="xliff"></a>

```PowerShell
#Provide the subscription Id of the subscription where snapshot exists
$sourceSubscriptionId='yourSourceSubscriptionId'

#Provide the name of your resource group where snapshot exists
$sourceResourceGroupName='yourResourceGroupName'

#Provide the name of the snapshot
$snapshotName='yourSnapshotName'

#Set the context to the subscription Id where snapshot exists
Select-AzureRmSubscription -SubscriptionId $sourceSubscriptionId

#Get the source snapshot
$snapshot= Get-AzureRmSnapshot -ResourceGroupName $sourceResourceGroupName -Name $snapshotName

#Provide the subscription Id of the subscription where snapshot will be copied to
#If snapshot is copied to the same subscription then you can skip this step
$targetSubscriptionId='yourTargetSubscriptionId'

#Name of the resource group where snapshot will be copied to
$targetResourceGroupName='yourTargetResourceGroupName'

#Set the context to the subscription Id where snapshot will be copied to
#If snapshot is copied to the same subscription then you can skip this step
Select-AzureRmSubscription -SubscriptionId $targetSubscriptionId

$snapshotConfig = New-AzureRmSnapshotConfig -SourceResourceId $snapshot.Id -Location $snapshot.Location -CreateOption Copy 

#Create a new snapshot in the target subscription and resource group
New-AzureRmSnapshot -Snapshot $snapshotConfig -SnapshotName $snapshotName -ResourceGroupName $targetResourceGroupName
```


## 脚本说明
<a id="script-explanation" class="xliff"></a>

此脚本使用以下命令，通过源快照的 ID 在目标订阅中创建快照。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 说明 |
|---|---|
| [New-AzureRmSnapshotConfig](https://docs.microsoft.com/powershell/module/azurerm.compute/New-AzureRmSnapshotConfig) | 创建用于创建快照的快照配置。 包括父快照的资源 ID 以及与父快照相同的位置。  |
| [New-AzureRmSnapshot](https://docs.microsoft.com/powershell/module/azurerm.compute/New-AzureRmDisk) | 使用快照配置、快照名称和作为参数传递的资源组名称创建快照。 |


## 后续步骤
<a id="next-steps" class="xliff"></a>

[从快照创建虚拟机](./../../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure Windows VM 文档](../../virtual-machines/windows/powershell-samples.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)中找到其他虚拟机 PowerShell 脚本示例。