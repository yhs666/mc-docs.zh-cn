---
title: Azure PowerShell 脚本示例 - 将托管磁盘的快照复制（移动）到相同或不同的订阅 | Azure
description: Azure PowerShell 脚本示例 - 将托管磁盘的快照复制（移动）到相同或不同的订阅
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
origin.date: 06/06/2017
ms.date: 02/18/2019
ms.author: v-yeche
ms.openlocfilehash: 9b74e818649a8f1bb24ed05d1770a373035877fe
ms.sourcegitcommit: dd6cee8483c02c18fd46417d5d3bcc2cfdaf7db4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/22/2019
ms.locfileid: "56666311"
---
# <a name="copy-snapshot-of-a-managed-disk-in-same-subscription-or-different-subscription-with-powershell"></a>在相同或不同订阅中通过 PowerShell 复制托管磁盘的快照

此脚本在同一订阅或不同订阅中创建某个快照的副本。 使用此脚本将快照移到其他订阅中，进行数据保留。 不同订阅中的存储快照可防止意外删除主订阅中的快照。 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

## <a name="sample-script"></a>示例脚本

```powershell
#Provide the subscription Id of the subscription where snapshot exists
$sourceSubscriptionId='yourSourceSubscriptionId'

#Provide the name of your resource group where snapshot exists
$sourceResourceGroupName='yourResourceGroupName'

#Provide the name of the snapshot
$snapshotName='yourSnapshotName'

#Sign-in the Azure China Cloud
Connect-AzAccount -Environment AzureChinaCloud

#Set the context to the subscription Id where snapshot exists
Select-AzSubscription -SubscriptionId $sourceSubscriptionId

#Get the source snapshot
$snapshot= Get-AzSnapshot -ResourceGroupName $sourceResourceGroupName -Name $snapshotName

#Provide the subscription Id of the subscription where snapshot will be copied to
#If snapshot is copied to the same subscription then you can skip this step
$targetSubscriptionId='yourTargetSubscriptionId'

#Name of the resource group where snapshot will be copied to
$targetResourceGroupName='yourTargetResourceGroupName'

#Set the context to the subscription Id where snapshot will be copied to
#If snapshot is copied to the same subscription then you can skip this step
Select-AzSubscription -SubscriptionId $targetSubscriptionId

$snapshotConfig = New-AzSnapshotConfig -SourceResourceId $snapshot.Id -Location $snapshot.Location -CreateOption Copy 

#Create a new snapshot in the target subscription and resource group
New-AzSnapshot -Snapshot $snapshotConfig -SnapshotName $snapshotName -ResourceGroupName $targetResourceGroupName
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令，通过源快照的 ID 在目标订阅中创建快照。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [New-AzSnapshotConfig](https://docs.microsoft.com/powershell/module/az.compute/New-AzSnapshotConfig) | 创建用于创建快照的快照配置。 包括父快照的资源 ID 以及与父快照相同的位置。  |
| [New-AzSnapshot](https://docs.microsoft.com/powershell/module/az.compute/New-AzDisk) | 使用快照配置、快照名称和作为参数传递的资源组名称创建快照。 |

## <a name="next-steps"></a>后续步骤

[从快照创建虚拟机](./virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md)

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure Windows VM 文档](../windows/powershell-samples.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)中找到其他虚拟机 PowerShell 脚本示例。

<!--Update_Description: update meta properties, update link-->