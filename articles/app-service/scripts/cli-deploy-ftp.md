---
title: Azure CLI 脚本示例 - 创建应用并使用 FTP 部署文件 | Azure Docs
description: Azure CLI 脚本示例 - 创建应用服务应用并使用 FTP 部署文件
services: app-service\web
documentationcenter: ''
author: msangapu
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.service: app-service-web
ms.workload: web
ms.devlang: azurecli
ms.tgt_pltfrm: sample
ms.topic: sample
origin.date: 12/12/2017
ms.date: 01/21/2019
ms.author: v-biyu
ms.custom: seodec18
ms.openlocfilehash: 1810cf51c909fade32b9a2a10b1f03440c1b2c5a
ms.sourcegitcommit: 90d5f59427ffa599e8ec005ef06e634e5e843d1e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2019
ms.locfileid: "54083833"
---
# <a name="create-an-app-service-app-and-deploy-files-with-ftp-using-azure-cli"></a>使用 Azure CLI 创建应用服务应用并通过 FTP 部署文件

本示例脚本使用其相关资源，在应用服务中创建应用，然后使用 FTP 部署静态 HTML 页。 脚本使用 [cURL](https://en.wikipedia.org/wiki/CURL) 作为 FTP 上传的示例。 可以使用任意 FTP 工具来上传文件。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


如果选择在本地安装并使用 CLI，则需使用 Azure CLI 2.0 或更高版本。 若要查找版本，请运行 `az --version`。 如果需要进行安装或升级，请参阅[安装 Azure CLI 2.0](https://docs.azure.cn/zh-cn/cli/install-azure-cli?view=azure-cli-lastest)。

## <a name="sample-script"></a>示例脚本

```azurecli
#!/bin/bash

# Replace the following URL with a public GitHub repo URL
warurl=https://raw.githubusercontent.com/Azure-Samples/html-docs-hello-world/master/index.html
webappname=mywebapp$RANDOM

# Download sample static HTML page
curl $warurl --output index.html

# Create a resource group.
az group create --location chinanorth --name myResourceGroup

# Create an App Service plan in `FREE` tier.
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku FREE

# Create a web app.
az webapp create --name $webappname --resource-group myResourceGroup --plan myAppServicePlan

# Get FTP publishing profile and query for publish URL and credentials
creds=($(az webapp deployment list-publishing-profiles --name $webappname --resource-group myResourceGroup \
--query "[?contains(publishMethod, 'FTP')].[publishUrl,userName,userPWD]" --output tsv))

# Use cURL to perform FTP upload. You can use any FTP tool to do this instead. 
curl -T index.html -u ${creds[1]}:${creds[2]} ${creds[0]}/

# Copy the result of the following command into a browser to see the static HTML site.
echo http://$webappname.chinacloudsites.cn
```

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>脚本说明 

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [`az group create`](https://docs.azure.cn/zh-cn/cli/group?view=azure-cli-latest#az_group_create) | 创建用于存储所有资源的资源组。 |
| [`az appservice plan create`](https://docs.azure.cn/zh-cn/cli/appservice/plan?view=azure-cli-latest#az_appservice_plan_create) | 创建应用服务计划。 |
| [`az webapp create`](https://docs.azure.cn/zh-cn/cli/webapp?view=azure-cli-latest#az_webapp_create) | 创建 Azure Web 应用。 |
| [`az webapp deployment list-publishing-profiles`](https://docs.azure.cn/zh-cn/cli/webapp/deployment?view=azure-cli-latest#az_webapp_deployment_list_publishing_profiles) | 获取可用的 Web 应用部署配置文件的详细信息。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/zh-cn/cli/overview?view=azure-cli-lastest)。

可以在 [Azure 应用服务文档](../samples-cli.md)中找到其他应用服务 CLI 脚本示例。
