---
title: "创建用于备份的 Azure 托管磁盘的副本 | Azure"
description: "了解如何创建 Azure 托管磁盘的副本，以便将其用于备份或排查磁盘问题。"
documentationcenter: 
author: bbetstcw
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 15eb778e-fc07-45ef-bdc8-9090193a6d20
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
origin.date: 02/09/2017
ms.date: 06/20/2017
ms.author: v-dazen
ms.openlocfilehash: 4d9b6605b0cdb2f9841f43ca7817fe3f82e56608
ms.sourcegitcommit: b3e981fc35408835936113e2e22a0102a2028ca0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/30/2017
---
# <a name="create-a-copy-of-a-vhd-stored-as-an-azure-managed-disk-by-using-managed-snapshots"></a>使用托管快照创建作为 Azure 托管磁盘存储的 VHD 的副本
创建托管磁盘的快照进行备份，或者从快照创建托管磁盘，然后将其附加到测试虚拟机进行故障诊断。 托管快照是 VM 托管磁盘的完整时间点副本。 它将创建 VHD 的只读副本，默认情况下，将它存储为标准托管磁盘。 有关托管磁盘的详细信息，请参阅 [Azure 托管磁盘概述](../../storage/storage-managed-disks-overview.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)

有关定价的详细信息，请参阅 [Azure 存储定价](https://www.azure.cn/pricing/details/managed-disks/)。 

## <a name="before-you-begin"></a>开始之前
如果使用 PowerShell，请确保使用的是最新版本的 AzureRM.Compute PowerShell 模块。 运行以下命令来安装该模块。

```
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
有关详细信息，请参阅 [Azure PowerShell 版本控制](https://docs.microsoft.com/powershell/azure/overview)。

## <a name="copy-the-vhd-with-a-snapshot"></a>使用快照复制 VHD
使用 PowerShell 创建托管磁盘的快照。

### <a name="use-powershell-to-take-a-snapshot"></a>使用 PowerShell 创建快照
以下步骤演示如何获取要复制的 VHD 磁盘，如何创建快照配置，以及如何使用 New-AzureRmSnapshot cmdlet<!--Add link to cmdlet when available-->创建磁盘的快照。 

1. 设置一些参数。 

  ```powershell
  $resourceGroupName = 'myResourceGroup' 
  $location = 'chinaeast' 
  $dataDiskName = 'ContosoMD_datadisk1' 
  $snapshotName = 'ContosoMD_datadisk1_snapshot1'  
  ```
  替换参数值：
  -  将“myResourceGroup”替换为 VM 的资源组。
  -  将“chinaeast”替换为要将托管快照存储到的地理位置。 <!---How do you look these up? -->
  -  将“ContosoMD_datadisk1”替换为想要复制的 VHD 磁盘的名称。
  -  将“ContosoMD_datadisk1_snapshot1”替换为要用于新快照的名称。

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
如果计划使用快照创建托管磁盘并将其附加到需要高性能的 VM 上，请将参数 `-AccountType Premium_LRS` 用于 New-AzureRmSnapshot 命令。 该参数创建快照，使其作为高级托管磁盘进行存储。 高级托管磁盘比标准托管磁盘开销大。 因此，在使用该参数之前，请确保确实需要“高级”。