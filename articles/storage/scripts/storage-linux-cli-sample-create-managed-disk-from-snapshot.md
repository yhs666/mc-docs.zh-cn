---
title: "Azure CLI 脚本示例 - 从快照创建托管磁盘 | Microsoft Docs"
description: "Azure CLI 脚本示例 - 从快照创建托管磁盘"
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
ms.openlocfilehash: 2d5137beac84682a9fb43b16edc40b5ec455e98e
ms.sourcegitcommit: cc3f528827a8acd109ba793eee023b8c6b2b75e4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
# 使用 CLI 从快照创建托管磁盘
<a id="create-a-managed-disk-from-a-snapshot-with-cli" class="xliff"></a>

此脚本从快照创建托管磁盘。 使用它从 OS 和数据磁盘的快照还原虚拟机。 从各自的快照创建 OS 和数据托管磁盘，然后通过附加托管磁盘创建新的虚拟机。 还可以通过附加从快照创建的数据磁盘还原现有 VM 的数据磁盘。


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## 示例脚本
<a id="sample-script" class="xliff"></a>

```sh
#Provide the subscription Id of the subscription where you want to create Managed Disks
subscriptionId=<your_subscription_id>

#Provide the name of your resource group
resourceGroupName=myResourceGroupName

#Provide the name of the snapshot that will be used to create Managed Disks
snapshotName=mySnapshotName

#Provide the name of the new Managed Disks that will be create
diskName=myDiskName

#Provide the size of the disks in GB. It should be greater than the VHD file size.
diskSize=128

#Provide the storage type for Managed Disk. Premium_LRS or Standard_LRS.
storageType=Premium_LRS

#Set the context to the subscription Id where Managed Disk will be created
az account set --subscription $subscriptionId

#Get the snapshot Id 
snapshotId=$(az snapshot show --name $snapshotName --resource-group $resourceGroupName --query [id] -o tsv)

#Create a new Managed Disks using the snapshot Id
#Note that managed disk will be created in the same location as the snapshot
az disk create --resource-group $resourceGroupName --name $diskName --sku $storageType --size-gb $diskSize --source $snapshotId


```


## 脚本说明
<a id="script-explanation" class="xliff"></a>

此脚本使用以下命令从快照创建托管磁盘。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 说明 |
|---|---|
| [az snapshot show](https://docs.microsoft.com/cli/azure/snapshot#show) | 使用快照的名称和资源组属性获取该快照的所有属性。 使用 ID 属性创建托管磁盘。  |
| [az disk create](https://docs.microsoft.com/cli/azure/disk#create) | 使用托管快照的快照 ID 创建托管磁盘 |

## 后续步骤
<a id="next-steps" class="xliff"></a>

[通过将托管磁盘附加为 OS 磁盘来创建虚拟机](./../../virtual-machines/scripts/virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.microsoft.com/cli/azure/overview)。

可以在 [Azure Linux VM 文档](../../virtual-machines/linux/cli-samples.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)中找到其他虚拟机和托管磁盘 CLI 脚本示例。
