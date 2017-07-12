---
title: "将 VM 从非托管磁盘转换为托管磁盘 - Azure | Azure"
description: "在 Resource Manager 部署模型中使用 PowerShell 将 VM 从非托管磁盘转换为托管磁盘"
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
origin.date: 06/05/2017
ms.date: 07/03/2017
ms.author: v-dazen
ms.openlocfilehash: 8b72a8347a5d0701ae5b15c875a74f1db6a42a84
ms.sourcegitcommit: b1d2bd71aaff7020dfb3f7874799e03df3657cd4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
<a id="convert-a-vm-from-unmanaged-disks-to-managed-disks" class="xliff"></a>

# 将 VM 从非托管磁盘转换为托管磁盘

如果 Azure 中的现有 Linux VM 在存储帐户中使用非托管磁盘，而你希望这些 VM 能充分利用[托管磁盘](../../storage/storage-managed-disks-overview.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)，可以转换这些 VM。 此过程将同时转换 OS 磁盘和任何附加的数据磁盘。 该转换过程需要重新启动 VM，因此请在预先存在的维护时段内计划 VM 迁移。 迁移过程不可逆。 在生产中执行迁移之前，请务必通过迁移测试虚拟机来测试迁移过程。 在开始之前，请确保已查看[规划迁移到托管磁盘](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks)。

> [!IMPORTANT] 
> 在转换期间，将解除分配 VM。 转换完成后，VM 在启动时会接收新的 IP 地址。 如果依赖于固定 IP，请使用保留 IP。

<a id="prepare-availability-set-for-conversion" class="xliff"></a>

## 准备用于转换的可用性集

> [!NOTE] 
> 如果 VM 不在可用性集中，请跳过此步骤。

```powershell
$rgName = 'myResourceGroup'
$avSetName = 'myAvailabilitySet'

$avSet =  Get-AzureRmAvailabilitySet -ResourceGroupName $rgName -Name $avSetName
Update-AzureRmAvailabilitySet -AvailabilitySet $avSet -Sku Aligned 
```

<a id="troubleshooting" class="xliff"></a>

### 故障排除

错误：指定的容错域计数 3 必须在 1 到 2 的范围内。

如果可用性集所在的区域只包含 2 个托管容错域，但非托管容错域的数目为 3，则会引发上述错误。 若要解决此错误，请将容错域的数目更新为 2，同时按如下所示更新 SKU：

```powershell
$avSet.PlatformFaultDomainCount = 2
Update-AzureRmAvailabilitySet -AvailabilitySet $avSet -Sku Aligned
```

<a id="prepare-vms-for-conversion" class="xliff"></a>

## 准备用于转换的 VM
本部分包含在将 VM 转换为托管磁盘之前或者在不同的存储类型之间转换时需要采取的步骤。

<a id="deallocate-the-vm" class="xliff"></a>

### 解除分配 VM
只能在解除分配 VM 之后转换为托管磁盘。 在下面显示的步骤中，我们使用了 `Stop-AzureRmVM` cmdlet 来停止 VM。

<a id="converting-from-standard-to-premium-storage" class="xliff"></a>

### 从标准存储转换为高级存储
在转换到托管磁盘的过程中，还可以将 VM 转换为高级存储。 若要使用高级托管磁盘，VM 必须使用支持高级存储的 [VM 大小](sizes.md)。 确定适当的大小后，可按如下所示更新 VM 的硬件配置文件。

<a id="convert-vms-in-an-availability-set-to-managed-disks" class="xliff"></a>

## 将可用性集中的 VM 转换为托管磁盘
由于在前面的步骤中已将可用性集转换为托管可用性集，因此我们可以开始转换 VM。

```powershell
$avSet =  Get-AzureRmAvailabilitySet -ResourceGroupName $rgName -Name $avSetName

foreach($vmInfo in $avSet.VirtualMachinesReferences)
{
   $vm =  Get-AzureRmVM -ResourceGroupName $rgName | Where-Object {$_.Id -eq $vmInfo.id}
   Stop-AzureRmVM -ResourceGroupName $rgName -Name  $vm.Name -Force
   ConvertTo-AzureRmVMManagedDisk -ResourceGroupName $rgName -VMName $vm.Name
}
```

<a id="convert-vms-not-in-an-availability-set" class="xliff"></a>

## 转换不在可用性集中的 VM
对于不在可用性集中的 VM，只需解除分配 VM，然后转换为托管磁盘。

```powershell
$rgName = "myResourceGroup"
$vmName = "myVM"
Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force
ConvertTo-AzureRmVMManagedDisk -ResourceGroupName $rgName -VMName $vmName
```

<a id="convert-vms-using-standard-managed-disks-to-premium-managed-disks" class="xliff"></a>

## 将使用标准托管磁盘的 VM 转换为高级托管磁盘
将 VM 转换为托管磁盘后，接下来还可以在存储类型之间切换。 以下示例演示如何从标准存储类型切换到高级存储类型。 若要使用高级托管磁盘，VM 必须使用支持高级存储的 [VM 大小](sizes.md)。 在以下示例中，我们还会切换到支持高级存储的大小。

```powershell
$resourceGroupName = 'YourResourceGroupName'
$vmName = 'YourVMName'
$size = 'Standard_DS2_v2'
$vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $resourceGroupName

# Stop deallocate the VM before changing the size
Stop-AzureRmVM -ResourceGroupName $resourceGroupName -Name $vmName -Force

# Change VM size to a size supporting Premium storage
$vm.HardwareProfile.VmSize = $size
Update-AzureRmVM -VM $vm -ResourceGroupName $resourceGroupName

# Get all disks in the resource group of the VM
$vmDisks = Get-AzureRmDisk -ResourceGroupName $resourceGroupName 

# For disks that belong to the VM selected, convert to Premium storage
foreach ($disk in $vmDisks)
{
    if($disk.OwnerId -eq $vm.Id)
    {
        $diskUpdateConfig = New-AzureRmDiskUpdateConfig -AccountType PremiumLRS
        Update-AzureRmDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $resourceGroupName `
        -DiskName $disk.Name
    }
}

Start-AzureRmVM -ResourceGroupName $resourceGroupName -Name $vmName
```

> [!NOTE] 
> 也可以对磁盘混合使用标准和高级存储。

> [!IMPORTANT] 
> 转换之前 VM 使用的原始 VHD 和存储帐户未被删除，因此会产生费用。 若要避免这些项目产生费用，请在确认转换已完成后删除原始 VHD Blob。

<a id="troubleshooting" class="xliff"></a>

### 故障排除
如果转换期间出错 - 或者由于前面的转换出现问题而导致某个虚拟机处于故障状态，请再次运行 ConvertTo-AzureRmVMManagedDisk cmdlet 重试转换。
简单的重试通常可以解决这种情况。

<a id="managed-disks-and-azure-storage-service-encryption-sse" class="xliff"></a>

## 托管磁盘和 Azure 存储服务加密 (SSE)

如果非托管磁盘所在的存储帐户已使用或曾使用 [Azure 存储服务加密 (SSE)](../../storage/storage-service-encryption.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json) 加密，则无法将其转换为托管磁盘。 以下步骤详细说明了如何转换位于（或曾位于）已加密存储帐户的非托管磁盘：

- 使用 [AzCopy](../../storage/storage-use-azcopy.md) 将虚拟硬盘 (VHD) 复制到从未启用 Azure 存储服务加密的存储帐户。
- 运行 `New-AzureRmVm` 创建使用托管磁盘的 VM 并指定创建期间的 VHD 文件，或
- 使用 `Add-AzureRmVmDataDisk` 将复制的 VHD 附加到具有托管磁盘的正在运行中的 VM。

<a id="next-steps" class="xliff"></a>

## 后续步骤

使用[快照](snapshot-copy-managed-disk.md)获取 VM 的只读副本。
