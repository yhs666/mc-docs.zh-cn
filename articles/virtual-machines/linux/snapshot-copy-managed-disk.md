---
title: "复制用于备份的 Azure 托管磁盘 | Azure"
description: "了解如何创建 Azure 托管磁盘的副本，以便将其用于备份或排查磁盘问题。"
documentationcenter: 
author: squillace
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
origin.date: 02/06/2017
ms.date: 06/20/2017
ms.author: v-dazen
ms.openlocfilehash: e608fbdc3fe8ab3c450bb8f71a6d4b76ef415a75
ms.sourcegitcommit: b1d2bd71aaff7020dfb3f7874799e03df3657cd4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
# 使用托管快照创建作为 Azure 托管磁盘存储的 VHD 的副本
<a id="create-a-copy-of-a-vhd-stored-as-an-azure-managed-disk-by-using-managed-snapshots" class="xliff"></a>
拍摄托管磁盘的快照进行备份，或者从快照创建托管磁盘，然后将其附加到测试虚拟机进行故障诊断。 托管快照是 VM 托管磁盘的完整时间点副本。 它会创建 VHD 的只读副本，并将其默认存储为标准托管磁盘。 

有关定价的详细信息，请参阅 [Azure 存储定价](https://www.azure.cn/pricing/details/managed-disks/)。 <!--Add link to topic or blog post that explains managed disks. -->

使用 Azure CLI 2.0 获取托管磁盘的快照。

## 使用 Azure CLI 2.0 拍摄快照
<a id="use-azure-cli-20-to-take-a-snapshot" class="xliff"></a>

> [!NOTE] 
> 以下示例需要安装 Azure CLI 2.0 并登录到 Azure 帐户。

以下步骤说明如何使用带有 `--source-disk` 参数的 `az snapshot create` 命令获取和拍摄托管操作系统磁盘的快照。 以下示例假设在 `myResourceGroup` 资源组中存在使用托管操作系统磁盘创建的名为 `myVM` 的VM。

```azure-cli
# take the disk id with which to create a snapshot
osDiskId=$(az vm show -g myResourceGroup -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
az snapshot create -g myResourceGroup --source "$osDiskId" --name osDisk-backup
```

输出应如下所示：

```json
{
  "accountType": "Standard_LRS",
  "creationData": {
    "createOption": "Copy",
    "imageReference": null,
    "sourceResourceId": null,
    "sourceUri": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/disks/osdisk_6NexYgkFQU",
    "storageAccountId": null
  },
  "diskSizeGb": 30,
  "encryptionSettings": null,
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/snapshots/osDisk-backup",
  "location": "chinanorth",
  "name": "osDisk-backup",
  "osType": "Linux",
  "ownerId": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "tags": null,
  "timeCreated": "2017-02-06T21:27:10.172980+00:00",
  "type": "Microsoft.Compute/snapshots"
}
```

如果计划使用快照创建托管磁盘并将其附加到需要具有高性能的 VM，请结合使用 `az snapshot create` 命令和参数 `--sku Premium_LRS`。 这样就会创建可以作为高级托管磁盘存储的快照。 高级托管磁盘性能更好，因为它们是固态硬盘 (SSD)，但成本高于标准磁盘 (HDD)。