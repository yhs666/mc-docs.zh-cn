---
title: include 文件
description: include 文件
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
origin.date: 02/02/2018
ms.date: 02/02/2018
ms.author: v-tawe
ms.custom: include file
ms.openlocfilehash: 9e9a8fe55193bd059f9e24caab57a829c6c3cd96
ms.sourcegitcommit: 0529a2aa102e058636d726b4a4f25208e1e60597
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/17/2019
ms.locfileid: "71059584"
---
在 `myAppServicePlan` 应用服务计划中创建一个 Web 应用。 

在 Azure CLI 中，可以使用 [`az webapp create`](/cli/webapp?view=azure-cli-latest#az-webapp-create) 命令。 在以下示例中，将 `<app-name>` 替换为全局唯一的应用名称（有效字符是 `a-z`、`0-9` 和 `-`）。 

```azurecli
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --deployment-local-git
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

> [!NOTE]
> Git 远程的 URL 将显示在 `deploymentLocalGitUrl` 属性中，其格式为 `https://<username>@<app_name>.scm.chinacloudsites.cn/<app-name>.git`。 保存此 URL，因为后面需要它。
>
