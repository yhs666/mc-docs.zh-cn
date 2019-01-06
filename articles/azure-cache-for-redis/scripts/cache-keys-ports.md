---
title: Azure CLI 脚本示例 - 获取 Azure Redis 缓存的主机名、端口和密钥 | Microsoft Docs
description: Azure CLI 脚本示例 - 获取 Azure Redis 缓存的主机名、端口和密钥
services: azure-cache-for-redis
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
tags: azure-service-management
ms.assetid: 761eb24e-2ba7-418d-8fc3-431153e69a90
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
origin.date: 08/30/2017
ms.date: 12/21/2018
ms.author: v-junlch
ms.openlocfilehash: d7768826f4f52478cc4825445aea758f5c63b2ea
ms.sourcegitcommit: d2893ae6bdbb3784d243d5d3c49c25c9cfd99d9b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2018
ms.locfileid: "53784891"
---
# <a name="get-the-hostname-ports-and-keys-for-azure-cache-for-redis"></a>获取 Azure Redis 缓存的主机名、端口和密钥

在此方案中，将学习如何检索用于连接到 Azure Redis 缓存实例的主机名、端口和密钥。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>示例脚本

```azurecli
#/bin/bash

# Retrieve the hostname, ports, and keys for contosoCache located in contosoGroup

# Retrieve the hostname and ports for an Azure Redis Cache instance
redis=($(az redis show --name contosoCache --resource-group contosoGroup --query [hostName,enableNonSslPort,port,sslPort] --output tsv))

# Retrieve the keys for an Azure Redis Cache instance
keys=($(az redis list-keys --name contosoCache --resource-group contosoGroup --query [primaryKey,secondaryKey] --output tsv))

# Display the retrieved hostname, keys, and ports
echo "Hostname:" ${redis[0]}
echo "Non SSL Port:" ${redis[2]}
echo "Non SSL Port Enabled:" ${redis[1]}
echo "SSL Port:" ${redis[3]}
echo "Primary Key:" ${keys[0]}
echo "Secondary Key:" ${keys[1]}
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令检索 Azure Redis 缓存实例的主机名、密钥和端口。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [az redis show](/cli/redis#az_redis_show) | 检索 Azure Redis 缓存实例的详细信息。 |
| [az redis list-keys](/cli/redis#az_redis_list_keys) | 检索 Azure Redis 缓存实例的访问密钥。 |


## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](/cli)。

可以在 [Azure Redis 缓存文档](../cli-samples.md)中找到其他 Azure Redis 缓存 CLI 脚本示例。

<!--Update_Description: wording update-->
