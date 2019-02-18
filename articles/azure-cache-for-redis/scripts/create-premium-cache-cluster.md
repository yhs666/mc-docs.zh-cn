---
title: Azure CLI 脚本示例 - 通过群集创建高级 Azure Redis 缓存 | Microsoft Docs
description: Azure CLI 脚本示例 - 通过群集创建高级层 Azure Redis 缓存
services: azure-cache-for-redis
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
ms.date: 02/12/2019
ms.author: v-junlch
ms.openlocfilehash: 78d8c9e660b07c6af80ffb759ff58508730a3643
ms.sourcegitcommit: c353902162a12f21aecbcbcde89f92c7ff9de441
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/12/2019
ms.locfileid: "56096543"
---
# <a name="create-a-premium-azure-cache-for-redis-with-clustering"></a>通过群集创建高级 Azure Redis 缓存

在此方案中，了解如何通过启用的群集与两个分片创建 6 GB 高级层 Azure Redis 缓存。

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

此脚本使用以下命令创建资源组并通过启用群集创建高级层 Azure Redis 缓存。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [az group create](/cli/group) | 创建用于存储所有资源的资源组。 |
| [az redis create](/cli/redis) | 创建 Azure Redis 缓存实例。 |


## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](/cli)。

可以在 [Azure Redis 缓存文档](../cli-samples.md)中找到其他 Azure Redis 缓存 CLI 脚本示例。

<!-- Update_Description: link update -->