---
title: 在 Azure 中创建 VHD 的快照 | Azure
description: 了解如何创建 Azure VM 的副本，以便将其用作备份或用于排查问题。
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: ''
tags: azure-resource-manager
ms.assetid: 15eb778e-fc07-45ef-bdc8-9090193a6d20
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
origin.date: 10/09/2017
ms.date: 03/19/2018
ms.author: v-yeche
ms.openlocfilehash: c049edf86cb786bc8482c5d601f143a03af8df01
ms.sourcegitcommit: 41a236135b2eaf3d104aa1edaac00356f04807df
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2018
---
# <a name="create-a-snapshot"></a>创建快照

创建 OS 或数据磁盘 VHD 的快照，以便将其用作备份或用于排查 VM 问题。 快照是 VHD 的完整只读副本。 

## <a name="use-azure-portal-to-take-a-snapshot"></a>使用 Azure 门户创建快照 

1. 登录到 [Azure 门户](https://portal.azure.cn)。
2. 首先在左上角单击“创建资源”并搜索“快照”。
3. 在“快照”边栏选项卡中，单击“创建”。
4. 输入快照的 **名称** 。
5. 选择现有的[资源组](../../azure-resource-manager/resource-group-overview.md#resource-groups)，或键入新资源组的名称。 
6. 选择 Azure 数据中心“位置”。  
7. 对于**源磁盘**，选择要获取其快照的托管磁盘。
8. 选择用于存储快照的“帐户类型”。 建议使用 **Standard_LRS**，除非需要将其存储在高性能磁盘上。
9. 单击“创建”。

## <a name="use-powershell-to-take-a-snapshot"></a>使用 PowerShell 创建快照
以下步骤演示如何获取要复制的 VHD 磁盘，如何创建快照配置，以及如何使用 [New-AzureRmSnapshot](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermsnapshot) cmdlet 创建磁盘的快照。 

请确保安装了最新版本的 AzureRM.Compute PowerShell 模块。 运行以下命令来安装该模块。

```
Install-Module AzureRM.Compute -MinimumVersion 2.6.0
```
有关详细信息，请参阅 [Azure PowerShell 版本控制](https://docs.microsoft.com/powershell/azure/overview)。

1. 设置一些参数。 

    ```powershell
    $resourceGroupName = 'myResourceGroup' 
    $location = 'chinaeast' 
    $dataDiskName = 'myDisk' 
    $snapshotName = 'mySnapshot'  
    ```

2. 获取要复制的 VHD 磁盘。

    ```powershell
    $disk = Get-AzureRmDisk -ResourceGroupName $resourceGroupName -DiskName $dataDiskName 
    ```
3. 创建快照配置。 

    ```powershell
    $snapshot =  New-AzureRmSnapshotConfig -SourceUri $disk.Id -CreateOption Copy -Location $location 
    ```
4. 创建快照。

    ```powershell
    New-AzureRmSnapshot -Snapshot $snapshot -SnapshotName $snapshotName -ResourceGroupName $resourceGroupName 
    ```
如果计划使用快照创建托管磁盘并将其附加到需要高性能的 VM 上，请将参数 `-AccountType Premium_LRS` 用于 New-AzureRmSnapshotConfig 命令。 该参数创建快照，使其作为高级托管磁盘进行存储。 高级托管磁盘比标准托管磁盘费用高。 因此，在使用该参数之前，请确保确实需要“高级”。
<!-- Notice: -AccountType Premium_LRS 只能用于 New-AzureRmSnapshotConfig 命令,而不是 New-AzureRmSnapshot 命令 -->

## <a name="next-steps"></a>后续步骤

通过从快照创建托管磁盘，然后将新的托管磁盘附加为 OS 磁盘，来从快照创建虚拟机。 有关详细信息，请参阅[从快照创建 VM](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json) 示例。
<!--Update_Description: update meta properties, update link-->
