---
title: "Azure CLI 脚本 - Azure Cosmos DB 的多区域复制 | Azure"
description: "Azure CLI 脚本示例 - Azure Cosmos DB 的多区域复制"
services: cosmosdb
documentationcenter: cosmosdb
author: mimig1
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: cosmosdb
ms.custom: sample
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: cosmosdb
ms.workload: database
ms.date: 05/10/2017
wacn.date: 
ms.author: mimig
ms.translationtype: Human Translation
ms.sourcegitcommit: 4a18b6116e37e365e2d4c4e2d144d7588310292e
ms.openlocfilehash: 244176f666d9ba09bd0fdf69429fdfd4ad5f5c3a
ms.contentlocale: zh-cn
ms.lasthandoff: 05/19/2017

---

# <a name="replicate-an-azure-cosmos-db-database-account-in-multiple-regions-and-configure-failover-priorities-using-the-azure-cli"></a>使用 Azure CLI 将 Azure Cosmos DB 数据库帐户复制到多个区域中并配置故障转移优先级

此示例可使用 Azure CLI 将任何类型的 Azure Cosmos DB 数据库帐户复制到多个区域中并配置故障转移优先级。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>示例脚本

```azurecli-interactive
#!/bin/bash

# Set variables for the new account, database, and collection
resourceGroupName='myResourceGroup'
location='southcentralus'
name='docdb-test'

# Create a resource group
az group create \
    --name $resourceGroupName \
    --location $location

# Create a DocumentDB API Cosmos DB account
az cosmosdb create \
    --name $name \
    --kind GlobalDocumentDB \
    --resource-group $resourceGroupName \
    --max-interval 10 \
    --max-staleness-prefix 200 

# Replicate in multiple regions
az cosmosdb update \
    --name $name \
    --resource-group $resourceGroupName \
    --locations "South Central US"=0 "North Central US"=1 "East US"=2 "West US"=3

# Modify regional failover priorities
az cosmosdb update \
    --name $name \
    --resource-group $resourceGroupName \
    --locations "South Central US"=3 "North Central US"=2 "East US"=1 "West US"=0



```

## <a name="clean-up-deployment"></a>清理部署

运行脚本示例后，可以使用以下命令删除资源组以及与其关联的所有资源。

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 说明 |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | 创建用于存储所有资源的资源组。 |
| [az cosmosdb update](https://docs.microsoft.com/cli/azure/cosmosdb/name#update) | 更新 Azure Cosmos DB 帐户。 |
| [az group delete](https://docs.microsoft.com/cli/azure/resource#delete) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.microsoft.com/cli/azure/overview)。

有关其他 Azure Cosmos DB CLI 脚本示例，请参见 [Azure Cosmos DB CLI 文档](../cli-samples.md)。
