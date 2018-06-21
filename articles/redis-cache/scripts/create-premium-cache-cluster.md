---
title: Azure CLI 脚本示例 - 通过群集创建高级 Azure Redis 缓存 | Microsoft Docs
description: Azure CLI 脚本示例 - 通过群集创建高级层 Azure Redis 缓存
services: redis-cache
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
tags: azure-service-management
ms.assetid: 07bcceae-2521-4fe3-b88f-ed833104ddd2
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
origin.date: 08/30/2017
ms.date: 03/01/2018
ms.author: v-junlch
ms.openlocfilehash: 320161df155bea5464b9eb463f6b2ae6d5c0ac9b
ms.sourcegitcommit: 34925f252c9d395020dc3697a205af52ac8188ce
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
ms.locfileid: "29730944"
---
# <a name="create-a-premium-azure-redis-cache-with-clustering"></a>使用群集功能创建高级 Azure Redis 缓存

本方案介绍如何创建大小为 6 GB、启用了群集功能且包含两个分片的高级层 Azure Redis 缓存。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>示例脚本

```azurecli
#/bin/bash

# Creates a Resource Group named contosoGroup, and creates a Premium Redis Cache with clustering in that group named contosoCache

# Create a Resource Group 
az group create --name contosoGroup --location chinanorth

# Create a Premium P1 (6 GB) Redis Cache with clustering enabled and 2 shards (for a total of 12 GB)
az redis create --name contosoCache --resource-group contosoGroup --location chinanorth --vm-size P1 --sku Premium --shard-count 2
```

[!INCLUDE [cli-script-clean-up](../../../includes/redis-cli-script-clean-up.md)]

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建资源组以及启用了群集功能的高级层 Redis 缓存。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [az group create](/cli/group#az_group_create) | 创建用于存储所有资源的资源组。 |
| [az redis create](/cli/redis#az_redis_create) | 创建 Redis 缓存实例。 |


## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](/cli/overview)。

可以在 [Azure Redis 缓存文档](../cli-samples.md)中找到其他 Azure Redis 缓存 CLI 脚本示例。

<!--Update_Description: code update-->
