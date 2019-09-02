---
title: include 文件
description: include 文件
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
origin.date: 02/02/2018
ms.date: 03/18/2019
ms.author: v-biyu
ms.custom: include file
ms.openlocfilehash: ed024945f0c573c2b5a41e7d4211903417b4fbe6
ms.sourcegitcommit: 0ccbf718e90bc4e374df83b1460585d3b17239ab
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2019
ms.locfileid: "57350494"
---
使用 [az webapp create](https://docs.azure.cn/zh-cn/cli/webapp?view=azure-cli-latest#az_webapp_create) 命令在 `myAppServicePlan` 应用服务计划中创建一个 Web 应用。 

在 Azure CLI 中，可以使用 [`az webapp create`](/cli/webapp?view=azure-cli-latest) 命令。在以下示例中，将 `<app_name>` 替换为全局唯一的应用名称（有效字符是 `a-z`、`0-9` 和 `-`）。 运行时设置为 `PHP|7.0`。 若要查看所有受支持的运行时，请运行 [az webapp list-runtimes](https://docs.azure.cn/zh-cn/cli/webapp?view=azure-cli-latest#az_webapp_list_runtimes)。 

```azurecli
# Bash
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --runtime "PHP|7.0" --deployment-local-git
# PowerShell
az --% webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --runtime "PHP|7.0" --deployment-local-git
```

创建 Web 应用后，Azure CLI 会显示类似于以下示例的输出：

```json
Local git is configured with url of 'https://<username>@<app_name>.scm.chinacloudsites.cn/<app_name>.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.chinacloudsites.cn",
  "deploymentLocalGitUrl": "https://<username>@<app_name>.scm.chinacloudsites.cn/<app_name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

现在你已经创建了一个新的空 Web 应用并启用了 Git 部署。

> [!NOTE]
> Git 远程的 URL 将显示在 `deploymentLocalGitUrl` 属性中，其格式为 `https://<username>@<app_name>.scm.chinacloudsites.cn/<app_name>.git`。 保存此 URL，因为后面需要它。
>
