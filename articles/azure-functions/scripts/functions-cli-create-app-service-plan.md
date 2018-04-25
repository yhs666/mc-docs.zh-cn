---
title: Azure CLI 脚本示例 - 在应用服务计划中创建 Function App | Microsoft Docs
description: Azure CLI 脚本示例 - 在应用服务计划中创建 Function App
services: functions
documentationcenter: functions
author: syntaxc4
manager: cfowler
editor: ''
tags: azure-service-management
ms.assetid: 0e221db6-ee2d-4e16-9bf6-a456cd05b6e7
ms.service: functions
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
origin.date: 10/22/2018
ms.date: 04/19/2018
ms.author: v-junlch
ms.custom: mvc
ms.openlocfilehash: 7db35091461b0b42c5419c209e6f0acb2dc7f2d1
ms.sourcegitcommit: f97c9253d16fac8be0266c9473c730ebd528e542
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/19/2018
---
# <a name="create-a-function-app-in-an-app-service-plan"></a>在应用服务计划中创建 Function App

此 Azure Functions 示例脚本将创建一个函数应用，作为函数的容器。 创建的函数应用使用专用应用服务计划，这意味着服务器资源将始终启用。

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。

如果选择在本地安装并使用 CLI，本文要求使用 Azure CLI 2.0 或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI 2.0](/cli/install-azure-cli)。 

## <a name="sample-script"></a>示例脚本

此脚本使用专用[应用服务计划](../functions-scale.md#app-service-plan)创建 Azure Function App。

```azurecli
#!/bin/bash

# Create a resource resourceGroupName
az group create `
  --name myResourceGroup `
  --location chinanorth

# Create an azure storage account
az storage account create `
  --name myappsvcpstore `
  --location chinanorth `
  --resource-group myResourceGroup `
  --sku Standard_LRS

# Create an App Service plan
az appservice plan create `
  --name myappserviceplan `
  --resource-group myResourceGroup `
  --location chinanorth

# Create a Function App
az functionapp create `
  --name myappsvcpfunc `
  --storage-account myappsvcpstore `
  --plan myappserviceplan `
  --resource-group myResourceGroup
```

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>脚本说明

表中的每条命令均链接到特定于命令的文档。 此脚本使用以下命令：

| 命令 | 注释 |
|---|---|
| [az group create](/cli/group#az_group_create) | 创建用于存储所有资源的资源组。 |
| [az storage account create](/cli/storage/account#az_storage_account_create) | 创建 Azure 存储帐户。 |
| [az appservice plan create](/cli/appservice/plan?view=azure-cli-latest) | 创建应用服务计划。 |
| [az functionapp create](https://docs.microsoft.com/cli/azure/functionapp#az_functionapp_delete) | 创建 Azure Function App。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](/cli/overview?view=azure-cli-latest)。

可以在 [Azure Functions 文档](../functions-cli-samples.md)中找到其他 Azure Functions CLI 脚本示例。

