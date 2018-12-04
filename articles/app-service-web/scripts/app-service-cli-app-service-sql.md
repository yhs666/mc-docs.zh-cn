---
title: Azure CLI 脚本示例 - 将 Web 应用连接到 SQL 数据库 | Azure
description: Azure CLI 脚本示例 - 将 Web 应用连接到 SQL 数据库
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: ''
tags: azure-service-management
ms.assetid: 7c2efdd0-f553-4038-a77a-e953021b3f77
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
origin.date: 06/19/2017
ms.date: 10/09/2017
ms.author: v-yiso
ms.custom: mvc
ms.openlocfilehash: 502ff1a379de2a00429a5e4897a6b394849cd5b5
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52649564"
---
# <a name="connect-a-web-app-to-a-sql-database"></a>将 Web 应用连接到 SQL 数据库

通过此方案可以了解如何创建 Azure SQL 数据库和 Azure Web 应用。 然后，使用应用设置将 SQL 数据库链接到 Web 应用。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

如果选择在本地安装并使用 CLI，本主题要求运行 Azure CLI 2.0 版或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli)。 

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

## <a name="sample-script"></a>示例脚本

```azurecli
#/bin/bash

# Variables
appName="webappwithSQL$random"
serverName="webappwithsql$random"
location="ChinaNorth"
startip="0.0.0.0"
endip="0.0.0.0"
username="<replace-with-username>"
sqlServerPassword="<replace-with-password>"

# Create a Resource Group 
az group create --name myResourceGroup --location $location

# Create an App Service Plan
az appservice plan create --name WebAppWithSQLPlan --resource-group myResourceGroup --location $location

# Create a Web App
az webapp create --name $appName --plan WebAppWithSQLPlan --resource-group myResourceGroup

# Create a SQL Server
az sql server create --name $serverName --resource-group myResourceGroup --location $location --admin-user $username --admin-password $sqlServerPassword

# Configure Firewall for Azure Access
az sql server firewall-rule create --resource-group myResourceGroup --server $serverName --name AllowYourIp --start-ip-address $startip --end-ip-address $endip

# Create Database on Server
az sql db create --resource-group myResourceGroup --server $serverName --name MySampleDatabase --service-objective S0

# Assign the connection string to an App Setting in the Web App
az webapp config appsettings set --settings "SQLSRV_CONNSTR=Server=tcp:$serverName.database.chinacloudapi.cn;Database=MySampleDatabase;User ID=$username@$serverName;Password=$sqlServerPassword;Trusted_Connection=False;Encrypt=True;" --name $appName --resource-group myResourceGroup

```

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建资源组、Web 应用、SQL 数据库和所有相关资源。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | 创建用于存储所有资源的资源组。 |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | 创建应用服务计划。 这与 Azure Web 应用的服务器场类似。 |
| [az webapp create](https://docs.microsoft.com/cli/azure/webapp#az_webapp_create) | 创建 Azure Web 应用。 |
| [az sql server create](https://docs.microsoft.com/cli/azure/sql/server#az_sql_server_create) | 创建 SQL 数据库服务器。  |
| [az sql db create](https://docs.microsoft.com/cli/azure/sql/db#az_sql_db_create) | 使用 SQL 数据库服务器创建新的数据库。 |
| [az webapp config appsettings set](https://docs.microsoft.com/cli/azure/webapp/config/appsettings#az_webapp_config_appsettings_set) | 创建或更新 Azure Web 应用的应用设置。 应用设置将作为应用的环境变量公开。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.microsoft.com/cli/azure/overview)。

可以在 [Azure 应用服务文档](../app-service-cli-samples.md)中找到其他应用服务 CLI 脚本示例。

<!--Update_Description: add a note about Azure CLI 2.0 version-->