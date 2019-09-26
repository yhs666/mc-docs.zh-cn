---
title: include 文件
description: include 文件
services: app-service
author: msangapu
ms.service: app-service
ms.topic: include
origin.date: 09/18/2018
ms.date: 09/18/2018
ms.author: v-tawe
ms.custom: include file
ms.openlocfilehash: c3bcdbec02a3c1619a74718858887438727219c5
ms.sourcegitcommit: 6a62dd239c60596006a74ab2333c50c4db5b62be
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/20/2019
ms.locfileid: "71155820"
---
在 Azure CLI 中，使用 [`az appservice plan create`](/cli/appservice/plan?view=azure-cli-latest#az-appservice-plan-create) 命令创建一个应用服务计划。

以下示例在免费  定价层中创建名为 `myAppServicePlan` 的应用服务计划：

```azurecli
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku FREE
```

创建应用服务计划后，Azure CLI 将显示类似于以下示例的信息：

```json
{ 
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "china north",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "app",
  "location": "china north",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  < JSON data removed for brevity. >
  "targetWorkerSizeId": 0,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
} 
```
