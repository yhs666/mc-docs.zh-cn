<!-- not suitable for Mooncake -->

---
title: 创建用于备份的 Azure 托管磁盘的副本 | Azure
description: 了解如何创建 Azure 托管磁盘的副本，以便将其用于备份或排查磁盘问题。
documentationcenter: ''
author: cwatsonMSFT
manager: timlt
editor: ''
tags: azure-resource-manager

ms.assetid: 15eb778e-fc07-45ef-bdc8-9090193a6d20
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 2/9/2017
wacn.date: 03/20/2017
ms.author: cwatson
---

# 使用托管快照创建作为 Azure 托管磁盘存储的 VHD 的副本
创建托管磁盘的快照进行备份，或者从快照创建托管磁盘，然后将其附加到测试虚拟机进行故障诊断。托管快照是 VM 托管磁盘的完整时间点副本。它将创建 VHD 的只读副本，默认情况下，将它存储为标准托管磁盘。有关托管磁盘的详细信息，请参阅 [Azure 托管磁盘概述](../storage/storage-managed-disks-overview.md)

有关定价的信息，请参阅 [Azure 存储定价](https://www.azure.cn/pricing/details/managed-disks/)。

## 开始之前
如果使用 PowerShell，请确保使用的是最新版本的 AzureRM.Compute PowerShell 模块。运行以下命令来安装该模块。

```
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```

有关详细信息，请参阅 [Azure PowerShell 版本控制](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/#azure-powershell-versioning)。

## 使用快照复制 VHD
使用 Azure 门户预览或 PowerShell 创建托管磁盘的快照。

### 使用 Azure 门户预览创建快照 

1. 登录 [Azure 门户预览](https://portal.azure.cn)。
2. 从左上角开始，单击“新建”并搜索“快照”。
3. 在“快照”边栏选项卡中，单击“创建”。
4. 输入快照的**名称**。
5. 选择现有的[资源组](../azure-resource-manager/resource-group-overview.md#resource-groups)，或键入新资源组的名称。
6. 选择 Azure 数据中心位置。
7. 对于“源磁盘”，选择要创建快照的托管磁盘。
8. 选择用于存储快照的“帐户类型”。建议选择 **Standard\_LRS**，除非需要将快照存储在高性能磁盘上。
9. 单击“创建”。

### 使用 PowerShell 创建快照
以下步骤演示如何获取要复制的 VHD 磁盘，如何创建快照配置，以及如何使用 New-AzureRmSnapshot cmdlet<!--Add link to cmdlet when available--> 创建磁盘的快照。

1. 设置一些参数。

    ```
    $resourceGroupName = 'myResourceGroup' 
    $location = 'chinaeast' 
    $dataDiskName = 'ContosoMD_datadisk1' 
    $snapshotName = 'ContosoMD_datadisk1_snapshot1'  
    ```

    替换参数值：

    -  将“myResourceGroup”替换为 VM 的资源组。
    -  将“chinaeast”替换为要将托管快照存储到的地理位置。<!---How do you look these up? -->
    -  将“ContosoMD\_datadisk1”替换为要复制的 VHD 磁盘的名称。
    -  将“ContosoMD\_datadisk1\_snapshot1”替换为要用于新快照的名称。

2. 获取要复制的 VHD 磁盘。

    $disk = Get-AzureRmDisk -ResourceGroupName $resourceGroupName -DiskName $dataDiskName

3. 创建快照配置。

    $snapshot = New-AzureRmSnapshotConfig -SourceUri $disk.Id -CreateOption Copy -Location $location

4. 创建快照。

    New-AzureRmSnapshot -Snapshot $snapshot -SnapshotName $snapshotName -ResourceGroupName $resourceGroupName

如果计划使用快照创建托管磁盘并将其附加到需要高性能的 VM 上，请将参数 `-AccountType Premium_LRS` 用于 New-AzureRmSnapshot 命令。该参数创建快照，使其作为高级托管磁盘进行存储。高级托管磁盘比标准托管磁盘开销大。因此，在使用该参数之前，请确保确实需要“高级”。

<!---HONumber=Mooncake_0313_2017-->