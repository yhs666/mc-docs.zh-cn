---
title: "Azure CLI 脚本示例 - 将托管磁盘复制（移动）到相同或不同的订阅 | Microsoft Docs"
description: "Azure CLI 脚本示例 - 将托管磁盘复制（移动）到相同或不同的订阅"
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
ms.openlocfilehash: 0ed107e742bf133d2037967829889a6db610415f
ms.sourcegitcommit: cc3f528827a8acd109ba793eee023b8c6b2b75e4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
# 使用 CLI 将托管磁盘复制到相同或不同的订阅
<a id="copy-managed-disks-to-same-or-different-subscription-with-cli" class="xliff"></a>

此脚本会将托管磁盘复制到位于同一区域的相同或不同的订阅中。 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## 示例脚本
<a id="sample-script" class="xliff"></a>

```sh
#Provide the subscription Id of the subscription where managed disk exists
sourceSubscriptionId=<your_subscription_id>

#Provide the name of your resource group where managed disk exists
sourceResourceGroupName=mySourceResourceGroupName

#Provide the name of the managed disk
managedDiskName=myDiskName

#Set the context to the subscription Id where managed disk exists
az account set --subscription $sourceSubscriptionId

#Get the managed disk Id 
managedDiskId=$(az disk show --name $managedDiskName --resource-group $sourceResourceGroupName --query [id] -o tsv)

#If managedDiskId is blank then it means that managed disk does not exist.
echo 'source managed disk Id is: ' $managedDiskId

#Provide the subscription Id of the subscription where managed disk will be copied to
targetSubscriptionId=<target_subscription_id>

#Name of the resource group where managed disk will be copied to
targetResourceGroupName=mytargetResourceGroupName

#Set the context to the subscription Id where managed disk will be copied to
az account set --subscription $targetSubscriptionId

#Copy managed disk to different subscription using managed disk Id
az disk create --resource-group $targetResourceGroupName --name $managedDiskName --source $managedDiskId
```


## 脚本说明
<a id="script-explanation" class="xliff"></a>

此脚本使用以下命令，通过源托管磁盘的 ID 在目标订阅中创建新的托管磁盘。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 说明 |
|---|---|
| [az disk show](https://docs.microsoft.com/cli/azure/disk#show) | 使用托管磁盘的名称和资源组属性获取该托管磁盘的所有属性。 使用 ID 属性将托管磁盘复制到其他订阅。  |
| [az disk create](https://docs.microsoft.com/cli/azure/disk#create) | 通过使用父托管磁盘的 ID 和名称在不同订阅中创建新的托管磁盘来复制该托管磁盘。  |

## 后续步骤
<a id="next-steps" class="xliff"></a>

[从托管磁盘创建虚拟机](./../../virtual-machines/scripts/virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.microsoft.com/cli/azure/overview)。

可以在 [Azure Linux VM 文档](../../virtual-machines/linux/cli-samples.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)中找到其他虚拟机和托管磁盘 CLI 脚本示例。
