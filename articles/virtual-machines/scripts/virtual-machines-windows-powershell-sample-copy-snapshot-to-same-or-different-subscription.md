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
ms.date: 08/27/2018
ms.author: v-yeche
ms.openlocfilehash: 32049945c9f53deed80d614b46382ef5d6e68e37
ms.sourcegitcommit: bdffde936fa2a43ea1b5b452b56d307647b5d373
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/24/2018
ms.locfileid: "42872289"
---
# <a name="copy-snapshot-of-a-managed-disk-in-same-subscription-or-different-subscription-with-powershell"></a>在相同或不同订阅中通过 PowerShell 复制托管磁盘的快照

此脚本在同一订阅或不同订阅中创建某个快照的副本。 使用此脚本将快照移到其他订阅中，进行数据保留。 不同订阅中的存储快照可防止意外删除主订阅中的快照。 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>示例脚本

```powershell
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

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令，通过源快照的 ID 在目标订阅中创建快照。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [New-AzureRmSnapshotConfig](https://docs.microsoft.com/powershell/module/azurerm.compute/New-AzureRmSnapshotConfig) | 创建用于创建快照的快照配置。 包括父快照的资源 ID 以及与父快照相同的位置。  |
| [New-AzureRmSnapshot](https://docs.microsoft.com/powershell/module/azurerm.compute/New-AzureRmDisk) | 使用快照配置、快照名称和作为参数传递的资源组名称创建快照。 |

## <a name="next-steps"></a>后续步骤

[从快照创建虚拟机](./virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md)

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure Windows VM 文档](../windows/powershell-samples.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)中找到其他虚拟机 PowerShell 脚本示例。

<!--Update_Description: update meta properties, update link-->