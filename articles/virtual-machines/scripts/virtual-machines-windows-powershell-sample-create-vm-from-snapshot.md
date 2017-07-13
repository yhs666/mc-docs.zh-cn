---
title: "Azure PowerShell 脚本示例 - 从快照创建 VM | Azure"
description: "Azure PowerShell 脚本示例 - 从快照创建 VM"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: ramankum
manager: kavithag
editor: ramankum
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
origin.date: 05/10/2017
ms.date: 07/03/2017
ms.author: v-dazen
ms.custom: mvc
ms.openlocfilehash: 70564337753ee62296ad3fc2afd99e312674462d
ms.sourcegitcommit: b1d2bd71aaff7020dfb3f7874799e03df3657cd4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
# 使用 PowerShell 从快照创建虚拟机
<a id="create-a-virtual-machine-from-a-snapshot-with-powershell" class="xliff"></a>

此脚本从 OS 磁盘的快照创建虚拟机。 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## 示例脚本
<a id="sample-script" class="xliff"></a>

```powershell
#Provide the subscription Id
$subscriptionId = 'yourSubscriptionId'

#Provide the name of your resource group
$resourceGroupName ='yourResourceGroupName'

#Provide the name of the snapshot that will be used to create OS disk
$snapshotName = 'yourSnapshotName'

#Provide the name of the OS disk that will be created using the snapshot
$osDiskName = 'yourOSDiskName'

#Provide the name of an existing virtual network where virtual machine will be created
$virtualNetworkName = 'yourVNETName'

#Provide the name of the virtual machine
$virtualMachineName = 'yourVMName'

#Provide the size of the virtual machine
#e.g. Standard_DS3
#Get all the vm sizes in a region using below script:
#e.g. Get-AzureRmVMSize -Location chinanorth
$virtualMachineSize = 'Standard_DS3'

#Set the context to the subscription Id where Managed Disk will be created
Select-AzureRmSubscription -SubscriptionId $SubscriptionId

$snapshot = Get-AzureRmSnapshot -ResourceGroupName $resourceGroupName -SnapshotName $snapshotName 

$diskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location $snapshot.Location -SourceResourceId $snapshot.Id -CreateOption Copy

$disk = New-AzureRmDisk -Disk $diskConfig -ResourceGroupName $resourceGroupName -DiskName $osDiskName

#Initialize virtual machine configuration
$VirtualMachine = New-AzureRmVMConfig -VMName $virtualMachineName -VMSize $virtualMachineSize

#Use the Managed Disk Resource Id to attach it to the virtual machine. Please change the OS type to linux if OS disk has linux OS
$VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -ManagedDiskId $disk.Id -CreateOption Attach -Linux

#Create a public IP for the VM  
$publicIp = New-AzureRmPublicIpAddress -Name ($VirtualMachineName.ToLower()+'_ip') -ResourceGroupName $resourceGroupName -Location $snapshot.Location -AllocationMethod Dynamic

#Get the virtual network where virtual machine will be hosted
$vnet = Get-AzureRmVirtualNetwork -Name $virtualNetworkName -ResourceGroupName $resourceGroupName

# Create NIC in the first subnet of the virtual network 
$nic = New-AzureRmNetworkInterface -Name ($VirtualMachineName.ToLower()+'_nic') -ResourceGroupName $resourceGroupName -Location $snapshot.Location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $publicIp.Id

$VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $nic.Id

#Create the virtual machine with Managed Disk
New-AzureRmVM -VM $VirtualMachine -ResourceGroupName $resourceGroupName -Location $snapshot.Location
```

## 清理部署
<a id="clean-up-deployment" class="xliff"></a> 

运行以下命令来删除资源组、VM 和所有相关资源。

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## 脚本说明
<a id="script-explanation" class="xliff"></a>

此脚本使用以下命令获取快照属性、从快照创建托管磁盘并创建 VM。 表中的每一项均链接到特定于命令的文档。

| 命令 | 说明 |
|---|---|
| [Get-AzureRmSnapshot](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermsnapshot) | 使用快照名称获取快照。 |
| [New-AzureRmDiskConfig](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermdiskconfig) | 创建磁盘配置。 在磁盘创建过程中将使用此配置。 |
| [New-AzureRmDisk](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermdisk) | 创建托管磁盘。 |
| [New-AzureRmVMConfig](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvmconfig) | 创建 VM 配置。 此配置包括 VM 名称、操作系统和管理凭据等信息。 在创建 VM 期间将使用此配置。 |
| [Set-AzureRmVMOSDisk](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmosdisk) | 将托管磁盘作为 OS 磁盘附加到虚拟机 |
| [New-AzureRmPublicIpAddress](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermpublicipaddress) | 创建公共 IP 地址。 |
| [New-AzureRmNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermnetworkinterface) | 创建网络接口。 |
| [New-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvm) | 创建虚拟机。 |
|[Remove-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/remove-azurermresourcegroup) | 删除资源组及其中包含的所有资源。 |

## 后续步骤
<a id="next-steps" class="xliff"></a>

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure Windows VM 文档](../windows/powershell-samples.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)中找到其他虚拟机 PowerShell 脚本示例。