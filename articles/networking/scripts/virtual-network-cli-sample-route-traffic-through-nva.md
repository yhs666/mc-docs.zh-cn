---
title: Azure CLI 脚本示例 - 通过网络虚拟设备路由流量
description: Azure CLI 脚本示例 - 通过防火墙网络虚拟设备路由流量。
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
ms.workload: infrastructure
origin.date: 05/16/2017
ms.date: 01/07/2019
ms.author: v-biyu
ms.openlocfilehash: 1056193e4aaa6f2ba46f823b1b3ad93bd53f90e1
ms.sourcegitcommit: a46f12240aea05f253fb4445b5e88564a2a2a120
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/26/2018
ms.locfileid: "53785301"
---
# <a name="route-traffic-through-a-network-virtual-appliance"></a>通过网络虚拟设备路由流量

该脚本示例创建了包含前端和后端子网的虚拟网络。 它还会创建一个 VM，并启用 IP 转发，在两个子网之间路由流量。 运行脚本后，可将网络软件（例如防火墙应用程序）部署到 VM。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a>示例脚本


#<a name="binbash"></a>!/bin/bash

RgName="MyResourceGroup" Location="chinaeast"

# <a name="create-a-resource-group"></a>创建资源组。
az group create \
  --name $RgName \
  --location $Location

# <a name="create-a-virtual-network-with-a-front-end-subnet"></a>创建包含前端子网的虚拟网络。
az network vnet create \
  --name MyVnet \
  --resource-group $RgName \
  --location $Location \
  --address-prefix 10.0.0.0/16 \
  --subnet-name MySubnet-FrontEnd \
  --subnet-prefix 10.0.1.0/24

# <a name="create-a-network-security-group-for-the-front-end-subnet-allowing-http-and-https-inbound"></a>为前端子网创建允许 HTTP 和 HTTPS 入站流量的网络安全组。
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
  --destination-port-range 80 az network nsg rule create \
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

# <a name="associate-the-front-end-nsg-to-the-front-end-subnet"></a>将前端 NSG 关联到前端子网。
az network vnet subnet update \
  --vnet-name MyVnet \
  --name MySubnet-FrontEnd \
  --resource-group $RgName \
  --network-security-group MyNsg-FrontEnd

# <a name="create-the-back-end-subnet"></a>创建后端子网。
az network vnet subnet create \
  --address-prefix 10.0.2.0/24 \
  --name MySubnet-BackEnd \
  --resource-group $RgName \
  --vnet-name MyVnet

#<a name="create-the-dmz-subnet"></a>创建外围网络子网。
az network vnet subnet create \
  --address-prefix 10.0.0.0/24 \
  --name MySubnet-Dmz \
  --resource-group $RgName \
  --vnet-name MyVnet

# <a name="create-a-public-ip-address-for-the-firewall-vm"></a>为防火墙 VM 创建一个公共 IP 地址。
az network public-ip create \
  --resource-group $RgName \
  --name MyPublicIP-Firewall

# <a name="create-a-nic-for-the-firewall-vm-and-enable-ip-forwarding"></a>为防火墙 VM 创建一个 NIC 并启用 IP 转发。
az network nic create \
  --resource-group $RgName \
  --name MyNic-Firewall \
  --vnet-name MyVnet \
  --subnet MySubnet-Dmz \
  --public-ip-address MyPublicIp-Firewall \
  --ip-forwarding

#<a name="create-a-firewall-vm-to-accept-all-traffic-between-the-front-and-back-end-subnets"></a>创建一个防火墙 VM，接受前端和后端子网之间的所有流量。
az vm create \
  --resource-group $RgName \
  --name MyVm-Firewall \
  --nics MyNic-Firewall \
  --image UbuntuLTS \
  --admin-username azureadmin \
  --generate-ssh-keys

# <a name="get-the-private-ip-address-from-the-vm-for-the-user-defined-route"></a>从 VM 获取用户定义的路由的专用 IP 地址。
Fw1Ip=$(az vm list-ip-addresses \
  --resource-group $RgName \
  --name MyVm-Firewall \
  --query [].virtualMachine.network.privateIpAddresses[0] --out tsv)

# <a name="create-route-table-for-the-frontend-subnet"></a>创建 FrontEnd 子网的路由表。
az network route-table create \
  --name MyRouteTable-FrontEnd \
  --resource-group $RgName

# <a name="create-a-route-for-traffic-from-the-front-end-to-the-back-end-subnet-through-the-firewall-vm"></a>创建一个路由，该路由适用于通过防火墙 VM 从前端子网发往后端子网的流量。
az network route-table route create \
  --name RouteToBackEnd \
  --resource-group $RgName \
  --route-table-name MyRouteTable-FrontEnd \
  --address-prefix 10.0.2.0/24 \
  --next-hop-type VirtualAppliance \
  --next-hop-ip-address $Fw1Ip
  
# <a name="create-a-route-for-traffic-from-the-front-end-subnet-to-the-internet-through-the-firewall-vm"></a>创建一个路由，该路由适用于通过防火墙 VM 从前端子网发往 Internet 的流量。
az network route-table route create \
  --name RouteToInternet \
  --resource-group $RgName \
  --route-table-name MyRouteTable-FrontEnd \
  --address-prefix 0.0.0.0/0 \
  --next-hop-type VirtualAppliance \
  --next-hop-ip-address $Fw1Ip

# <a name="associate-the-route-table-to-the-frontend-subnet"></a>将路由表关联到 FrontEnd 子网。
az network vnet subnet update \
  --name MySubnet-FrontEnd \
  --vnet-name MyVnet \
  --resource-group $RgName \
  --route-table MyRouteTable-FrontEnd

# <a name="create-route-table-for-the-backend-subnet"></a>创建 BackEnd 子网的路由表。
az network route-table create \
  --name MyRouteTable-BackEnd \
  --resource-group $RgName
  
# <a name="create-a-route-for-traffic-from-the-back-end-subnet-to-the-front-end-subnet-through-the-firewall-vm"></a>创建一个路由，该路由适用于通过防火墙 VM 从后端子网发往前端子网的流量。
az network route-table route create \
  --name RouteToFrontEnd \
  --resource-group $RgName \
  --route-table-name MyRouteTable-BackEnd \
  --address-prefix 10.0.1.0/24 \
  --next-hop-type VirtualAppliance \
  --next-hop-ip-address $Fw1Ip

# <a name="create-a-route-for-traffic-from-the-back-end-subnet-to-the-internet-through-the-firewall-vm"></a>创建一个路由，该路由适用于通过防火墙 VM 从后端子网发往 Internet 的流量。
az network route-table route create \
  --name RouteToInternet \
  --resource-group $RgName \
  --route-table-name MyRouteTable-BackEnd \
  --address-prefix 0.0.0.0/0 \
  --next-hop-type VirtualAppliance \
  --next-hop-ip-address $Fw1Ip

# <a name="associate-the-route-table-to-the-backend-subnet"></a>将路由表关联到 BackEnd 子网。
az network vnet subnet update \
  --name MySubnet-BackEnd \
  --vnet-name MyVnet \
  --resource-group $RgName \
  --route-table MyRouteTable-BackEnd

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
| [az network subnet create](/cli/network/vnet/subnet#az_network_vnet_subnet_create) | 创建后端子网和 DMZ 子网。 |
| [az network public-ip create](/cli/network/public-ip#az_network_public_ip_create) | 创建用于从 Internet 访问 VM 的公共 IP 地址。 |
| [az network nic create](/cli/network/nic#az_network_nic_create) | 创建虚拟网络接口，并对它启用 IP 转发。 |
| [az network nsg create](/cli/network/nsg#az_network_nsg_create) | 创建网络安全组 (NSG)。 |
| [az network nsg rule create](/cli/network/nsg/rule#az_network_nsg_rule_create) | 创建允许 HTTP 和 HTTPS 端口入站到 VM 的 NSG 规则。 |
| [az network vnet subnet update](/cli/network/vnet/subnet#az_network_vnet_subnet_update)| 将 NSG 和路由表关联到子网。 |
| [az network route-table create](/cli/network/route-table#az-network-route-table-create)| 为所有路由创建路由表。 |
| [az network route-table route create](/cli/network/route-table/route#az-network-route-table-route-create)| 创建路由，通过 VM 在子网和 Internet 之间路由流量。 |
| [az vm create](/cli/vm#az_vm_create) | 创建虚拟机并向其附加 NIC。 此命令还指定要使用的虚拟机映像和管理凭据。 |
| [az group delete](/cli/group#az_group_delete) | 删除资源组及其包含的所有资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](/cli)。

可在 [Azure 网络概述文档](../cli-samples.md)中找到其他网络 CLI 脚本示例