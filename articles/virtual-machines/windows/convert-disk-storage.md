---
title: Azure 标准与高级托管磁盘存储的相互转换 | Azure
description: 如何使用 Azure PowerShell 实现 Azure 标准与高级托管磁盘的相互转换。
services: virtual-machines-windows
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
origin.date: 08/07/2017
ms.date: 08/27/2018
ms.author: v-yeche
ms.openlocfilehash: 1a2d4b2b35fd9c9ed47c8999ecb07034bfa1e934
ms.sourcegitcommit: bdffde936fa2a43ea1b5b452b56d307647b5d373
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/24/2018
ms.locfileid: "42872366"
---
# <a name="convert-azure-managed-disks-storage-from-standard-to-premium-and-vice-versa"></a>Azure 标准与高级托管磁盘存储的相互转换

托管磁盘提供两种存储选项：[高级 SSD](../windows/premium-storage.md) 和[标准 HDD](../windows/standard-storage.md)。 它支持基于性能需求在选项之间轻松切换，并保障最短停机时间。 非托管磁盘不支持此操作。 但可以轻松[转换为托管磁盘](convert-unmanaged-to-managed-disks.md)，以在这些磁盘类型之间轻松切换。

本文介绍了如何使用 Azure PowerShell 实现标准与高级托管磁盘的相互转换。 如需进行安装或升级，请参阅[安装和配置 Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)。

## <a name="before-you-begin"></a>准备阶段

* 该转换需要重启 VM，因此请在预先存在的维护时段内计划磁盘存储迁移。 
* 如果使用非托管磁盘，请首先[转换为托管磁盘](convert-unmanaged-to-managed-disks.md)，并按照本文说明在存储选项之间切换。 
* 本文需要 Azure PowerShell 模块 6.0.0 版或更高版本。 运行 ` Get-Module -ListAvailable AzureRM` 即可查找版本。 如果需要进行升级，请参阅 [Install Azure PowerShell module](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)（安装 Azure PowerShell 模块）。 此外，还需要运行 `Connect-AzureRmAccount -Environment AzureChinaCloud` 以创建与 Azure 的连接。

## <a name="convert-all-the-managed-disks-of-a-vm-from-standard-to-premium-and-vice-versa"></a>实现 VM 的所有标准与高级托管磁盘的相互转换

以下示例展示如何将 VM 的所有磁盘从标准存储切换到高级存储。 若要使用高级托管磁盘，VM 必须使用支持高级存储的 [VM 大小](sizes.md)。 此示例还切换到了支持高级存储的大小。

```PowerShell
# Name of the resource group that contains the VM
$rgName = 'yourResourceGroup'

# Name of the your virtual machine
$vmName = 'yourVM'

# Choose between StandardLRS and PremiumLRS based on your scenario
$storageType = 'Premium_LRS'

# Premium capable size
# Required only if converting storage from standard to premium
$size = 'Standard_DS2_v2'

# Stop and deallocate the VM before changing the size
Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force

$vm = Get-AzureRmVM -Name $vmName -resourceGroupName $rgName

# Change the VM size to a size that supports premium storage
# Skip this step if converting storage from premium to standard
$vm.HardwareProfile.VmSize = $size
Update-AzureRmVM -VM $vm -ResourceGroupName $rgName

# Get all disks in the resource group of the VM
$vmDisks = Get-AzureRmDisk -ResourceGroupName $rgName 

# For disks that belong to the selected VM, convert to premium storage
foreach ($disk in $vmDisks)
{
    if ($disk.ManagedBy -eq $vm.Id)
    {
        $diskUpdateConfig = New-AzureRmDiskUpdateConfig -AccountType $storageType
        Update-AzureRmDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $rgName `
        -DiskName $disk.Name
    }
}

Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
```
## <a name="convert-a-managed-disk-from-standard-to-premium-and-vice-versa"></a>标准与高级托管磁盘的相互转换

对于开发/测试工作负荷，可能需要同时具有标准磁盘和高级磁盘，以减少成本。 可通过仅将需要更佳性能的磁盘升级到高级存储来实现此目的。 以下示例展示如何将 VM 的单个磁盘在标准存储与高级存储之间相互切换。 若要使用高级托管磁盘，VM 必须使用支持高级存储的 [VM 大小](sizes.md)。 此示例还切换到了支持高级存储的大小。

```PowerShell

$diskName = 'yourDiskName'
# resource group that contains the managed disk
$rgName = 'yourResourceGroupName'
# Choose between StandardLRS and PremiumLRS based on your scenario
$storageType = 'Premium_LRS'
# Premium capable size 
$size = 'Standard_DS2_v2'

$disk = Get-AzureRmDisk -DiskName $diskName -ResourceGroupName $rgName

# Get parent VM resource
$vmResource = Get-AzureRmResource -ResourceId $disk.ManagedBy

# Stop and deallocate the VM before changing the storage type
Stop-AzureRmVM -ResourceGroupName $vm.ResourceGroupName -Name $vmResource.Name -Force

$vm = Get-AzureRmVM -ResourceGroupName $vmResource.ResourceGroupName -Name $vmResource.Name 

# Change the VM size to a size that supports premium storage
# Skip this step if converting storage from premium to standard
$vm.HardwareProfile.VmSize = $size
Update-AzureRmVM -VM $vm -ResourceGroupName $rgName

# Update the storage type
$diskUpdateConfig = New-AzureRmDiskUpdateConfig -AccountType $storageType -DiskSizeGB $disk.DiskSizeGB
Update-AzureRmDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $rgName `
-DiskName $disk.Name

Start-AzureRmVM -ResourceGroupName $vm.ResourceGroupName -Name $vm.Name
```

<!-- Not Available on ## Convert a managed disk from standard HDD to standard SSD, and vice versa-->
## <a name="next-steps"></a>后续步骤

使用[快照](snapshot-copy-managed-disk.md)获取 VM 的只读副本。
<!--Update_Description: update meta properties, wording update -->
