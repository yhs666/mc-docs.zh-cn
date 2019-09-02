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
ms.openlocfilehash: ee6a31b59c06c2a695daa0f52485f0b247b09d4f
ms.sourcegitcommit: b8aa5d05ef46f1db2df4f2653cdd8d150e847113
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/25/2019
ms.locfileid: "54906293"
---
[!INCLUDE [resource group intro text](resource-group.md)]

在 CLI 中，使用 [`az group create`](/cli/azure/group?view=azure-cli-latest#az_group_create) 命令创建资源组。 下面的示例命令在“美国中南部”位置创建名为 *myResourceGroup* 的资源组。 要查看“免费”层中应用服务支持的所有位置，请运行 [`az appservice list-locations --sku FREE`](/cli/azure/appservice?view=azure-cli-latest#az_appservice_list_locations) 命令。

```azurecli
az group create --name myResourceGroup --location "china north"
```

通常在附近的区域中创建资源组和资源。 

此命令完成后，JSON 输出会显示资源组属性。