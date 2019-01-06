---
title: Azure CLI 脚本示例 - 获取 Azure Redis 缓存的详细信息 | Microsoft Docs
description: Azure CLI 脚本示例 - 获取 Azure Redis 缓存的详细信息
services: azure-cache-for-redis
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
tags: azure-service-management
ms.assetid: 155924e6-00d5-4a8c-ba99-5189f300464a
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
origin.date: 08/30/2017
ms.date: 12/21/2018
ms.author: v-junlch
ms.openlocfilehash: d51ae06911ae4efe931e87dbb124f075f4b459c6
ms.sourcegitcommit: d2893ae6bdbb3784d243d5d3c49c25c9cfd99d9b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2018
ms.locfileid: "53784892"
---
# <a name="get-details-of-an-azure-cache-for-redis"></a>获取 Azure Redis 缓存的详细信息

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

| 命令 | 注释 |
|---|---|
| [az redis show](/cli/redis#az_redis_show) | 检索 Azure Redis 缓存实例的详细信息。 |


## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](/cli)。

可以在 [Azure Redis 缓存文档](../cli-samples.md)中找到其他 Azure Redis 缓存 CLI 脚本示例。

<!--Update_Description: wording update-->
