---
title: Azure CLI 示例 - 附加并使用数据磁盘 | Microsoft Docs
description: Azure CLI 示例
services: virtual-machine-scale-sets
documentationcenter: ''
author: zr-msft
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 03/27/2018
ms.date: 11/27/2018
ms.author: v-junlch
ms.custom: mvc
ms.openlocfilehash: af0067168e60da1230fabb62748b29aeca8c5675
ms.sourcegitcommit: bfd0b25b0c51050e51531fedb4fca8c023b1bf5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52672474"
---
# <a name="attach-and-use-data-disks-with-a-virtual-machine-scale-set-with-the-azure-cli"></a>使用 Azure CLI 为虚拟机规模集附加并使用数据磁盘
此脚本创建虚拟机规模集并附加和准备数据磁盘。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。

## <a name="sample-script"></a>示例脚本
```azurecli
#!/bin/bash

# Create a resource group
az group create --name myResourceGroup --location chinanorth

# Create a scale set
# Network resources such as an Azure load balancer are automatically created
# Two data disks are created and attach - a 64Gb disk and a 128Gb disk
az vmss create `
  --resource-group myResourceGroup `
  --name myScaleSet `
  --image UbuntuLTS `
  --upgrade-policy-mode automatic `
  --admin-username azureuser `
  --generate-ssh-keys `
  --vm-sku Standard_DS1 `
  --data-disk-sizes-gb 64 128

# Attach an additional 128Gb data disk
az vmss disk attach `
  --resource-group myResourceGroup `
  --name myScaleSet `
  --size-gb 128

# Install the Azure Custom Script Extension to run a script that prepares the data disks
az vmss extension set `
  --publisher Microsoft.Azure.Extensions `
  --version 2.0 `
  --name CustomScript `
  --resource-group myResourceGroup `
  --vmss-name myScaleSet `
  --settings "{'fileUris':['https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/prepare_vm_disks.sh'],'commandToExecute':'./prepare_vm_disks.sh'}"
```

## <a name="clean-up-deployment"></a>清理部署
运行以下命令可删除资源组、规模集和所有相关资源。

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>脚本说明
此脚本使用以下命令创建资源组、虚拟机规模集和所有相关资源。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [az group create](/cli/ad/group#az_ad_group_create) | 创建用于存储所有资源的资源组。 |
| [az vmss create](/cli/vmss#az_vmss_create) | 创建虚拟机规模集并将其连接到虚拟网络、子网和网络安全组。 负载均衡器也会被创建，以将流量分配到多个 VM 实例。 此命令还指定要使用的 VM 映像和管理凭据。  |
| [az vmss disk attach](/cli/vmss/disk#az_vmss_disk_attach) | 创建数据磁盘并将其附加到虚拟机规模集。 |
| [az vmss extension set](/cli/vmss/extension#az_vmss_extension_set) | 安装 Azure 自定义脚本扩展以运行脚本，从而在每个 VM 实例上准备数据磁盘。 |
| [az group delete](/cli/ad/group#delete) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤
有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](/cli/overview)。

可以在 [Azure 虚拟机规模集文档](../cli-samples.md)中找到其他虚拟机规模集 Azure CLI 脚本示例。

<!-- Update_Description: update metedata properties -->