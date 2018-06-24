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
origin.date: 04/10/2018
ms.date: 06/25/2018
ms.author: v-yeche
ms.openlocfilehash: 329a66455c92cae0669055d65d69f4fb164d2db8
ms.sourcegitcommit: 092d9ef3f2509ca2ebbd594e1da4048066af0ee3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/22/2018
ms.locfileid: "36315520"
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

在开始之前，请确保有最新版本的 AzureRM.Compute PowerShell 模块。 本文需要 5.7.0 版本或更高版本的 AzureRM 模块。 运行 `Get-Module -ListAvailable AzureRM` 即可查找版本。 如果需要进行升级，请参阅 [Install Azure PowerShell module](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)（安装 Azure PowerShell 模块）。 如果在本地运行 PowerShell，则还需运行 `Connect-AzureRmAccount -Environment AzureChinaCloud` 以创建与 Azure 的连接。

设置一些参数。 

 ```powershell
$resourceGroupName = 'myResourceGroup' 
$location = 'chinaeast' 
$vmName = 'myVM'
$snapshotName = 'mySnapshot'  
```

获取 VM。

 ```powershell
$vm = get-azurermvm `
   -ResourceGroupName $resourceGroupName `
   -Name $vmName
```

创建快照配置。 在此示例中，我们将创建 OS 磁盘的快照。

 ```powershell
$snapshot =  New-AzureRmSnapshotConfig `
   -SourceUri $vm.StorageProfile.OsDisk.ManagedDisk.Id `
   -Location $location `
   -CreateOption copy
```

> [!NOTE]
> 如果希望将快照存储在具有区域复原能力的存储中，需要在支持[可用性区域](../../availability-zones/az-overview.md)的区域中创建该快照并包括 `-SkuName Standard_ZRS` 参数。   

创建快照。

```powershell
New-AzureRmSnapshot `
   -Snapshot $snapshot `
   -SnapshotName $snapshotName `
   -ResourceGroupName $resourceGroupName 
```

## <a name="next-steps"></a>后续步骤

通过从快照创建托管磁盘，然后将新的托管磁盘附加为 OS 磁盘，来从快照创建虚拟机。 有关详细信息，请参阅[从快照创建 VM](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json) 示例。
<!--Update_Description: update meta properties, update link-->
