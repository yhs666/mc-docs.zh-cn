---
title: Azure CLI 脚本示例 - 对等互连两个虚拟网络
description: Azure CLI 脚本示例 - 对等互连两个虚拟网络
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: timlt
editor: tysonn
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
origin.date: 05/16/2017
ms.date: 01/07/2019
ms.author: v-biyu
ms.openlocfilehash: 4d10b22cc5419d9eb89e66cc3809916c21641d46
ms.sourcegitcommit: b8fb6890caed87831b28c82738d6cecfe50674fd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/29/2019
ms.locfileid: "58625586"
---
# <a name="peer-two-virtual-networks"></a>对等互连两个虚拟网络

此脚本通过 Azure 网络在同一区域创建并连接两个虚拟网络。 运行脚本后，会在两个虚拟网络之间创建对等互连。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a>示例脚本

```bash
# !/bin/bash

RgName="MyResourceGroup"
Location="chinaeast"

# Create a resource group.
az group create \
  --name $RgName \
  --location $Location

# Create virtual network 1.
az network vnet create \
  --name Vnet1 \
  --resource-group $RgName \
  --location $Location \
  --address-prefix 10.0.0.0/16

# Create virtual network 2.
az network vnet create \
  --name Vnet2 \
  --resource-group $RgName \
  --location $Location \
  --address-prefix 10.1.0.0/16

# Get the id for VNet1.
VNet1Id=$(az network vnet show \
  --resource-group $RgName \
  --name Vnet1 \
  --query id --out tsv)

# Get the id for VNet2.
VNet2Id=$(az network vnet show \
  --resource-group $RgName \
  --name Vnet2 \
  --query id \
  --out tsv)

# Peer VNet1 to VNet2.
az network vnet peering create \
  --name LinkVnet1ToVnet2 \
  --resource-group $RgName \
  --vnet-name VNet1 \
  --remote-vnet-id $VNet2Id \
  --allow-vnet-access

# Peer VNet2 to VNet1.
az network vnet peering create \
  --name LinkVnet2ToVnet1 \
  --resource-group $RgName \
  --vnet-name VNet2 \
  --remote-vnet-id $VNet1Id \
  --allow-vnet-access
```

## <a name="clean-up-deployment"></a>清理部署 

运行以下命令来删除资源组、VM 和所有相关资源。

```azurecli
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建资源组、虚拟机和所有相关资源。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [az group create](https://docs.azure.cn/zh-cn/cli/group#az_group_create) | 创建用于存储所有资源的资源组。 |
| [az network vnet create](https://docs.azure.cn/zh-cn/cli/network/vnet#az_network_vnet_create) | 创建 Azure 虚拟网络和子网。 |
| [az network vnet peering create](https://docs.azure.cn/zh-cn/cli/network/vnet/peering#az_network_vnet_peering_create) | 创建两个虚拟网络之间的对等互连。  |
| [az group delete](https://docs.azure.cn/zh-cn/cli/vm/extension#az_vm_extension_set) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/zh-cn/cli)。

可在 [Azure 网络概述文档](../cli-samples.md)中找到其他网络 CLI 脚本示例。
