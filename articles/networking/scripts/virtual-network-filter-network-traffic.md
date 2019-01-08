---
title: Azure CLI 脚本示例 - 筛选 VM 网络流量 | Microsoft Docs
description: Azure CLI 脚本示例 - 筛选入站和出站 VM 网络流量。
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: timlt
editor: tysonn
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: ''
origin.date: 07/07/2017
ms.date: 01/07/2019
ms.author: v-biyu
ms.openlocfilehash: 07efe04da63306a09f6afaa68096e223e66b16a8
ms.sourcegitcommit: a46f12240aea05f253fb4445b5e88564a2a2a120
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/26/2018
ms.locfileid: "53785295"
---
# <a name="filter-inbound-and-outbound-vm-network-traffic"></a>筛选入站和出站 VM 网络流量

该脚本示例创建了包含前端和后端子网的虚拟网络。 前端子网的入站网络流量仅限于 HTTP、HTTPS 和 SSH，而从后端子网到 Internet 的出站流量则不受限制。 运行该脚本后，将具有一个包含两个 NIC 的虚拟机。 每个 NIC 连接到不同的子网。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>示例脚本


#<a name="binbash"></a>!/bin/bash

RgName="MyResourceGroup" Location="chinaeast"

# <a name="create-a-resource-group"></a>创建资源组。
az group create \
  --name $RgName \
  --location $Location

# <a name="create-a-virtual-network-and-a-front-end-subnet"></a>创建虚拟网络和前端子网。
az network vnet create \
  --resource-group $RgName \
  --name MyVnet \
  --address-prefix 10.0.0.0/16  \
  --location $Location \
  --subnet-name MySubnet-FrontEnd \
  --subnet-prefix 10.0.1.0/24

# <a name="create-a-back-end-subnet"></a>创建后端子网。
az network vnet subnet create \
  --address-prefix 10.0.2.0/24 \
  --name MySubnet-BackEnd \
  --resource-group $RgName \
  --vnet-name MyVnet

# <a name="create-a-network-security-group-nsg-for-the-front-end-subnet"></a>为前端子网创建网络安全组 (NSG)。
az network nsg create \
  --resource-group $RgName \
  --name MyNsg-FrontEnd \
  --location $Location

# <a name="create-nsg-rules-to-allow-http--https-traffic-inbound"></a>创建允许 HTTP 和 HTTPS 入站流量的 NSG 规则。
az network nsg rule create \
  --resource-group $RgName \
  --nsg-name MyNsg-FrontEnd \
  --name Allow-HTTP-All \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 100 \
  --source-address-prefix Internet \
  --source-port-range "*" \ --destination-address-prefix "*" \
  --destination-port-range 80

az network nsg rule create \
  --resource-group $RgName \
  --nsg-name MyNsg-FrontEnd \
  --name Allow-HTTPS-All \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 200 \
  --source-address-prefix Internet \
  --source-port-range "*" \ --destination-address-prefix "*" \
  --destination-port-range 443

# <a name="create-an-nsg-rule-to-allow-ssh-traffic-in-from-the-internet-to-the-front-end-subnet"></a>创建允许 SSH 流量从 Internet 流入到前端子网的 NSG 规则。
az network nsg rule create \
  --resource-group $RgName \
  --nsg-name MyNsg-FrontEnd \
  --name Allow-SSH-All \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 300 \
  --source-address-prefix Internet \
  --source-port-range "*" \ --destination-address-prefix "*" \
  --destination-port-range 22

# <a name="associate-the-front-end-nsg-to-the-front-end-subnet"></a>将前端 NSG 关联到前端子网。
az network vnet subnet update \
  --vnet-name MyVnet \
  --name MySubnet-FrontEnd \
  --resource-group $RgName \
  --network-security-group MyNsg-FrontEnd

# <a name="create-a-network-security-group-for-the-back-end-subnet"></a>为后端子网创建网络安全组。
az network nsg create \
  --resource-group $RgName \
  --name MyNsg-BackEnd \
  --location $Location

# <a name="create-an-nsg-rule-to-block-all-outbound-traffic-from-the-back-end-subnet-to-the-internet-inbound-blocked-by-default"></a>创建一项 NSG 规则，阻止从后端子网流向 Internet 的所有出站流量（默认情况下会阻止入站流量）。
az network nsg rule create \
  --resource-group $RgName \
  --nsg-name MyNsg-BackEnd \
  --name Deny-Internet-All \
  --access Deny --protocol Tcp \
  --direction Outbound --priority 100 \
  --source-address-prefix "*" \ --source-port-range "*" \
  --destination-address-prefix "Internet" \
  --destination-port-range "*"

# <a name="associate-the-back-end-nsg-to-the-back-end-subnet"></a>将后端 NSG 关联到后端子网。
az network vnet subnet update \
  --vnet-name MyVnet \
  --name MySubnet-BackEnd \
  --resource-group $RgName \
  --network-security-group MyNsg-BackEnd

# <a name="create-a-public-ip-address-for-the-vm-front-end-network-interface"></a>为 VM 前端网络接口创建公共 IP 地址。
az network public-ip create \
  --resource-group $RgName \
  --name MyPublicIp-FrontEnd \
  --allocation-method Dynamic

# <a name="create-a-network-interface-for-the-vm-attached-to-the-front-end-subnet"></a>为附加到前端子网的 VM 创建网络接口。
az network nic create \
  --resource-group $RgName \
  --vnet-name MyVnet \
  --subnet MySubnet-FrontEnd \
  --name MyNic-FrontEnd \
  --public-ip-address MyPublicIp-FrontEnd

# <a name="create-a-network-interface-for-the-vm-attached-to-the-back-end-subnet"></a>为附加到后端子网的 VM 创建网络接口。
az network nic create \
  --resource-group $RgName \
  --vnet-name MyVnet \
  --subnet MySubnet-BackEnd \
  --name MyNic-BackEnd

# <a name="create-the-vm-with-both-the-frontend-and-backend-nics"></a>创建带 FrontEnd NIC 和 BackEnd NIC 的 VM。
az vm create \
  --resource-group $RgName \
  --name MyVm \
  --nics MyNic-FrontEnd MyNic-BackEnd \
  --image UbuntuLTS \
  --admin-username azureadmin \
  --generate-ssh-keys

## <a name="clean-up-deployment"></a>清理部署 

运行以下命令来删除资源组、VM 和所有相关资源。

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建资源组、虚拟网络和网络安全组。 表中的每条命令链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [az group create](/cli/group#az_group_create) | 创建用于存储所有资源的资源组。 |
| [az network vnet create](/cli/network/vnet#az_network_vnet_create) | 创建 Azure 虚拟网络和前端子网。 |
| [az network subnet create](/cli/network/vnet/subnet#az_network_vnet_subnet_create) | 创建后端子网。 |
| [az network vnet subnet update](/cli/network/vnet/subnet#az_network_vnet_subnet_update) | 将 NSG 关联到子网。 |
| [az network public-ip create](/cli/network/public-ip#az_network_public_ip_create) | 创建用于从 Internet 访问 VM 的公共 IP 地址。 |
| [az network nic create](/cli/network/nic#az_network_nic_create) | 创建虚拟网络接口，并将其附加到虚拟网络的前端和后端子网。 |
| [az network nsg create](/cli/network/nsg#az_network_nsg_create) | 创建关联到前端和后端子网的网络安全组 (NSG)。 |
| [az network nsg rule create](/cli/network/nsg/rule#az_network_nsg_rule_create) |创建 NSG 规则，允许或阻止特定子网的特定端口。 |
| [az vm create](/cli/vm#az_vm_create) | 创建虚拟机，并将 NIC 附加到每个 VM。 此命令还指定要使用的虚拟机映像和管理凭据。 |
| [az group delete](/cli/group#az_group_delete) | 删除资源组及其包含的所有资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](/cli/)。

可在 [Azure 网络概述文档](../cli-samples.md)中找到其他网络 CLI 脚本示例