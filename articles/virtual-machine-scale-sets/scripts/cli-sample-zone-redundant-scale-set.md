---
title: Azure CLI 示例 - 区域冗余规模集 | Microsoft Docs
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
ms.openlocfilehash: 8e87f39e0c7504b4a0075dfb9655ec91273198e2
ms.sourcegitcommit: bfd0b25b0c51050e51531fedb4fca8c023b1bf5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52672502"
---
# <a name="create-a-zone-redundant-virtual-machine-scale-set-with-powershell"></a>使用 PowerShell 创建区域冗余虚拟机规模集
此脚本创建跨多个可用性区域运行 Ubuntu 的虚拟机规模集。 运行脚本后，可通过 RDP 访问虚拟机。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。

## <a name="sample-script"></a>示例脚本
```azurecli
#!/bin/bash

# Create a resource group
az group create --name myResourceGroup --location chinanorth

# Create a zone-redundant scale set across zones 1, 2, and 3
# This command also creates a 'Standard' SKU public IP address and load balancer
# For the Load Balancer Standard SKU, a Network Security Group and rules are also created
az vmss create `
  --resource-group myResourceGroup `
  --name myScaleSet `
  --image UbuntuLTS `
  --upgrade-policy-mode automatic `
  --admin-username azureuser `
  --generate-ssh-keys `
  --vm-sku Standard_DS1 `
  --zones 1 2 3

# Apply the Custom Script Extension that installs a basic Nginx webserver
az vmss extension set `
  --publisher Microsoft.Azure.Extensions `
  --version 2.0 `
  --name CustomScript `
  --resource-group myResourceGroup `
  --vmss-name myScaleSet `
  --settings '{"fileUris":["https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/automate_nginx.sh"],"commandToExecute":"./automate_nginx.sh"}'

# Create a Network Security Group rule to allow TCP port 80
az network nsg rule create `
  --resource-group myResourceGroup `
  --nsg-name myScaleSetNSG `
  --name http `
  --protocol Tcp `
  --direction Inbound `
  --access allow `
  --priority 1001 `
  --destination-port-range 80

# Output the public IP address to access the site in a web browser
az network public-ip show `
  --resource-group myResourceGroup `
  --name myScaleSetLBPublicIP `
  --query [ipAddress] `
  --output tsv
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