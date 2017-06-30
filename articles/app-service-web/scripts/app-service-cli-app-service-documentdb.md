---
title: "Azure CLI 脚本示例 - 将 Web 应用连接到 documentdb | Azure"
description: "Azure CLI 脚本示例 - 将 Web 应用连接到 documentdb"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: bbbdbc42-efb5-4b4f-8ba6-c03c9d16a7ea
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
ms.openlocfilehash: 98fcd8daa9cec4a29bc8eaa71256a50b1680cce8
ms.contentlocale: zh-cn
ms.lasthandoff: 05/19/2017

---

# <a name="connect-a-web-app-to-cosmos-db"></a>将 Web 应用连接到 documentdb

在此方案中，将了解如何创建 Azure documentdb 帐户和 Azure Web 应用。 然后，使用应用设置将 documentdb 链接到 Web 应用。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>示例脚本

```azurecli
#/bin/bash

# Variables
appName="webappwithcosmosdb$random"
storageName="webappwithcosmosdb$random"
location="ChinaNorth"

# Create a Resource Group 
az group create --name myResourceGroup --location $location

# Create an App Service Plan
az appservice plan create --name WebAppWithCosmosDBPlan --resource-group myResourceGroup --location $location

# Create a Web App
az appservice web create --name $appName --plan WebAppWithCosmosDBPlan --resource-group myResourceGroup 

# Create a documentdb
cosmosdb=$(az cosmosdb create --name $appName --resource-group myResourceGroup --query documentEndpoint --output tsv)
cosmosCreds=$(az cosmosdb list-keys --name $appName --resource-group myResourceGroup --query primaryMasterKey --output tsv)

# Assign the connection string to an App Setting in the Web App
az appservice web config appsettings update --settings "COSMOSDB_URL=$cosmosdb" "COSMOSDB_KEY=$cosmosCreds" --name $appName --resource-group myResourceGroup
```

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建资源组、Web 应用、documentdb 和所有相关资源。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 说明 |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | 创建用于存储所有资源的资源组。 |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#create) | 创建应用服务计划。 这与 Azure Web 应用的服务器场类似。 |
| [az appservice web create](https://docs.microsoft.com/cli/azure/webapp#create) | 创建应用服务计划中的 Azure Web 应用。 |
| [az cosmosdb create](https://docs.microsoft.com/cli/azure/cosmosdb#create) | 创建 documentdb 帐户。 这将是数据存储位置。 |
| [az cosmosdb list-keys](https://docs.microsoft.com/cli/azure/cosmosdb#list-keys) | 列出指定 documentdb 帐户的访问密钥。 |
| [az appservice web config appsettings update](https://docs.microsoft.com/cli/azure/webapp/config/appsettings#update) | 创建或更新 Azure Web 应用的应用设置。 应用设置将作为应用的环境变量公开。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.microsoft.com/cli/azure/overview)。

可以在 [Azure 应用服务文档](../app-service-cli-samples.md)中找到其他应用服务 CLI 脚本示例。

