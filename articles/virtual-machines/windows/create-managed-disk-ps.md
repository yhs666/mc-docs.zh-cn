---
title: "从 Azure 中的 VHD 创建托管磁盘 | Azure"
description: "使用 Resource Manager 部署模型从目前在 Azure 存储帐户中的 VHD 创建托管磁盘。"
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
origin.date: 02/05/2017
ms.date: 06/20/2017
ms.author: v-dazen
ms.openlocfilehash: 2744f94e0f39670c57b2c20d969a998c894a99cd
ms.sourcegitcommit: b1d2bd71aaff7020dfb3f7874799e03df3657cd4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
# <a name="create-managed-disks-from-unmanaged-disks-in-a-storage-account"></a>从存储帐户中的非托管磁盘创建托管磁盘

可以从现有数据磁盘或当前在 Azure 存储帐户中的 OS 磁盘创建托管磁盘。 也可以创建可用作 VM 的新数据磁盘的空磁盘。 

## <a name="before-you-begin"></a>开始之前
如果使用 PowerShell，请确保使用的是最新版本的 AzureRM.Compute PowerShell 模块。 运行以下命令来安装该模块。

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
有关详细信息，请参阅 [Azure PowerShell 版本控制](https://docs.microsoft.com/powershell/azure/overview)。

## <a name="create-a-managed-disk-from-a-vhd-in-an-azure-storage-account"></a>从 Azure 存储帐户中的 VHD 创建托管磁盘

在此示例中，我们将从 VHD 创建磁盘作为托管磁盘，并将其分配给参数 **$disk1** ，以便以后使用。 

托管磁盘将在“美国西部”位置名为 **myResourceGroup** 的资源组中创建。 该磁盘命名为 **myDisk**，它将从名为 **myDisk.vhd** 的 VHD 文件创建，该文件在之前已上传到名为 **mystorageaccount** 的存储帐户。 托管磁盘将在高级本地冗余存储 (LRS) 中创建。 StandardLRS 和 PremiumLRS 是可用于托管磁盘的唯一 **-AccountType** 选项。 

1.  设置一些参数

    ```powershell
    $rgName = "myResourceGroup"
    $location = "China North"
    $diskName = "myDisk"
    $vhdUri = "https://mystorageaccount.blob.core.chinacloudapi.cn/vhds/myDisk.vhd"
    ```

2. 创建数据磁盘。 
    ```powershell
    $disk1 = New-AzureRmDisk -DiskName $diskName -Disk (New-AzureRmDiskConfig -AccountType PremiumLRS -Location $location -CreateOption Import -SourceUri $vhdUri) -ResourceGroupName $rgName
    ```

## <a name="create-an-empty-data-disk-as-a-managed-disk"></a>创建空数据磁盘作为托管磁盘

在此示例中，我们将创建空数据磁盘作为托管磁盘，并将其分配给参数 **$dataDisk2** ，以便以后使用。 将空数据磁盘附加到正在运行的 VM 后，需要登录到 VM，使用 diskmgmt.msc 或[远程使用 WinRM 和脚本](attach-disk-ps.md#initialize-the-disk)对该磁盘进行初始化。

空数据磁盘将在“中国北部”位置名为 **myResourceGroup** 的资源组中创建。 磁盘将名为 **myEmptyDataDisk**。 空磁盘将在高级本地冗余存储 (LRS) 中创建。 StandardLRS 和 PremiumLRS 是可用于托管磁盘的唯一 **-AccountType** 选项。

此示例中的磁盘大小为 128GB，但应选择满足 VM 上运行的任何应用程序需求的大小。

1.  设置一些参数

    ```powershell
    $rgName = "myResourceGroup"
    $location = "China North"
    $dataDiskName = "myEmptyDataDisk"
    ```

2. 创建数据磁盘。
    ```powershell
    $dataDisk2 = New-AzureRmDisk -DiskName $dataDiskName -Disk (New-AzureRmDiskConfig -AccountType PremiumLRS -Location $location -CreateOption Empty -DiskSizeGB 128) -ResourceGroupName $rgName
    ```

## <a name="next-steps"></a>后续步骤   
- 如果已有一个 VM，则可以[附加数据磁盘](attach-disk-portal.md)。
