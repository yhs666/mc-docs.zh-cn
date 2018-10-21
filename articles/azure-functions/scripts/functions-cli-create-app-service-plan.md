---
title: Azure CLI 脚本示例 - 在应用服务计划中创建 Function App | Microsoft Docs
description: Azure CLI 脚本示例 - 在应用服务计划中创建 Function App
services: functions
documentationcenter: functions
author: ggailey777
manager: jeconnoc
ms.assetid: 0e221db6-ee2d-4e16-9bf6-a456cd05b6e7
ms.service: azure-functions
ms.devlang: azurecli
ms.topic: sample
origin.date: 07/03/2018
ms.date: 10/18/2018
ms.author: v-junlch
ms.custom: mvc
ms.openlocfilehash: ef4222dec224b6bd9ef91b03edad20913a18c470
ms.sourcegitcommit: 2d33477aeb0f2610c23e01eb38272a060142c85d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/19/2018
ms.locfileid: "49453666"
---
# <a name="create-a-function-app-in-an-app-service-plan"></a>在应用服务计划中创建 Function App

此 Azure Functions 示例脚本将创建一个函数应用，作为函数的容器。 创建的函数应用使用专用应用服务计划，这意味着服务器资源将始终启用。

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。

如果选择在本地安装并使用 CLI，本文要求使用 Azure CLI 2.0 或更高版本。 运行 `az --version` 即可查找版本。 如需进行安装或升级，请参阅[安装 Azure CLI](/cli/install-azure-cli)。 

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
| [az group create](/cli/group#az-group-create) | 创建用于存储所有资源的资源组。 |
| [az storage account create](/cli/storage/account#az-storage-account-create) | 创建 Azure 存储帐户。 |
| [az appservice plan create](/cli/appservice/plan#az-appservice-plan-create) | 创建应用服务计划。 |
| [az functionapp create](/cli/functionapp#az-functionapp-create) | 在应用服务计划中创建函数应用。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](/cli)。

可以在 [Azure Functions 文档](../functions-cli-samples.md)中找到其他 Azure Functions CLI 脚本示例。


<!-- Update_Description: wording update -->