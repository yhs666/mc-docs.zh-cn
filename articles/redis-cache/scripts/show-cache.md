---
title: "Azure CLI 脚本示例 - 获取 Azure Redis 缓存的详细信息 | Azure"
description: "Azure CLI 脚本示例 - 获取 Azure Redis 缓存的详细信息"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
tags: azure-service-management
ms.assetid: 155924e6-00d5-4a8c-ba99-5189f300464a
ms.service: redis-cache
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
origin.date: 04/14/2017
ms.date: 05/02/2017
ms.author: v-dazen
ms.openlocfilehash: a5d304da5abf8bf181dbba1e37add351bcf3245a
ms.sourcegitcommit: 033f4f0e41d31d256b67fc623f12f79ab791191e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
# <a name="get-details-of-an-azure-redis-cache"></a>获取 Azure Redis 缓存的详细信息

在此方案中，你将了解如何检索 Azure Redis 缓存实例的详细信息，包括其预配状态。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>示例脚本

```azurecli
#/bin/bash

# Retrieve the details for an Azure Redis Cache instance named contosoCache in the Resource Group contosoGroup
# This script shows details such as hostname, ports, and provisioning status
az redis show --name contosoCache --resource-group contosoGroup 
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令检索 Azure Redis 缓存实例的详细信息。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 说明 |
|---|---|
| [az redis show](https://docs.microsoft.com/cli/azure/redis#show) | 检索 Azure Redis 缓存实例的详细信息。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.microsoft.com/cli/azure/overview)。

可以在 [Azure Redis 缓存文档](../cli-samples.md)中找到其他 Azure Redis 缓存 CLI 脚本示例。