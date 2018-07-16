---
title: Azure CLI 脚本示例 - 将托管磁盘复制（移动）到相同或不同的订阅 | Azure
description: Azure CLI 脚本示例 - 将托管磁盘复制（移动）到相同或不同的订阅
services: virtual-machines-linux
documentationcenter: storage
author: rockboyfor
manager: digimobile
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
origin.date: 05/19/2017
ms.date: 04/16/2018
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: 273c302020652672bc1de3d83423541e28c353d7
ms.sourcegitcommit: 00c8a6a07e6b98a2b6f2f0e8ca4090853bb34b14
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/11/2018
ms.locfileid: "38939930"
---
# <a name="copy-managed-disks-to-same-or-different-subscription-with-cli"></a>使用 CLI 将托管磁盘复制到相同或不同的订阅

此脚本会将托管磁盘复制到位于同一区域的相同或不同的订阅中。 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>示例脚本

```azurecli
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

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令，通过源托管磁盘的 ID 在目标订阅中创建新的托管磁盘。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [az disk show](https://docs.azure.cn/zh-cn/cli/disk?view=azure-cli-latest#az_disk_show) | 使用托管磁盘的名称和资源组属性获取该托管磁盘的所有属性。 使用 ID 属性将托管磁盘复制到其他订阅。  |
| [az disk create](https://docs.azure.cn/zh-cn/cli/disk?view=azure-cli-latest#az_disk_create) | 通过使用父托管磁盘的 ID 和名称在不同订阅中创建新的托管磁盘来复制该托管磁盘。  |

## <a name="next-steps"></a>后续步骤

[基于托管磁盘创建虚拟机](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/zh-cn/cli/overview?view=azure-cli-latest)。

可以在 [Azure Linux VM 文档](../../app-service/app-service-cli-samples.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)中找到其他虚拟机和托管磁盘 CLI 脚本示例。

<!--Update_Description: update meta properties -->