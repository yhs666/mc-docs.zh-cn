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
ms.openlocfilehash: ce1bacac27cbd54b6f980bf07b18bf7122cd32d8
ms.sourcegitcommit: 4a09701b1cbc1d9ccee46d282e592aec26998bff
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75334901"
---
使用 [az webapp create](https://docs.azure.cn/cli/webapp?view=azure-cli-latest#az_webapp_create) 命令在 `myAppServicePlan` 应用服务计划中创建一个 Web 应用。 

在 Azure CLI 中，可以使用 [`az webapp create`](/cli/webapp?view=azure-cli-latest) 命令。在以下示例中，将 `<app-name>` 替换为全局唯一的应用名称（有效字符是 `a-z`、`0-9` 和 `-`）。 运行时设置为 `PHP|7.0`。 若要查看所有受支持的运行时，请运行 [az webapp list-runtimes](/cli/webapp?view=azure-cli-latest)。 

```azurecli
# Bash
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app-name> --runtime "PHP|7.0" --deployment-local-git
# PowerShell
az --% webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app-name> --runtime "PHP|7.0" --deployment-local-git
```

创建 Web 应用后，Azure CLI 会显示类似于以下示例的输出：

```json
Local git is configured with url of 'https://<username>@<app-name>.scm.chinacloudsites.cn/<app-name>.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app-name>.chinacloudsites.cn",
  "deploymentLocalGitUrl": "https://<username>@<app-name>.scm.chinacloudsites.cn/<app-name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

现在你已经创建了一个新的空 Web 应用并启用了 Git 部署。

> [!NOTE]
> Git 远程的 URL 将显示在 `deploymentLocalGitUrl` 属性中，其格式为 `https://<username>@<app-name>.scm.chinacloudsites.cn/<app-name>.git`。 保存此 URL，因为后面需要它。
>
