---
title: "Azure CLI 脚本示例 - 从本地 Git 存储库创建 Web 应用并部署代码 | Azure"
description: "Azure CLI 脚本示例 - 从本地 Git 存储库创建 Web 应用并部署代码"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 048f98aa-f708-44cb-9b9e-953f67dc6da8
ms.service: app-service-web
ms.workload: web
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
origin.date: 03/20/2017
ms.date: 04/24/2017
ms.author: v-dazen
ms.custom: mvc
ms.openlocfilehash: be4740378cccfaff6b3a9eb7e76994e9c1a03bd6
ms.sourcegitcommit: 2c397ac599bdb39b257580a1b55a1ce67e19ae56
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# 从本地 Git 存储库创建 Web 应用并部署代码
<a id="create-a-web-app-and-deploy-code-from-a-local-git-repository" class="xliff"></a>

此示例脚本使用其相关资源，在应用服务中创建 Web 应用，然后在本地 Git 存储库中部署 Web 应用代码。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## 示例脚本
<a id="sample-script" class="xliff"></a>

```azurecli
#!/bin/bash

gitdirectory=<Replace with path to local Git repo>
username=<Replace with desired deployment username>
password=<Replace with desired deployment password>
webappname=mywebapp$RANDOM

# Create a resource group.
az group create --location chinanorth --name myResourceGroup

# Create an App Service plan in FREE tier.
az appservice plan create --name $webappname --resource-group myResourceGroup --sku FREE

# Create a web app.
az webapp create --name $webappname --resource-group myResourceGroup --plan $webappname

# Set the account-level deployment credentials
az webapp deployment user set --user-name $username --password $password

# Configure local Git and get deployment URL
url=$(az webapp deployment source config-local-git --name $webappname \
--resource-group myResourceGroup --query url --output tsv)

# Add the Azure remote to your local Git respository and push your code
cd $gitdirectory
git remote add azure $url
git push azure master

# When prompted for password, use the value of $password that you specified

# Browse to the deployed web app.
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
| [az appservice web deployment user set](https://docs.microsoft.com/cli/azure/webapp/deployment/user#set) | 为应用服务设置帐户级别部署凭据。 |
| [az appservice web source-control config-local-git](https://docs.microsoft.com/cli/azure/webapp/deployment/source#config-local-git) | 为本地 Git 存储库创建源控件配置。 |
| [az appservice web browse](https://docs.microsoft.com/cli/azure/webapp#browse) | 在浏览器中打开 Azure Web 应用。 |

## 后续步骤
<a id="next-steps" class="xliff"></a>

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.microsoft.com/cli/azure/overview)。

可以在 [Azure 应用服务文档](../app-service-cli-samples.md)中找到其他应用服务 CLI 脚本示例。
