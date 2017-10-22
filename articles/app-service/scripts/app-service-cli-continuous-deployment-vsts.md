---
title: "Azure CLI 脚本示例 - 从 Visual Studio Team Services 使用连续部署创建 Web 应用 | Microsoft 文档"
description: "Azure CLI 脚本示例 - 从 Visual Studio Team Services 使用连续部署创建 Web 应用"
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 389d3bd3-cd8e-4715-a3a1-031ec061d385
ms.service: app-service-web
ms.workload: web
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
origin.date: 06/19/2017
ms.date: 10/30/2017
ms.author: v-yiso
ms.custom: mvc
ms.openlocfilehash: 12d314806cc4fdcdd5123483e3d111cf02730c3c
ms.sourcegitcommit: 6ef36b2aa8da8a7f249b31fb15a0fb4cc49b2a1b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/20/2017
---
# <a name="create-a-web-app-with-continuous-deployment-from-visual-studio-team-services"></a>从 Visual Studio Team Services 使用连续部署创建 Web 应用

此示例脚本使用其相关资源在应用服务中创建 Web 应用，并在 Visual Studio Team Services 存储库中设置连续部署。 此示例需要：

* 包含应用程序代码且你对其拥有管理权限的 Visual Studio Team Services 存储库。
* Visual Studio Team Services 帐户的[个人访问令牌 (PAT)](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate)。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]



如果选择在本地安装并使用 CLI，本主题要求运行 Azure CLI 2.0 版或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI 2.0]( /cli/azure/install-azure-cli)。 

## <a name="sample-script"></a>示例脚本

```azurecli
#!/bin/bash

gitrepo=<Replace with your Visual Studio Team Services repo URL>
token=<Replace with a Visual Studio Team Services personal access token>
webappname=mywebapp$RANDOM

# Create a resource group.
az group create --location chinanorth --name myResourceGroup

# Create an App Service plan in `FREE` tier.
az appservice plan create --name $webappname --resource-group myResourceGroup --sku FREE

# Create a web app.
az webapp create --name $webappname --resource-group myResourceGroup --plan $webappname

# Configure continuous deployment from Visual Studio Team Services. 
# --git-token parameter is required only once per Azure account (Azure remembers token).
az webapp deployment source config --name $webappname --resource-group myResourceGroup \
--repo-url $gitrepo --branch master --git-token $token

# Copy the result of the following command into a browser to see the web app.
echo http://$webappname.chinacloudsites.cn
```

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 说明 |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | 创建用于存储所有资源的资源组。 |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | 创建应用服务计划。 |
| [az webapp create](https://docs.microsoft.com/cli/azure/webapp#az_webapp_create) | 创建 Azure Web 应用。 |
| [az webapp deployment source config](https://docs.microsoft.com/cli/azure/webapp/deployment/source#az_webapp_deployment_source_config) | 将 Azure Web 应用与 Git 或 Mercurial 存储库相关联。 |
| [az webapp browse](https://docs.microsoft.com/cli/azure/webapp#az_webapp_browse) | 在浏览器中打开 Azure Web 应用。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.microsoft.com/cli/azure/overview)。

可以在 [Azure 应用服务文档](../app-service-cli-samples.md)中找到其他应用服务 CLI 脚本示例。
