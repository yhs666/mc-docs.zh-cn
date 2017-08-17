---
title: "将 Windows 虚拟机从非托管磁盘转换为托管磁盘 - Azure 托管磁盘 | Azure"
description: "如何在资源管理器部署模型中使用 PowerShell 将 Windows VM 从非托管磁盘转换为托管磁盘"
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
origin.date: 06/23/2017
ms.date: 08/14/2017
ms.author: v-dazen
ms.openlocfilehash: b44477b7f4b2b6876d0c1458f6d761357400543a
ms.sourcegitcommit: f858adac6a7a32df67bcd5c43946bba5b8ec6afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/07/2017
---
# <a name="convert-a-windows-virtual-machine-from-unmanaged-disks-to-managed-disks"></a>将 Windows 虚拟机从非托管磁盘转换为托管磁盘

如果有使用非托管磁盘的现有 Windows 虚拟机 (VM)，可通过 [Azure 托管磁盘](../../storage/storage-managed-disks-overview.md)服务将 VM 转换为使用托管磁盘。 此过程会同时转换 OS 磁盘和任何附加的数据磁盘。

本文介绍如何使用 Azure PowerShell 转换 VM。 如需进行安装或升级，请参阅[安装和配置 Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)。

## <a name="before-you-begin"></a>开始之前

* 请查看[计划迁移到托管磁盘](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks)。

[!INCLUDE [virtual-machines-common-convert-disks-considerations](../../../includes/virtual-machines-common-convert-disks-considerations.md)]

## <a name="convert-single-instance-vms"></a>转换单实例 VM
本节介绍如何将单实例 Azure VM 从非托管磁盘转换为托管磁盘。 （如果 VM 位于可用性集中，请参阅下一节。） 

1. 使用 [Stop-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/stop-azurermvm) cmdlet 解除分配 VM。 以下示例在名为 `myResourceGroup` 的资源组中解除分配名为 `myVM` 的 VM： 

  ```powershell
  $rgName = "myResourceGroup"
  $vmName = "myVM"
  Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force
  ```

2. 使用 [ConvertTo-AzureRmVMManagedDisk](https://docs.microsoft.com/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk) cmdlet 将 VM 转换为托管磁盘。 以下过程转换之前的 VM，包括 OS 磁盘和任何数据磁盘：

  ```powershell
  ConvertTo-AzureRmVMManagedDisk -ResourceGroupName $rgName -VMName $vmName
  ```

3. 使用 [Start-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/start-azurermvm) 在转换为托管磁盘后启动 VM。 以下示例重启之前的 VM：

  ```powershell
  Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
  ```

## <a name="convert-vms-in-an-availability-set"></a>在可用性集中转换 VM

如果要转换为托管磁盘的 VM 位于可用性集中，则需要先将可用性集转换为托管可用性集。

1. 使用 [Update-AzureRmAvailabilitySet](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermavailabilityset) cmdlet 转换可用性集。 以下示例在名为 `myResourceGroup` 的资源组中更新名为 `myAvailabilitySet` 的可用性集：

  ```powershell
  $rgName = 'myResourceGroup'
  $avSetName = 'myAvailabilitySet'

  $avSet = Get-AzureRmAvailabilitySet -ResourceGroupName $rgName -Name $avSetName
  Update-AzureRmAvailabilitySet -AvailabilitySet $avSet -Sku Aligned 
  ```

  如果可用性集所在的区域只有 2 个托管容错域，但却有 3 个非托管容错域，则此命令会显示类似于“指定的容错域计数 3 必须在 1 到 2 这个范围内”的错误消息。 若要解决此错误，请将容错域更新为 2，并按如下所示将 `Sku` 更新为 `Aligned`：

  ```powershell
  $avSet.PlatformFaultDomainCount = 2
  Update-AzureRmAvailabilitySet -AvailabilitySet $avSet -Sku Aligned
  ```

2. 解除分配 VM，并转换可用性集中的 VM。 以下脚本使用 [Stop-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/stop-azurermvm) cmdlet 解除分配每个 VM，使用 [ConvertTo-AzureRmVMManagedDisk](https://docs.microsoft.com/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk) 进行转换，并使用 [Start-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/start-azurermvm) 重启：

  ```powershell
  $avSet = Get-AzureRmAvailabilitySet -ResourceGroupName $rgName -Name $avSetName

  foreach($vmInfo in $avSet.VirtualMachinesReferences)
  {
     $vm = Get-AzureRmVM -ResourceGroupName $rgName | Where-Object {$_.Id -eq $vmInfo.id}
     Stop-AzureRmVM -ResourceGroupName $rgName -Name $vm.Name -Force
     ConvertTo-AzureRmVMManagedDisk -ResourceGroupName $rgName -VMName $vm.Name
     Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
  }
  ```

## <a name="convert-standard-managed-disks-to-premium"></a>将标准托管磁盘转换为高级托管磁盘
将 VM 转换为托管磁盘后，可在存储类型间进行切换。 还可使用具有标准存储和高级存储的混合磁盘。 

下方示例展示如何从标准存储切换到高级存储。 若要使用高级托管磁盘，VM 必须使用支持高级存储的 [VM 大小](sizes.md)。 此示例还切换到了支持高级存储的大小。

```powershell
$rgName = 'myResourceGroup'
$vmName = 'YourVM'
$size = 'Standard_DS2_v2'
$vm = Get-AzureRmVM -Name $vmName -rgName $resourceGroupName

# Stop and deallocate the VM before changing the size
Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force

# Change the VM size to a size that supports premium storage
$vm.HardwareProfile.VmSize = $size
Update-AzureRmVM -VM $vm -ResourceGroupName $rgName

# Get all disks in the resource group of the VM
$vmDisks = Get-AzureRmDisk -ResourceGroupName $rgName 

# For disks that belong to the selected VM, convert to premium storage
foreach ($disk in $vmDisks)
{
    if ($disk.OwnerId -eq $vm.Id)
    {
        $diskUpdateConfig = New-AzureRmDiskUpdateConfig -AccountType PremiumLRS
        Update-AzureRmDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $rgName `
        -DiskName $disk.Name
    }
}

Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
```

## <a name="troubleshooting"></a>故障排除

如果转换过程中出现错误，或先前转换中的问题导致 VM 处于“失败”状态，请再次运行 `ConvertTo-AzureRmVMManagedDisk` cmdlet。 简单的重试通常可以解决这种情况。

## <a name="managed-disks-and-azure-storage-service-encryption"></a>托管磁盘和 Azure 存储服务加密

如果非托管磁盘所在的存储帐户曾使用 [Azure 存储服务加密](../../storage/storage-service-encryption.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)进行了加密，则无法使用之前的步骤将其转换为托管磁盘。 下列步骤详细说明了如何复制和使用曾位于加密存储帐户中的非托管磁盘：

1. 使用 [AzCopy](../../storage/storage-use-azcopy.md) 将 VHD 复制到从未启用 Azure 存储服务加密的存储帐户。

2. 采用以下任一方式使用复制的 VM：

   * 使用 `New-AzureRmVm` 创建使用托管磁盘的 VM 并指定创建期间的 VHD 文件。

   * 使用 `Add-AzureRmVmDataDisk` 将复制的 VHD 附加到正在运行且使用托管磁盘的 VM。

## <a name="next-steps"></a>后续步骤

使用[快照](snapshot-copy-managed-disk.md)获取 VM 的只读副本。

<!--Update_Description: wording update-->