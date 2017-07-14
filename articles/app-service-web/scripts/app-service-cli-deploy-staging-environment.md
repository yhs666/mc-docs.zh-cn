---
title: "Azure CLI 脚本示例 - 创建 Web 应用并将代码部署到过渡环境 | Azure"
description: "Azure CLI 脚本示例 - 创建 Web 应用并将代码部署到过渡环境"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 2b995dcd-e471-4355-9fda-00babcdb156e
ms.service: app-service-web
ms.workload: web
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
origin.date: 03/20/2017
ms.date: 04/24/2017
ms.author: v-dazen
ms.custom: mvc
ms.openlocfilehash: dfa4b52a9267f50169947c3e53ad71bf77c99ea8
ms.sourcegitcommit: 2c397ac599bdb39b257580a1b55a1ce67e19ae56
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# 创建 Web 应用并将代码部署到过渡环境
<a id="create-a-web-app-and-deploy-code-to-a-staging-environment" class="xliff"></a>

此示例脚本使用名为“过渡”的附加部署槽在应用服务中创建 Web 应用，然后将示例应用部署到“过渡”槽。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## 示例脚本
<a id="sample-script" class="xliff"></a>

```azurecli
#!/bin/bash

# Replace the following URL with a public GitHub repo URL
gitrepo=https://github.com/Azure-Samples/php-docs-hello-world
webappname=mywebapp$RANDOM

# Create a resource group.
az group create --location chinanorth --name myResourceGroup

# Create an App Service plan in STANDARD tier (minimum required by deployment slots).
az appservice plan create --name $webappname --resource-group myResourceGroup --sku S1

# Create a web app.
az webapp create --name $webappname --resource-group myResourceGroup \
--plan $webappname

#Create a deployment slot with the name "staging".
az webapp deployment slot create --name $webappname --resource-group myResourceGroup \
--slot staging

# Deploy sample code to "staging" slot from GitHub.
az webapp deployment source config --name $webappname --resource-group myResourceGroup \
--slot staging --repo-url $gitrepo --branch master --manual-integration

# Browse to the deployed web app on staging. Deployment may be in progress, so rerun this if necessary.
az webapp browse --name $webappname --resource-group myResourceGroup --slot staging

# Swap the verified/warmed up staging slot into production.
az webapp deployment slot swap --name $webappname --resource-group myResourceGroup \
--slot staging

# Browse to the production slot. 
az webapp browse --name $webappname --resource-group myResourceGroup

```

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## 脚本说明
<a id="script-explanation" class="xliff"></a>

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 说明 |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | 创建用于存储所有资源的资源组。 |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#create) | 创建应用服务计划。 |
| [az appservice web create](https://docs.microsoft.com/cli/azure/webapp#delete) | 创建 Azure Web 应用。 |
| [az appservice web deployment slot create](https://docs.microsoft.com/cli/azure/webapp/deployment/slot#create) | 创建部署槽。 |
| [az appservice web source-control config](https://docs.microsoft.com/cli/azure/webapp/deployment/source#config) | 将 Azure Web 应用与 Git 或 Mercurial 存储库相关联。 |
| [az appservice web browse](https://docs.microsoft.com/cli/azure/webapp#browse) | 在浏览器中打开 Azure Web 应用。 |
| [az appservice web deployment slot swap](https://docs.microsoft.com/cli/azure/webapp/deployment/slot#swap) | 将指定的部署槽交换到生产环境。 |

## 后续步骤
<a id="next-steps" class="xliff"></a>

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.microsoft.com/cli/azure/overview)。

可以在 [Azure 应用服务文档](../app-service-cli-samples.md)中找到其他应用服务 CLI 脚本示例。
