---
title: 使用 Azure CLI 创建第一个函数
description: 了解如何使用 Azure CLI 和 Azure Functions Core Tools 创建第一个可无服务器执行的 Azure Function。
services: functions
keywords: ''
author: ggailey777
ms.author: v-junlch
ms.assetid: 674a01a7-fd34-4775-8b69-893182742ae0
origin.date: 11/13/2018
ms.date: 12/26/2018
ms.topic: quickstart
ms.service: azure-functions
ms.custom: mvc
ms.devlang: azure-cli
manager: jeconnoc
ms.openlocfilehash: 860b340b57fb85ee881817ad9378dfceeb85ff5e
ms.sourcegitcommit: d15400cf780fd494d491b2fe1c56e312d3a95969
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/28/2018
ms.locfileid: "53806529"
---
# <a name="create-your-first-function-from-the-command-line"></a>从命令行创建第一个函数

本快速入门主题逐步讲解如何使用命令行或终端创建第一个函数。 使用 Azure CLI 创建函数应用（托管函数的[无服务器](https://azure.microsoft.com/solutions/serverless/)基础结构）。 此函数代码项目通过 [Azure Functions Core Tools](functions-run-local.md) 从模板生成，该工具也用于将函数应用项目部署到 Azure。

可以使用 Mac、Windows 或 Linux 计算机执行以下步骤。

## <a name="prerequisites"></a>先决条件

运行此示例之前，必须做好以下准备：

+ 安装 [Azure Core Tools 2.x 版](functions-run-local.md#v2)。

+ 安装 [Azure CLI](/cli/install-azure-cli)。 本文需要 Azure CLI 2.0 或更高版本。 运行 `az --version` 即可确定你拥有的版本。

+ 一个有效的 Azure 订阅。

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。

## <a name="create-the-local-function-app-project"></a>创建本地函数应用项目

从命令行运行以下命令，在当前本地目录的 `MyFunctionProj` 文件夹中创建函数应用项目。 也会在 `MyFunctionProj` 中创建一个 GitHub 存储库。

```bash
func init MyFunctionProj
```

当系统提示时，请从下面的语言选项中选择一个辅助角色运行时：

+ `dotnet`：创建一个 .NET 类库项目 (.csproj)。
+ `node`：创建 JavaScript 项目。

执行命令后，看到的内容如以下输出所示：

```output
Writing .gitignore
Writing host.json
Writing local.settings.json
Initialized empty Git repository in C:/functions/MyFunctionProj/.git/
```

使用以下命令导航到新的 `MyFunctionProj` 项目文件夹。

```bash
cd MyFunctionProj
```

[!INCLUDE [functions-create-function-core-tools](../../includes/functions-create-function-core-tools.md)]

[!INCLUDE [functions-update-function-code](../../includes/functions-update-function-code.md)]

[!INCLUDE [functions-run-function-test-local](../../includes/functions-run-function-test-local.md)]

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-function-app"></a>创建函数应用

必须使用 Function App 托管函数的执行。 Function App 提供一个环境，以便在不使用服务器的情况下执行函数代码。 它可让你将函数分组为一个逻辑单元，以便更轻松地管理、部署和共享资源。 使用 [az functionapp create](/cli/functionapp#az-functionapp-create) 命令创建 Function App。 

在以下命令中，请将 `<app_name>` 占位符替换成唯一函数应用名称，将 `<storage_name>` 替换为存储帐户名。 `<app_name>` 将用作 Function App 的默认 DNS 域，因此，该名称需要在 Azure 中的所有应用之间保持唯一。 你还应该通过 `dotnet` (C#) 或 `node` (JavaScript) 为函数应用设置 `<language>` 运行时。

```azurecli
az functionapp create --resource-group myResourceGroup --plan <App Service plan> `
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

[!INCLUDE [functions-publish-project](../../includes/functions-publish-project.md)]

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

[!INCLUDE [functions-quickstart-next-steps-cli](../../includes/functions-quickstart-next-steps-cli.md)]

<!-- Update_Description: wording update -->