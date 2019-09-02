---
title: Azure CLI 脚本示例 - 使用流量管理器在全球范围内缩放应用 | Azure Docs
description: Azure CLI 脚本示例 - 缩放具有高可用性体系结构的全球应用服务应用
services: appservice
documentationcenter: appservice
author: msangapu
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: e4033a50-0e05-4505-8ce8-c876204b2acc
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
origin.date: 12/11/2017
ms.date: 01/21/2019
ms.author: v-biyu
ms.custom: seodec18
ms.openlocfilehash: 4c06181631c2535044488342a1e447e189fbf7c6
ms.sourcegitcommit: 90d5f59427ffa599e8ec005ef06e634e5e843d1e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2019
ms.locfileid: "54083874"
---
# <a name="scale-an-app-service-app-worldwide-with-a-high-availability-architecture-using-azure-cli"></a>使用 Azure CLI 缩放具有高可用性体系结构的全球应用服务应用

此示例脚本将创建一个资源组、两个应用服务计划、两个应用、一个流量管理器配置文件和两个流量管理器终结点。 完成本练习后，会获得一个高可用性体系结构，该体系结构基于最低网络延迟提供应用的全局可用性。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

如果选择在本地安装并使用 CLI，则需使用 Azure CLI 2.0 或更高版本。 若要查找版本，请运行 `az --version`。 如果需要进行安装或升级，请参阅[安装 Azure CLI 2.0](https://docs.azure.cn/zh-cn/cli/install-azure-cli?view=azure-cli-lastest)。

## <a name="sample-script"></a>示例脚本

```azurecli
#/bin/bash

# Variables
trafficManagerDnsName="myDnsName$random"
app1Name="AppServiceTM1$random"
app2Name="AppServiceTM2$random"
location1="ChinaNorth"
location2="ChinaEast"

# Create a Resource Group
az group create --name myResourceGroup --location $location1

# Create a Traffic Manager Profile
az network traffic-manager profile create --name $trafficManagerDnsName-tmp --resource-group myResourceGroup --routing-method Performance --unique-dns-name $trafficManagerDnsName

# Create App Service Plans in two Regions
az appservice plan create --name $app1Name-Plan --resource-group myResourceGroup --location $location1 --sku S1
az appservice plan create --name $app2Name-Plan --resource-group myResourceGroup --location $location2 --sku S1

# Add a Web App to each App Service Plan
site1=$(az webapp create --name $app1Name --plan $app1Name-Plan --resource-group myResourceGroup --query id --output tsv)
site2=$(az webapp create --name $app2Name --plan $app2Name-Plan --resource-group myResourceGroup --query id --output tsv)

# Assign each Web App as an Endpoint for high-availabilty
az network traffic-manager endpoint create -n $app1Name-$location1 --profile-name $trafficManagerDnsName-tmp -g myResourceGroup --type azureEndpoints --target-resource-id $site1
az network traffic-manager endpoint create -n $app2Name-$location2 --profile-name $trafficManagerDnsName-tmp -g myResourceGroup --type azureEndpoints --target-resource-id $site2
```

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建资源组、应用服务应用、流量管理器配置文件和所有相关资源。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [`az group create`](https://docs.azure.cn/cli/group?view=azure-cli-latest#az_group_create) | 创建用于存储所有资源的资源组。 |
| [`az appservice plan create`](https://docs.azure.cn/zh-cn/cli/appservice/plan?view=azure-cli-latest#az_appservice_plan_create) | 创建应用服务计划。 |
| [`az webapp create`](https://docs.azure.cn/zh-cn/cli/webapp?view=azure-cli-latest#az_webapp_create) | 创建 Azure Web 应用。 |
| [`az network traffic-manager profile create`](https://docs.azure.cn/zh-cn/cli/network/traffic-manager/profile?view=azure-cli-latest#az_network_traffic_manager_profile_create) | 创建 Azure 流量管理器配置文件。 |
| [`az network traffic-manager endpoint create`](https://docs.azure.cn/zh-cn/cli/network/traffic-manager/endpoint?view=azure-cli-latest#az_network_traffic_manager_endpoint_create) | 将终结点添加到 Azure 流量管理器配置文件。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/zh-cn/cli/overview?view=azure-cli-lastest)。

可以在 [Azure 应用服务文档](../samples-cli.md)中找到其他应用服务 CLI 脚本示例。

<!--Update_Description: add a note about Azure CLI 2.0 version-->