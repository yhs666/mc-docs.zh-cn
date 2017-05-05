<!-- not suitable for Mooncake -->

---
title: 从 Azure 中的 VHD 创建托管磁盘 | Azure
description: 使用 Resource Manager 部署模型从目前在 Azure 存储帐户中的 VHD 创建托管磁盘。
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager

ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/05/2017
wacn.date: 03/20/2017
ms.author: cynthn
---

# 从存储帐户中的非托管磁盘创建托管磁盘

可以从现有数据磁盘或当前在 Azure 存储帐户中的 OS 磁盘创建托管磁盘。还可以创建可用作 VM 的新数据磁盘的空磁盘。

## 开始之前
如果使用 PowerShell，请确保使用的是最新版本的 AzureRM.Compute PowerShell 模块。运行以下命令来安装该模块。

```
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```

有关详细信息，请参阅 [Azure PowerShell 版本控制](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/#azure-powershell-versioning)。

## 从 Azure 存储帐户中的 VHD 创建托管磁盘

在此示例中，我们将从 VHD 创建磁盘作为托管磁盘，并将其分配给参数 **$disk1**，以便以后使用。

托管磁盘将在**美国西部**位置名为 **myResourceGroup** 的资源组中创建。该磁盘将名为 **myDisk**，并且将从以前上载到名为 **mystorageaccount** 的存储帐户的名为 **myDisk.vhd** 的 VHD 文件创建。托管磁盘将在高级本地冗余存储 (LRS) 中创建。StandardLRS 和 PremiumLRS 是可用于托管磁盘的唯一 **-AccountType** 选项。

1.  设置一些参数

    ```
    $rgName = "myResourceGroup"
    $location = "West China North"
    $diskName = "myDisk"
    $vhdUri = "https://mystorageaccount.blob.core.chinacloudapi.cn/vhds/myDisk.vhd"
    ```

2. 创建数据磁盘。

    ```
    $disk1 = New-AzureRmDisk -DiskName $diskName -Disk (New-AzureRmDiskConfig -AccountType PremiumLRS -Location $location -CreateOption Import -SourceUri $vhdUri) -ResourceGroupName $rgName
    ```

## 创建空数据磁盘作为托管磁盘

在此示例中，我们将创建空数据磁盘作为托管磁盘，并将其分配给参数 **$dataDisk2**，以便以后使用。将空数据磁盘附加到正在运行的 VM 后，需要登录到 VM，使用 diskmgmt.msc 或[远程使用 WinRM 和脚本](../virtual-machines-windows-attach-disk-ps.md#initialize-the-disk)对该磁盘进行初始化。

空数据磁盘将在**中国西北部**位置名为 **myResourceGroup** 的资源组中创建。磁盘将名为 **myEmptyDataDisk**。空磁盘将在高级本地冗余存储 (LRS) 中创建。StandardLRS 和 PremiumLRS 是可用于托管磁盘的唯一 **-AccountType** 选项。

此示例中的磁盘大小为 128GB，但应选择满足 VM 上运行的任何应用程序需求的大小。

1.  设置一些参数

    ```
    $rgName = "myResourceGroup"
    $location = "West China North"
    $dataDiskName = "myEmptyDataDisk"
    ```

2. 创建数据磁盘。

    ```
    $dataDisk2 = New-AzureRmDisk -DiskName $dataDiskName -Disk (New-AzureRmDiskConfig -AccountType PremiumLRS -Location $location -CreateOption Empty -DiskSizeGB 128) -ResourceGroupName $rgName
    ```

## 后续步骤	
- 如果你已有一个 VM，则可以[附加数据磁盘](../virtual-machines-windows-attach-disk-portal.md)。

<!---HONumber=Mooncake_0313_2017-->