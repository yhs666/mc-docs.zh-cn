---
title: "Azure CLI 脚本示例 - 在相同订阅的存储帐户中从 VHD 文件创建托管磁盘 | Microsoft Docs"
description: "Azure CLI 脚本示例 - 在相同订阅的存储帐户中从 VHD 文件创建托管磁盘"
services: managed-disks-linux
documentationcenter: storage
author: forester123
manager: digimobile
editor: ramankum
tags: azure-service-management
ms.assetid: 
ms.service: managed-disks-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
origin.date: 05/19/2017
ms.date: 06/26/2017
ms.author: v-johch
ms.openlocfilehash: c9b090e3de628159beb338978974e1cd81db94b2
ms.sourcegitcommit: cc3f528827a8acd109ba793eee023b8c6b2b75e4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
# 使用 CLI 在相同订阅的存储帐户中从 VHD 文件创建托管磁盘
<a id="create-a-managed-disk-from-a-vhd-file-in-a-storage-account-in-the-same-subscription-with-cli" class="xliff"></a>

此脚本在相同订阅的存储帐户中从 VHD 文件创建托管磁盘。 使用此脚本将专用 VHD（未通用化/未进行 sysprep）导入到托管 OS 磁盘以创建虚拟机。 或使用它将数据 VHD 导入到托管数据磁盘。 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## 示例脚本
<a id="sample-script" class="xliff"></a>

```sh
#Provide the subscription Id
subscriptionId=mySubscriptionId

#Provide the name of your resource group.
#Ensure that resource group is already created 
resourceGroupName=myResourceGroupName

#Provide the name of the Managed Disk
diskName=myDiskName

#Provide the size of the disks in GB. It should be greater than the VHD file size.
diskSize=128


#Provide the URI of the VHD file that will be used to create Managed Disk. 
# VHD file can be deleted as soon as Managed Disk is created.
# e.g. https://contosostorageaccount1.blob.core.chinacloudapi.cn/vhds/contosovhd123.vhd 
vhdUri=https://contosostorageaccount1.blob.core.chinacloudapi.cn/vhds/contosoumd78620170425131836.vhd

#Provide the storage type for the Managed Disk. Premium_LRS or Standard_LRS.
storageType=Premium_LRS


#Provide the Azure location (e.g. westus) where Managed Disk will be located. 
#The location should be same as the location of the storage account where VHD file is stored.
#Get all the Azure location supported for your subscription using command below:
#az account list-locations
location='China East'

#Set the context to the subscription Id where Managed Disk will be created
az account set --subscription $subscriptionId

#Create the Managed disk from the VHD file 
az disk create --resource-group $resourceGroupName --name $diskName --sku $storageType --location $location --size-gb $diskSize --source $vhdUri

```


## 脚本说明
<a id="script-explanation" class="xliff"></a>

此脚本使用以下命令从 VHD 创建托管磁盘。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 说明 |
|---|---|
| [az disk create](https://docs.microsoft.com/cli/azure/disk#create) | 在相同订阅的存储帐户中使用 VHD 的 URI 创建托管磁盘 |

## 后续步骤
<a id="next-steps" class="xliff"></a>

[通过将托管磁盘附加为 OS 磁盘来创建虚拟机](./../../virtual-machines/scripts/virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.microsoft.com/cli/azure/overview)。

可以在 [Azure Linux VM 文档](../../virtual-machines/linux/cli-samples.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)中找到其他虚拟机和托管磁盘 CLI 脚本示例。
