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
origin.date: 10/08/2018
ms.date: 10/22/2018
ms.author: v-yeche
ms.openlocfilehash: 480e4440456d082a390c36b5312c6882f92f0a95
ms.sourcegitcommit: c5529b45bd838791379d8f7fe90088828a1a67a1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2018
ms.locfileid: "50034986"
---
# <a name="create-a-snapshot"></a>创建快照

快照是虚拟硬盘 (VHD) 的完整只读副本。 可以创建 OS 或数据磁盘 VHD 的快照以用作备份，或用于解决虚拟机 (VM) 问题。 

## <a name="use-the-azure-portal"></a>使用 Azure 门户 

1. 登录到 [Azure 门户](https://portal.azure.cn)。
2. 在左侧菜单中，选择“创建资源”，然后搜索并选择“快照”。
3. 在“快照”窗口，选择“创建”。 此时将显示“创建快照”窗口。
4. 输入快照的 **名称** 。
5. 选择现有的[资源组](../../azure-resource-manager/resource-group-overview.md#resource-groups)，或键入新资源组的名称。 
6. 选择 Azure 数据中心“位置” 。  
7. 对于**源磁盘**，选择要获取其快照的托管磁盘。
8. 选择用于存储快照的“帐户类型”。 选择“Standard_HDD”，除非需要将快照存储在高性能磁盘上。
9. 选择“创建” 。

## <a name="use-powershell"></a>使用 PowerShell

以下步骤演示如何使用 [New-AzureRmSnapshot](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermsnapshot) cmdlet 复制 VHD 磁盘、创建快照配置以及创建磁盘的快照。 

开始前，请确保具有最新版本的 AzureRM.Compute PowerShell 模块（必须是 5.7.0 版本或更高版本）。 运行 `Get-Module -ListAvailable AzureRM` 即可查找版本。 如果需要进行升级，请参阅 [Install Azure PowerShell module](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)（安装 Azure PowerShell 模块）。 如果在本地运行 PowerShell，则需运行 [Connect-AzureRmAccount](https://docs.microsoft.com/powershell/module/azurerm.profile/connect-azurermaccount) 以创建与 Azure 的连接。

1. 设置一些参数： 

    ```PowerShell
    $resourceGroupName = 'myResourceGroup' 
    $location = 'chinaeast' 
    $vmName = 'myVM'
    $snapshotName = 'mySnapshot'  
    ```

2. 获取 VM：

    ```PowerShell
    $vm = get-azurermvm `
       -ResourceGroupName $resourceGroupName 
       -Name $vmName
    ```

3. 创建快照配置。 该示例中，此快照是 OS 磁盘的快照：

    ```PowerShell
    $snapshot =  New-AzureRmSnapshotConfig 
       -SourceUri $vm.StorageProfile.OsDisk.ManagedDisk.Id 
       -Location $location 
       -CreateOption copy
    ```

<!--Not Available on [availability zones](../../availability-zones/az-overview.md)-->

4. 拍摄快照：

    ```PowerShell
    New-AzureRmSnapshot 
       -Snapshot $snapshot 
       -SnapshotName $snapshotName 
       -ResourceGroupName $resourceGroupName 
    ```

## <a name="next-steps"></a>后续步骤

通过从快照创建托管磁盘，然后将新的托管磁盘附加为 OS 磁盘，来从快照创建虚拟机。 有关详细信息，请参阅[使用 PowerShell 从快照创建 VM](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md) 中的示例。

<!--Update_Description: update meta properties, update link， wording update -->
