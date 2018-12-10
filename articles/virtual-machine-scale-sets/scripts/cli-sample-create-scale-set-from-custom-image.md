---
title: Azure CLI 示例 - 使用自定义 VM 映像 | Microsoft Docs
description: Azure CLI 示例
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
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
ms.date: 11/14/2018
ms.author: v-junlch
ms.custom: mvc
ms.openlocfilehash: 052d2ebf97a60e4d1b2d96e1479592eb26b8b182
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52657168"
---
# <a name="create-a-virtual-machine-scale-set-from-a-custom-vm-image-with-the-azure-cli"></a>使用 Azure CLI 基于自定义 VM 映像创建虚拟机规模集
此脚本创建使用自定义 VM 映像作为 VM 实例源的虚拟机规模集。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。

## <a name="sample-script"></a>示例脚本
```azurecli
#!/bin/bash

# Create a resource group
az group create --name myResourceGroup --location chinanorth

# Create a scale set

# Custom VM image must already exist in your subscription
# See https://docs.microsoft.com/en-us/azure/virtual-machine-scale-sets/tutorial-use-custom-image-cli

# Network resources such as an Azure load balancer are automatically created
az vmss create `
  --resource-group myResourceGroup `
  --name myScaleSet `
  --image myImage `
  --upgrade-policy-mode automatic `
  --admin-username azureuser `
  --generate-ssh-keys `
  --vm-sku Standard_DS1
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
| [az group delete](/cli/ad/group#delete) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤
有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](/cli/overview)。

可以在 [Azure 虚拟机规模集文档](../cli-samples.md)中找到其他虚拟机规模集 Azure CLI 脚本示例。

<!-- Update_Description: wording update -->