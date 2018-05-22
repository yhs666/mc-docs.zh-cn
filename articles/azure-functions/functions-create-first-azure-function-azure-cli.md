---
title: 通过 Azure CLI 创建第一个函数 | Microsoft Docs
description: 了解如何使用 Azure CLI 创建第一个可无服务器执行的 Azure Function。
services: functions
keywords: ''
author: ggailey777
ms.author: v-junlch
ms.assetid: 674a01a7-fd34-4775-8b69-893182742ae0
origin.date: 01/24/2018
ms.date: 04/10/2018
ms.topic: quickstart
ms.service: functions
ms.custom: mvc
ms.devlang: azure-cli
manager: cfowler
ms.openlocfilehash: 17ea9bbb19587882d68ab8b02d7c6dfe9a5d5dc3
ms.sourcegitcommit: 6f08b9a457d8e23cf3141b7b80423df6347b6a88
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2018
---
# <a name="create-your-first-function-using-the-azure-cli"></a>使用 Azure CLI 创建第一个函数

本快速入门主题逐步讲解如何使用 Azure Functions 创建第一个函数。 使用 Azure CLI 创建函数应用（托管函数的[无服务器](https://azure.microsoft.com/overview/serverless-computing/)基础结构）。 函数代码本身将从 GitHub 示例存储库部署。    

可以使用 Mac、Windows 或 Linux 计算机执行以下步骤。 

## <a name="prerequisites"></a>先决条件 

运行此示例之前，必须做好以下准备：

+ 一个有效的 [GitHub](https://github.com) 帐户。 
+ 一个有效的 Azure 订阅。

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。

如果选择在本地安装并使用 CLI，本主题要求使用 Azure CLI 2.0 版或更高版本。 运行 `az --version` 即可确定你拥有的版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI 2.0](/cli/install-azure-cli)。 


[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-function-app"></a>创建函数应用

必须使用 Function App 托管函数的执行。 Function App 提供一个环境，以便在不使用服务器的情况下执行函数代码。 它可让你将函数分组为一个逻辑单元，以便更轻松地管理、部署和共享资源。 使用 [az functionapp create](https://docs.microsoft.com/en-us/cli/azure/functionapp?view=azure-cli-latest#az_functionapp_create) 命令创建 Function App。 

在以下命令中，请将 `<app_name>` 占位符替换成唯一函数应用名称，将 `<storage_name>` 替换为存储帐户名。 `<app_name>` 将用作 Function App 的默认 DNS 域，因此，该名称需要在 Azure 中的所有应用之间保持唯一。 _deployment-source-url_ 参数是 GitHub 中包含“Hello World”HTTP 触发函数的示例存储库。

```azurecli
az functionapp create --deployment-source-url https://github.com/Azure-Samples/functions-quickstart  `
--resource-group myResourceGroup --plan <App Service plan> chinanorth `
--name <app_name> --storage-account  <storage_name>  
```
在此计划中，将根据函数需要动态添加资源，你只在函数运行时付费。 有关详细信息，请参阅[选择适当的托管计划](functions-scale.md)。 

创建 Function App 后，Azure CLI 会显示类似于以下示例的信息：

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "containerSize": 1536,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "quickstart.chinacloudsites.cn",
  "enabled": true,
  "enabledHostNames": [
    "quickstart.chinacloudsites.cn",
    "quickstart.scm.chinacloudsites.cn"
  ],
   ....
    // Remaining output has been truncated for readability.
}
```

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

[!INCLUDE [functions-quickstart-next-steps-cli](../../includes/functions-quickstart-next-steps-cli.md)]

