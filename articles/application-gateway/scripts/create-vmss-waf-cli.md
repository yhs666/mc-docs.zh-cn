---
title: Azure CLI 脚本示例 - 限制 Web 流量 | Microsoft Docs
description: Azure CLI 脚本示例 - 使用 Web 应用程序防火墙和使用 OWASP 规则限制流量的虚拟机规模集创建应用程序网关。
services: application-gateway
documentationcenter: networking
author: vhorne
manager: jpconnock
editor: tysonn
tags: azure-resource-manager
ms.service: application-gateway
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
origin.date: 01/29/2018
ms.date: 02/26/2019
ms.author: v-junlch
ms.custom: mvc
ms.openlocfilehash: 0a0501390e2713e086f1e82baa184310cc488ad1
ms.sourcegitcommit: e9f088bee395a86c285993a3c6915749357c2548
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/26/2019
ms.locfileid: "56836976"
---
# <a name="restrict-web-traffic-using-the-azure-cli"></a>使用 Azure CLI 限制 Web 流量

此脚本创建具有 Web 应用程序防火墙且使用虚拟机规模集作为后端服务器的应用程序网关。 Web 应用程序防火墙基于 OWASP 规则限制 Web 流量。 在运行脚本后，可以使用应用程序网关的公用 IP 地址测试该网关。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。

## <a name="sample-script"></a>示例脚本

```azurecli 
# Create a resource group
az group create --name myResourceGroupAG --location chinanorth

# Create network resources
az network vnet create `
  --name myVNet `
  --resource-group myResourceGroupAG `
  --location chinanorth `
  --address-prefix 10.0.0.0/16 `
  --subnet-name myAGSubnet `
  --subnet-prefix 10.0.1.0/24
az network vnet subnet create `
  --name myBackendSubnet `
  --resource-group myResourceGroupAG `
  --vnet-name myVNet `
  --address-prefix 10.0.2.0/24
az network public-ip create `
  --resource-group myResourceGroupAG `
  --name myAGPublicIPAddress

# Create the application gateway with WAF
az network application-gateway create `
  --name myAppGateway `
  --location chinanorth `
  --resource-group myResourceGroupAG `
  --vnet-name myVNet `
  --subnet myAGSubnet `
  --capacity 2 `
  --sku WAF_Medium `
  --http-settings-cookie-based-affinity Disabled `
  --frontend-port 80 `
  --http-settings-port 80 `
  --http-settings-protocol Http `
  --public-ip-address myAGPublicIPAddress
az network application-gateway waf-config set `
  --enabled true `
  --gateway-name myAppGateway `
  --resource-group myResourceGroupAG `
  --firewall-mode Detection `
  --rule-set-version 3.0

# Create a virtual machine scale set
az vmss create `
  --name myvmss `
  --resource-group myResourceGroupAG `
  --image UbuntuLTS `
  --admin-username azureuser `
  --admin-password Azure123456! `
  --instance-count 2 `
  --vnet-name myVNet `
  --subnet myBackendSubnet `
  --vm-sku Standard_DS2 `
  --upgrade-policy-mode Automatic `
  --app-gateway myAppGateway `
  --backend-pool-name appGatewayBackendPool

# Install NGINX
az vmss extension set `
  --publisher Microsoft.Azure.Extensions `
  --version 2.0 `
  --name CustomScript `
  --resource-group myResourceGroupAG `
  --vmss-name myvmss `
  --settings "{ 'fileUris': ['https://raw.githubusercontent.com/davidmu1/samplescripts/master/install_nginx.sh'], 'commandToExecute': './install_nginx.sh' }"

# Get the IP address
az network public-ip show `
  --resource-group myResourceGroupAG `
  --name myAGPublicIPAddress `
  --query [ipAddress] `
  --output tsv
```

## <a name="clean-up-deployment"></a>清理部署 

运行以下命令来删除资源组、应用程序网关和所有相关资源。

```azurecli 
az group delete --name myResourceGroupAG --yes
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建部署。 表中的每一项均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [az group create](/cli/group#az-group-create) | 创建用于存储所有资源的资源组。 |
| [az network vnet create](/cli/network/vnet) | 创建虚拟网络。 |
| [az network vnet subnet create](/cli/network/vnet/subnet#az-network-vnet-subnet-create) | 在虚拟网络中创建子网。 |
| [az network public-ip create](/cli/network/public-ip?view=azure-cli-latest) | 创建应用程序网关的公用 IP 地址。 |
| [az network application-gateway create](/cli/network/application-gateway?view=azure-cli-latest) | 创建应用程序网关。 |
| [az vmss create](/cli/vmss#az-vmss-create) | 创建虚拟机规模集。 |
| [az storage account create](/cli/storage/account#az-storage-account-create) | 创建存储帐户。 |
| [az monitor diagnostic-settings create](/cli/monitor/diagnostic-settings#az-monitor-diagnostic-settings-create) | 创建存储帐户。 |
| [az network public-ip show](/cli/network/public-ip#az-network-public-ip-show) | 获取应用程序网关的公共 IP 地址。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](/cli/overview)。

可以在 [Azure 应用程序网关文档](../cli-samples.md)中找到其他应用程序网关 CLI 脚本示例。

<!-- Update_Description: link update -->