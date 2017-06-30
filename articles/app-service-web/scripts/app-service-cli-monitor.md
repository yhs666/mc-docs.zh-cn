---
title: "Azure CLI 脚本示例 - 使用 Web 服务器日志监视 Web 应用 | Azure"
description: "Azure CLI 脚本示例 - 使用 Web 服务器日志监视 Web 应用"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 0887656f-611c-4627-8247-b5cded7cef60
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
origin.date: 03/20/2017
ms.date: 04/24/2017
ms.author: v-dazen
ms.translationtype: Human Translation
ms.sourcegitcommit: 4a18b6116e37e365e2d4c4e2d144d7588310292e
ms.openlocfilehash: 2c2ca34b195295c076b2d9c99b6d27f8185a6142
ms.contentlocale: zh-cn
ms.lasthandoff: 05/19/2017

---

# <a name="monitor-a-web-app-with-web-server-logs"></a>使用 Web 服务器日志监视 Web 应用

在此方案中，将创建资源组、应用服务计划、Web 应用，并配置 Web 应用以启用 Web 服务器日志。 然后，将下载日志文件以供查看。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>示例脚本

```azurecli
#/bin/bash

# Variables
appName="AppServiceMonitor$random"
location="ChinaNorth"

# Create a Resource Group
az group create --name myResourceGroup --location $location

# Create an App Service Plan
az appservice plan create --name AppServiceMonitorPlan --resource-group myResourceGroup --location $location

# Create a Web App and save the URL
url=$(az appservice web create --name $appName --plan AppServiceMonitorPlan --resource-group myResourceGroup --query defaultHostName | sed -e 's/^"//' -e 's/"$//')

# Enable all logging options for the Web App
az appservice web log config --name $appName --resource-group myResourceGroup --application-logging true --detailed-error-messages true --failed-request-tracing true --web-server-logging filesystem

# Create a Web Server Log
curl -s -L $url/404

# Download the log files for review
az appservice web log download --name $appName --resource-group myResourceGroup
```

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建资源组、Web 应用和所有相关资源。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 说明 |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | 创建用于存储所有资源的资源组。 |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#create) | 创建应用服务计划。 这与 Azure Web 应用的服务器场类似。 |
| [az appservice web create](https://docs.microsoft.com/cli/azure/webapp#create) | 创建应用服务计划中的 Azure Web 应用。 |
| [az appservice web log config](https://docs.microsoft.com/cli/azure/webapp/log#config) | 配置 Azure Web 应用将持久保留的日志。 |
| [az appservice web log download](https://docs.microsoft.com/cli/azure/webapp/log#download) | 将 Azure Web 应用的日志下载到本地计算机。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.microsoft.com/cli/azure/overview)。

可以在 [Azure 应用服务文档](../app-service-cli-samples.md)中找到其他应用服务 CLI 脚本示例。

