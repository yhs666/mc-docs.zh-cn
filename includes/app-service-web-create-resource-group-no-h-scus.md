---
title: include 文件
description: include 文件
services: app-service
author: msangapu
ms.service: app-service
ms.topic: include
ms.date: 09/18/2018
ms.author: msangapu
ms.custom: include file
ms.openlocfilehash: 262e07dc4a777b2682d0b6dd87a05cfc5a36dfc5
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52676075"
---
[!INCLUDE [resource group intro text](resource-group.md)]

在 Cloud Shell 中，使用 [`az group create`](/cli/azure/group?view=azure-cli-latest#az_group_create) 命令创建资源组。 下面的示例命令在“美国中南部”位置创建名为 *myResourceGroup* 的资源组。 要查看“免费”层中应用服务支持的所有位置，请运行 [`az appservice list-locations --sku FREE`](/cli/azure/appservice?view=azure-cli-latest#az_appservice_list_locations) 命令。

```azurecli-interactive
az group create --name myResourceGroup --location "china north"
```

通常在附近的区域中创建资源组和资源。 

此命令完成后，JSON 输出会显示资源组属性。