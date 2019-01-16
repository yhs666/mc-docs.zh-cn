---
title: Azure CLI 脚本示例 - 使用 Azure CLI 手动缩放应用 | Azure Docs
description: Azure CLI 脚本示例 - 使用 Azure CLI 手动缩放应用服务
services: appservice
documentationcenter: appservice
author: msangapu
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: 251d9074-8fff-4121-ad16-9eca9556ac96
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
origin.date: 12/11/2017
ms.date: 01/21/2019
ms.author: v-biyu
ms.custom: seodec18
ms.openlocfilehash: 396e5139c8fc96bc88a35feeda8db44913861e1d
ms.sourcegitcommit: 90d5f59427ffa599e8ec005ef06e634e5e843d1e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2019
ms.locfileid: "54083879"
---
# <a name="scale-an-app-service-app-manually-using-azure-cli"></a>使用 Azure CLI 手动缩放应用服务应用

此示例脚本将创建一个资源组、一个应用服务计划和一个应用。 然后，它将该应用服务计划从单个实例扩展到多个实例。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


如果选择在本地安装并使用 CLI，则需使用 Azure CLI 2.0 或更高版本。 若要查找版本，请运行 `az --version`。 如果需要进行安装或升级，请参阅[安装 Azure CLI 2.0](https://docs.azure.cn/zh-cn/cli/install-azure-cli?view=azure-cli-lastest)。

## <a name="sample-script"></a>示例脚本

```azurecli
#/bin/bash

# Variables
appName="AppServiceManualScale$random"
location="ChinaNorth"

# Create a Resource Group
az group create --name myResourceGroup --location $location

# Create App Service Plans
az appservice plan create --name AppServiceManualScalePlan --resource-group myResourceGroup --location $location --sku B1

# Add a Web App
az webapp create --name $appName --plan AppServiceManualScalePlan --resource-group myResourceGroup

# Scale Web App to 2 Workers
az appservice plan update --number-of-workers 2 --name AppServiceManualScalePlan --resource-group myResourceGroup
```

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建资源组、应用服务应用和所有相关资源。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [`az group create`](https://docs.azure.cn/zh-cn/cli/group?view=azure-cli-latest#az_group_create) | 创建用于存储所有资源的资源组。 |
| [`az appservice plan create`](https://docs.azure.cn/zh-cn/cli/appservice/plan?view=azure-cli-latest#az_appservice_plan_create) | 创建应用服务计划。 |
| [`az webapp create`](https://docs.azure.cn/zh-cn/cli/webapp?view=azure-cli-latest#az_webapp_create) | 创建 Azure Web 应用。 |
| [`az appservice plan update`](https://docs.azure.cn/zh-cn/cli/appservice/plan?view=azure-cli-latest#az_appservice_plan_update) | 更新应用服务计划的属性。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/zh-cn/cli/overview?view=azure-cli-lastest)。

可以在 [Azure 应用服务文档](../samples-cli.md)中找到其他应用服务 CLI 脚本示例。
