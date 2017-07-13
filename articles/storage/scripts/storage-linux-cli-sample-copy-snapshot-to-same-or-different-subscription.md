---
title: "Azure CLI 脚本示例 - 使用 CLI 将托管磁盘的快照复制（移动）到相同或不同的订阅 | Microsoft Docs"
description: "Azure CLI 脚本示例 - 使用 CLI 将托管磁盘的快照复制（移动）到相同或不同的订阅"
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
ms.openlocfilehash: eef5ea5cb358612c51e996895ebdd5171ca87094
ms.sourcegitcommit: cc3f528827a8acd109ba793eee023b8c6b2b75e4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
# 使用 CLI 将托管磁盘的快照复制到相同或不同的订阅
<a id="copy-snapshot-of-a-managed-disk-to-same-or-different-subscription-with-cli" class="xliff"></a>

此脚本会将托管磁盘的快照复制到相同或不同的订阅。 使用此脚本将快照移到父快照所在区域中的不同订阅。


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## 示例脚本
<a id="sample-script" class="xliff"></a>

```sh
#Provide the subscription Id of the subscription where snapshot exists
sourceSubscriptionId=<your_subscription_id>

#Provide the name of your resource group where snapshot exists
sourceResourceGroupName=mySourceResourceGroupName

#Provide the name of the snapshot
snapshotName=mySnapshotName

#Set the context to the subscription Id where snapshot exists
az account set --subscription $sourceSubscriptionId

#Get the snapshot Id 
snapshotId=$(az snapshot show --name $snapshotName --resource-group $sourceResourceGroupName --query [id] -o tsv)

#If snapshotId is blank then it means that snapshot does not exist.
echo 'source snapshot Id is: ' $snapshotId

#Provide the subscription Id of the subscription where snapshot will be copied to
#If snapshot is copied to the same subscription then you can skip this step
targetSubscriptionId=<target_subscription_id>

#Name of the resource group where snapshot will be copied to
targetResourceGroupName=mytargetResourceGroupName

#Set the context to the subscription Id where snapshot will be copied to
#If snapshot is copied to the same subscription then you can skip this step
az account set --subscription $targetSubscriptionId

#Copy snapshot to different subscription using the snapshot Id
az snapshot create --resource-group $targetResourceGroupName --name $snapshotName --source $snapshotId

```


## 脚本说明
<a id="script-explanation" class="xliff"></a>

此脚本使用以下命令，通过源快照的 ID 在目标订阅中创建快照。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 说明 |
|---|---|
| [az snapshot show](https://docs.microsoft.com/cli/azure/snapshot#show) | 使用快照的名称和资源组属性获取该快照的所有属性。 使用 ID 属性将快照复制到其他订阅。  |
| [az snapshot create](https://docs.microsoft.com/cli/azure/snapshot#create) | 通过使用父快照的 ID 和名称在不同订阅中创建快照来复制快照。  |

## 后续步骤
<a id="next-steps" class="xliff"></a>

[从快照创建虚拟机](./../../virtual-machines/scripts/virtual-machines-linux-cli-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.microsoft.com/cli/azure/overview)。

可以在 [Azure Linux VM 文档](../../virtual-machines/linux/cli-samples.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)中找到其他虚拟机和托管磁盘 CLI 脚本示例。
