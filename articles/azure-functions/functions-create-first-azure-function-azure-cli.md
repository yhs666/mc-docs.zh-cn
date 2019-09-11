---
title: 使用 Azure CLI 创建第一个函数
description: 了解如何使用 Azure CLI 和 Azure Functions Core Tools 创建第一个可无服务器执行的 Azure Function。
services: functions
keywords: ''
author: ggailey777
ms.author: v-junlch
ms.assetid: 674a01a7-fd34-4775-8b69-893182742ae0
origin.date: 11/13/2018
ms.date: 09/06/2019
ms.topic: quickstart
ms.service: azure-functions
ms.custom: mvc
ms.devlang: azure-cli
manager: jeconnoc
ms.openlocfilehash: b8e8abccddaec3812cbb8fe48d493e0f4603a821
ms.sourcegitcommit: 4f1047b6848ca5dd96266150af74633b2e9c77a3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/09/2019
ms.locfileid: "70805752"
---
# <a name="create-your-first-function-from-the-command-line"></a>从命令行创建第一个函数

本快速入门主题逐步讲解如何使用命令行或终端创建第一个函数。 使用 Azure CLI 创建函数应用（托管函数的[无服务器](https://azure.microsoft.com/solutions/serverless/)基础结构）。 此函数代码项目通过 [Azure Functions Core Tools](functions-run-local.md) 从模板生成，该工具也用于将函数应用项目部署到 Azure。

可以使用 Mac、Windows 或 Linux 计算机执行以下步骤。

## <a name="prerequisites"></a>先决条件

运行此示例之前，必须做好以下准备：

+ 安装 [Azure Functions Core Tools](./functions-run-local.md#v2) 版本 2.6.666 或更高版本。

+ 安装 [Azure CLI](/cli/install-azure-cli)。 本文需要 Azure CLI 2.0 或更高版本。 运行 `az --version` 即可确定你拥有的版本。

+ 一个有效的 Azure 订阅。

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。

[!INCLUDE [functions-create-function-app-cli](../../includes/functions-create-function-app-cli.md)]

## <a name="enable-extension-bundles"></a>启用扩展捆绑包

[!INCLUDE [functions-extension-bundles](../../includes/functions-extension-bundles.md)]

[!INCLUDE [functions-create-function-core-tools](../../includes/functions-create-function-core-tools.md)]

[!INCLUDE [functions-run-function-test-local](../../includes/functions-run-function-test-local.md)]

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-function-app"></a>创建函数应用

必须使用 Function App 托管函数的执行。 Function App 提供一个环境，以便在不使用服务器的情况下执行函数代码。 它可让你将函数分组为一个逻辑单元，以便更轻松地管理、部署和共享资源。 使用 [az functionapp create](/cli/functionapp#az-functionapp-create) 命令创建 Function App。 

在以下命令中，请将 `<APP_NAME>` 占位符替换成唯一函数应用名称，将 `<STORAGE_NAME>` 替换为存储帐户名。 `<APP_NAME>` 将用作 Function App 的默认 DNS 域，因此，该名称需要在 Azure 中的所有应用之间保持唯一。 你还应该通过 `dotnet` (C#) 或 `node` (JavaScript) 为函数应用设置 `<language>` 运行时。

```azurecli
az functionapp create --resource-group myResourceGroup --consumption-plan-location chinanorth `
--name <APP_NAME> --storage-account  <STORAGE_NAME> --runtime <language>
```

设置 _consumption-plan-location_ 参数意味着函数应用将在消耗托管计划中托管。 在此无服务器计划中，将根据函数需要动态添加资源，你只在函数运行时付费。 有关详细信息，请参阅[选择适当的托管计划](functions-scale.md)。

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

[!INCLUDE [functions-publish-project](../../includes/functions-publish-project.md)]

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

[!INCLUDE [functions-quickstart-next-steps-cli](../../includes/functions-quickstart-next-steps-cli.md)]

<!-- Update_Description: wording update -->