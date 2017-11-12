---
title: "Azure CLI 脚本示例 - 创建 Azure Redis 缓存 | Microsoft Docs"
description: "Azure CLI 脚本示例 - 创建 Azure Redis 缓存"
services: redis-cache
documentationcenter: 
author: alexchen2016
manager: digimobile
editor: 
tags: azure-service-management
ms.assetid: afd7f6e0-9297-4c98-a95e-597be939cef7
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
origin.date: 08/30/2017
ms.date: 10/10/2017
ms.author: v-junlch
ms.openlocfilehash: 2cf5dc12357d442f58211c5926247224ab770732
ms.sourcegitcommit: 01b8f9a7e857463f49531e70dbb911c6f0286d76
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2017
---
# <a name="create-an-azure-redis-cache"></a>创建 Azure Redis 缓存

此方案介绍如何创建 Azure Redis 缓存。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>示例脚本

```azurecli
#/bin/bash

# Creates a Resource Group named contosoGroup, and creates a Redis Cache in that group named contosoCache

# Create a Resource Group 
az group create --name contosoGroup --location chinaeast

# Create a Redis Cache
az redis create --name contosoCache --resource-group contosoGroup --location chinaeast --sku-capacity 0 --sku-family C --sku-name Basic

```

[!INCLUDE [cli-script-clean-up](../../../includes/redis-cli-script-clean-up.md)]

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建资源组和 Redis 缓存。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 说明 |
|---|---|
| [az group create](/cli/group#az_group_create) | 创建用于存储所有资源的资源组。 |
| [az redis create](/cli/redis#az_redis_create) | 创建 Redis 缓存实例。 |


## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](/cli/overview)。

可以在 [Azure Redis 缓存文档](../cli-samples.md)中找到其他 Azure Redis 缓存 CLI 脚本示例。

<!--Update_Description: wording update-->
