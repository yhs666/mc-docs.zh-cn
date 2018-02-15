---
title: "使用 Azure CLI 2.0 创建虚拟机规模集 | Microsoft Docs"
description: "了解如何使用 Azure PowerShell 快速创建虚拟机规模集"
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: get-started-article
origin.date: 12/19/2017
ms.date: 01/29/2018
ms.author: v-junlch
ms.openlocfilehash: 39bff5eda26ac182f8f350872414aa0ea8b75373
ms.sourcegitcommit: 3629fd4a81f66a7d87a4daa00471042d1f79c8bb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2018
---
# <a name="create-a-virtual-machine-scale-set-with-the-azure-cli-20"></a>使用 Azure CLI 2.0 创建虚拟机规模集
利用虚拟机规模集，可以部署和管理一组相同的、自动缩放的虚拟机。 可以手动缩放规模集中的 VM 数，也可以定义规则，以便根据资源使用情况（如 CPU 使用率、内存需求或网络流量）进行自动缩放。 在此入门文章中，可以使用 Azure CLI 2.0 创建虚拟机规模集。 也可使用 [Azure PowerShell](virtual-machine-scale-sets-create-powershell.md) 或 [Azure 门户](virtual-machine-scale-sets-create-portal.md)创建规模集。

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。

如果选择在本地安装并使用 CLI，本教程要求运行 Azure CLI 2.0.20 或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI 2.0]( /cli/install-azure-cli)。 


## <a name="create-a-scale-set"></a>创建规模集
使用 [az group create](/cli/group#create) 创建资源组，然后才能创建规模集。 以下示例在“chinanorth”位置创建名为“myResourceGroup”的资源组：

```azurecli 
az group create --name myResourceGroup --location chinanorth
```

现在，使用 [az vmss create](/cli/vmss#create) 创建虚拟机规模集。 以下示例创建名为“myScaleSet”的规模集，并生成 SSH 密钥（如果不存在）：

```azurecli 
az vmss create \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --admin-username azureuser \
  --generate-ssh-keys
```

创建和配置所有的规模集资源和 VM 需要几分钟时间。


## <a name="install-nginx-webserver"></a>安装 NGINX Web 服务器
若要测试规模集，请使用自定义脚本扩展下载并运行一个可在 VM 实例上安装 NGINX 的脚本。 此扩展适用于部署后配置、软件安装或其他任何配置/管理任务。 有关详细信息，请参阅[自定义脚本扩展概述](../virtual-machines/linux/extensions-customscript.md)。

应用可安装 NGINX 的自定义脚本扩展，如下所示：

```azurecli
az vmss extension set \
  --publisher Microsoft.Azure.Extensions \
  --version 2.0 \
  --name CustomScript \
  --resource-group myResourceGroup \
  --vmss-name myScaleSet \
  --settings '{"fileUris":["https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/automate_nginx.sh"],"commandToExecute":"./automate_nginx.sh"}'
```


## <a name="allow-web-traffic"></a>允许 Web 流量
若要允许通信流到达 Web 服务器，请使用 [az network lb rule create](/cli/network/lb/rule#create) 命令创建负载均衡器规则。 以下示例创建名为“myLoadBalancerRuleWeb”的规则：

```azurecli 
az network lb rule create \
  --resource-group myResourceGroup \
  --name myLoadBalancerRuleWeb \
  --lb-name myScaleSetLB \
  --backend-pool-name myScaleSetLBBEPool \
  --backend-port 80 \
  --frontend-ip-name loadBalancerFrontEnd \
  --frontend-port 80 \
  --protocol tcp
```


## <a name="test-your-web-server"></a>测试 Web 服务器
若要查看运行中的 Web 服务器，请使用 [az network public-ip show](/cli/network/public-ip#show) 命令获取负载均衡器的公共 IP 地址。 以下示例获取创建为规模集一部分的“myScaleSetLBPublicIP”的 IP 地址：

```azurecli 
az network public-ip show \
  --resource-group myResourceGroup \
  --name myScaleSetLBPublicIP \
  --query [ipAddress] \
  --output tsv
```

将负载均衡器的公共 IP 地址输入到 Web 浏览器中。 负载均衡器将流量分发到某个 VM 实例，如以下示例所示：

![NGINX 中的默认网页](./media/virtual-machine-scale-sets-create-cli/running-nginx-site.png)


## <a name="clean-up-resources"></a>清理资源
如果不再需要资源组、规模集和所有相关的资源，可以使用 [az group delete](/cli/group#delete) 命令将其删除，如下所示：

```azurecli 
az group delete --name myResourceGroup
```


## <a name="next-steps"></a>后续步骤
此快速入门文章介绍了如何创建基本的规模集，以及如何使用自定义脚本扩展在 VM 实例上安装基本的 NGINX Web 服务器。 若要改进可伸缩性和自动化，请阅读以下操作指南文章，了解如何扩展规模集：

- [在虚拟机规模集上部署应用程序](virtual-machine-scale-sets-deploy-app.md)
- 通过 [Azure CLI](virtual-machine-scale-sets-autoscale-cli.md) 或 [Azure PowerShell](virtual-machine-scale-sets-autoscale-powershell.md) 自动进行缩放

