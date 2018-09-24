---
title: 使用 Azure CLI 创建第一个函数
description: 了解如何使用 Azure CLI 和 Azure Functions Core Tools 创建第一个支持无服务器执行的 Azure 函数。
services: functions
keywords: ''
author: ggailey777
ms.author: v-junlch
ms.assetid: 674a01a7-fd34-4775-8b69-893182742ae0
origin.date: 09/10/2018
ms.date: 09/21/2018
ms.topic: quickstart
ms.service: azure-functions
ms.custom: mvc
ms.devlang: azure-cli
manager: jeconnoc
ms.openlocfilehash: 755c9cba6b9feb2648a5e14dcae17e29ef3f482c
ms.sourcegitcommit: 54d9384656cee927000d77de5791c1d585d94a68
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/21/2018
ms.locfileid: "46524044"
---
# <a name="create-your-first-function-from-the-command-line"></a>从命令行创建第一个函数

本快速入门主题逐步讲解如何通过命令行或终端创建第一个函数。 使用 Azure CLI 创建函数应用（托管函数的[无服务器](https://azure.microsoft.com/overview/serverless-computing/)基础结构）。 函数代码项目是使用 [Azure Functions Core Tools](functions-run-local.md)（也可用于将函数应用项目部署到 Azure）从模板生成的。

可以使用 Mac、Windows 或 Linux 计算机执行以下步骤。

## <a name="prerequisites"></a>先决条件

运行此示例之前，必须做好以下准备：

+ 安装 [Azure Core Tools 版本 2.x](functions-run-local.md#v2)。

+ 安装 [Azure CLI](/cli/install-azure-cli)。 本文需要 Azure CLI 2.0 或更高版本。 运行 `az --version` 即可确定你拥有的版本。

+ 一个有效的 Azure 订阅。

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。

## <a name="create-the-local-function-app-project"></a>创建本地函数应用项目

从命令行运行以下命令，在当前本地目录的 `MyFunctionProj` 文件夹中创建函数应用项目。 同时在 `MyFunctionProj` 中创建一个 GitHub 存储库。

```bash
func init MyFunctionProj
```

出现提示时，请使用箭头键从以下语言选项中选择辅助角色运行时：

+ `dotnet`：创建 .NET 类库项目 (.csproj)。
+ `node`：创建 JavaScript 项目。

执行该命令时，会看到如下所示的输出：

```output
Writing .gitignore
Writing host.json
Writing local.settings.json
Initialized empty Git repository in C:/functions/MyFunctionProj/.git/
```

## <a name="create-a-function"></a>创建函数

以下命令导航到新项目，并创建名为 `MyHtpTrigger` 的 HTTP 触发的函数。

```bash
cd MyFunctionProj
func new --name MyHttpTrigger --template "HttpTrigger"
```

执行该命令时，会看到如下所示的输出（一个 JavaScript 函数）：

```output
Writing C:\functions\MyFunctionProj\MyHttpTrigger\index.js
Writing C:\functions\MyFunctionProj\MyHttpTrigger\sample.dat
Writing C:\functions\MyFunctionProj\MyHttpTrigger\function.json
```

## <a name="edit-the-function"></a>编辑函数

默认情况下，模板会创建一个在发出请求时需要函数密钥的函数。 若要在 Azure 中更轻松地测试该函数，需将该函数更新为允许匿名访问。 进行此项更改的方式取决于函数项目的语言。

### <a name="c"></a>C\#

打开 MyHttpTrigger.cs 代码文件（即新函数），将函数定义中的 **AuthorizationLevel** 特性更新为值 `anonymous`，然后保存更改。

```csharp
[FunctionName("MyHttpTrigger")]
        public static IActionResult Run([HttpTrigger(AuthorizationLevel.Anonymous, 
            "get", "post", Route = null)]HttpRequest req, ILogger log)
```

### <a name="javascript"></a>Javascript

在文本编辑器中打开新函数的 function.json 文件，将 **bindings.httpTrigger** 中的 **authLevel** 属性更新为 `anonymous`，然后保存更改。

```json
  "bindings": [
    {
      "authLevel": "anonymous",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
  ]
```

现在，无需提供函数密钥，即可在 Azure 中调用该函数。 在本地运行时永远不需要函数密钥。

## <a name="run-the-function-locally"></a>在本地运行函数

以下命令启动函数应用。 该应用将使用 Azure 中的相同 Azure Functions 运行时运行。

```bash
func host start --build
```

编译 C# 项目需要 `--build` 选项。 JavaScript 项目不需要此选项。

当 Functions 宿主启动时，会写入以下输出所示的内容（为方便阅读，内容已截断）：

```output

                  %%%%%%
                 %%%%%%
            @   %%%%%%    @
          @@   %%%%%%      @@
       @@@    %%%%%%%%%%%    @@@
     @@      %%%%%%%%%%        @@
       @@         %%%%       @@
         @@      %%%       @@
           @@    %%      @@
                %%
                %

...

Content root path: C:\functions\MyFunctionProj
Now listening on: http://0.0.0.0:7071
Application started. Press Ctrl+C to shut down.

...

Http Functions:

        HttpTrigger: http://localhost:7071/api/HttpTrigger

[8/27/2018 10:38:27 PM] Host started (29486ms)
[8/27/2018 10:38:27 PM] Job host started
```

从运行时输出中复制 `HTTPTrigger` 函数的 URL，并将其粘贴到浏览器的地址栏中。 将查询字符串 `?name=<yourname>` 追加到此 URL 并执行请求。 下面演示本地函数在浏览器中返回的对 GET 请求的响应：

![在浏览器本地进行测试](./media/functions-create-first-azure-function-azure-cli/functions-test-local-browser.png)

在本地运行函数后，可在 Azure 中创建函数应用和其他所需资源。

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-function-app"></a>创建函数应用

必须使用 Function App 托管函数的执行。 Function App 提供一个环境，以便在不使用服务器的情况下执行函数代码。 它可让你将函数分组为一个逻辑单元，以便更轻松地管理、部署和共享资源。 使用 [az functionapp create](/cli/functionapp#az-functionapp-create) 命令创建 Function App。 

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

<!-- Update_Description: wording update -->