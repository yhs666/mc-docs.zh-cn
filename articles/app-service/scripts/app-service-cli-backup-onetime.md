---
title: "Azure CLI 脚本示例 - 备份 Web 应用 | Microsoft Docs"
description: "Azure CLI 脚本示例 - 备份 Web 应用"
services: app-service\web
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
tags: azure-service-management
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
origin.date: 12/07/2017
ms.date: 01/02/2018
ms.author: v-yiso
ms.custom: mvc
ms.openlocfilehash: 0cf6ae7726051f564adffa0468f0b982741e46f0
ms.sourcegitcommit: 51f9fe7a93207e6b9d61e09b7abf56a7774ee856
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2017
---
# <a name="back-up-a-web-app"></a>备份 Web 应用

此示例脚本使用其相关资源，在应用服务中创建 Web 应用，然后为其创建一次性备份。 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]



如果选择在本地安装并使用 CLI，则需要使用 Azure CLI 2.0 版或更高版本。 若要查找版本，请运行 `az --version`。 如果需要进行安装或升级，请参阅[安装 Azure CLI 2.0](https://docs.azure.cn/zh-cn/cli/install-azure-cli?view=azure-cli-lastest)。

## <a name="sample-script"></a>示例脚本

```azurecli
#!/bin/bash

groupname="myResourceGroup"
planname="myAppServicePlan"
webappname=mywebapp$RANDOM
storagename=mywebappstorage$RANDOM
location="ChinaEast"
container="appbackup"
backupname="backup1"
expirydate=$(date -I -d "$(date) + 1 month")

# Create a Resource Group 
az group create --name $groupname --location $location

# Create a Storage Account
az storage account create --name $storagename --resource-group $groupname --location $location \
--sku Standard_LRS

# Create a storage container
az storage container create --account-name $storagename --name $container

# Generates an SAS token for the storage container, valid for one month.
# NOTE: You can use the same SAS token to make backups in App Service until --expiry
sastoken=$(az storage container generate-sas --account-name $storagename --name $container \
--expiry $expirydate --permissions rwdl --output tsv)

# Construct the SAS URL for the container
sasurl=https://$storagename.blob.core.chinacloudapi.cn/$container?$sastoken

# Create an App Service plan in Standard tier. Standard tier allows one backup per day.
az appservice plan create --name $planname --resource-group $groupname --location $location \
--sku S1

# Create a web app
az webapp create --name $webappname --plan $planname --resource-group $groupname

# Create a one-time backup
az webapp config backup create --resource-group $groupname --webapp-name $webappname \
--backup-name $backupname --container-url $sasurl

# List statuses of all backups that are complete or currently executing.
az webapp config backup list --resource-group $groupname --webapp-name $webappname
```

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [`az group create`](https://docs.azure.cn/zh-cn/cli/group?view=azure-cli-latest#az_group_create) | 创建用于存储所有资源的资源组。 |
| [`az storage account create`](https://docs.azure.cn/zh-cn/cli/storage/account?view=azure-cli-latest#az_storage_account_create) | 创建存储帐户。 |
| [`az storage container create`](https://docs.azure.cn/zh-cn/cli/storage/container?view=azure-cli-latest#az_storage_container_create) | 创建 Azure 存储容器。 |
| [`az storage container generate-sas`](https://docs.azure.cn/zh-cn/cli/storage/container?view=azure-cli-latest#az_storage_container_generate_sas) | 为 Azure 存储容器生成 SAS 令牌。  |
| [`az appservice plan create`](https://docs.azure.cn/zh-cn/cli/appservice/plan?view=azure-cli-latest#az_appservice_plan_create) | 创建应用服务计划。 |
| [`az webapp create`](https://docs.azure.cn/zh-cn/cli/webapp?view=azure-cli-latest#az_webapp_create) | 创建 Azure Web 应用。 |
| [`az webapp config backup create`](https://docs.azure.cn/zh-cn/cli/webapp/config/backup?view=azure-cli-latest#az_webapp_config_backup_create) | 创建 Web 应用的备份。 |
| [`az webapp config backup list`](https://docs.azure.cn/zh-cn/cli/webapp/config/backup?view=azure-cli-latest#az_webapp_config_backup_list) | 获取 Web 应用的备份列表。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/zh-cn/cli/overview?view=azure-cli-lastest)。

可以在 [Azure 应用服务文档](../app-service-cli-samples.md)中找到其他应用服务 CLI 脚本示例。
