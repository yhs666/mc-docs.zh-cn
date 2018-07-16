---
title: 使用 PowerShell 将数据磁盘附加到 Azure 中的 Windows VM | Azure
description: 如何通过 Resource Manager 部署模型使用 PowerShell 将新磁盘或现有数据磁盘附加到 Windows VM。
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
origin.date: 10/11/2017
ms.date: 06/04/2018
ms.author: v-yeche
ms.openlocfilehash: d92114571e92784895bba45880e7ab8dd19360de
ms.sourcegitcommit: 00c8a6a07e6b98a2b6f2f0e8ca4090853bb34b14
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/11/2018
ms.locfileid: "38939770"
---
# <a name="attach-a-data-disk-to-a-windows-vm-using-powershell"></a>使用 PowerShell 将数据磁盘附加到 Windows VM

本文介绍如何使用 PowerShell 将新磁盘和现有磁盘附加到 Windows 虚拟机。 

在开始之前，请查看以下提示：
* 虚拟机的大小决定了可以附加多少个磁盘。 有关详细信息，请参阅[虚拟机大小](sizes.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)。
* 若要使用高级存储，需要支持高级存储的 VM 大小，如 DS 系列虚拟机。 有关详细信息，请参阅[高级存储：适用于 Azure 虚拟机工作负荷的高性能存储](premium-storage.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)。
<!-- Not Available on GS-series -->

<!--Not Available [!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]-->

如果选择在本地安装并使用 PowerShell，则本教程需要 Azure PowerShell 模块 6.0.0 或更高版本。 运行 ` Get-Module -ListAvailable AzureRM` 即可查找版本。 如果需要进行升级，请参阅 [Install Azure PowerShell module](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)（安装 Azure PowerShell 模块）。 如果在本地运行 PowerShell，则还需运行 `Connect-AzureRmAccount -Environment AzureChinaCloud ` 以创建与 Azure 的连接。

## <a name="add-an-empty-data-disk-to-a-virtual-machine"></a>将空数据磁盘添加到虚拟机

此示例演示了如何将空数据磁盘添加到现有的虚拟机。

### <a name="using-managed-disks"></a>使用托管磁盘

```powershell
$rgName = 'myResourceGroup'
$vmName = 'myVM'
$location = 'China East' 
$storageType = 'Premium_LRS'
$dataDiskName = $vmName + '_datadisk1'

$diskConfig = New-AzureRmDiskConfig -SkuName $storageType -Location $location -CreateOption Empty -DiskSizeGB 128
$dataDisk1 = New-AzureRmDisk -DiskName $dataDiskName -Disk $diskConfig -ResourceGroupName $rgName

$vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName 
$vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1

Update-AzureRmVM -VM $vm -ResourceGroupName $rgName
```

<!-- Not Available on ### Using managed disks in an Availability Zone-->
### <a name="initialize-the-disk"></a>初始化磁盘

添加空磁盘后，需要对其进行初始化。 要初始化该磁盘，可以登录到一个 VM，并使用磁盘管理进行初始化。 如果在创建 VM 时在其上启用了 WinRM 和证书，则可以通过远程 PowerShell 初始化该磁盘。 还可以使用自定义脚本扩展： 

```powershell
    $location = "location-name"
    $scriptName = "script-name"
    $fileName = "script-file-name"
    Set-AzureRmVMCustomScriptExtension -ResourceGroupName $rgName -Location $locName -VMName $vmName -Name $scriptName -TypeHandlerVersion "1.4" -StorageAccountName "mystore1" -StorageAccountKey "primary-key" -FileName $fileName -ContainerName "scripts"
```

脚本文件可以包含类似如下所示代码初始化磁盘：

```powershell
    $disks = Get-Disk | Where partitionstyle -eq 'raw' | sort number

    $letters = 70..89 | ForEach-Object { [char]$_ }
    $count = 0
    $labels = "data1","data2"

    foreach ($disk in $disks) {
        $driveLetter = $letters[$count].ToString()
        $disk | 
        Initialize-Disk -PartitionStyle MBR -PassThru |
        New-Partition -UseMaximumSize -DriveLetter $driveLetter |
        Format-Volume -FileSystem NTFS -NewFileSystemLabel $labels[$count] -Confirm:$false -Force
    $count++
    }
```

## <a name="attach-an-existing-data-disk-to-a-vm"></a>将现有数据磁盘附加到 VM

可以将现有托管磁盘作为数据磁盘附加到 VM。 

```powershell
$rgName = "myResourceGroup"
$vmName = "myVM"
$location = "China East" 
$dataDiskName = "myDisk"
$disk = Get-AzureRmDisk -ResourceGroupName $rgName -DiskName $dataDiskName 

$vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName 

$vm = Add-AzureRmVMDataDisk -CreateOption Attach -Lun 0 -VM $vm -ManagedDiskId $disk.Id

Update-AzureRmVM -VM $vm -ResourceGroupName $rgName
```

## <a name="next-steps"></a>后续步骤

创建[快照](snapshot-copy-managed-disk.md)。
<!--Update_Description: update meta properties, wording update -->