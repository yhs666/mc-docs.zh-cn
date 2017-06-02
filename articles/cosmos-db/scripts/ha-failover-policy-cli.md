---
title: "Azure CLI 脚本 - 创建故障转移策略以实现高可用性 | Azure"
description: "Azure CLI 脚本示例 - 创建故障转移策略以实现高可用性"
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
ms.openlocfilehash: fc299db474ae2eb52f90ee349618d367e3b21b84
ms.contentlocale: zh-cn
ms.lasthandoff: 05/19/2017

---

# <a name="create-a-failover-policy-for-high-availability-using-the-azure-cli"></a>使用 Azure CLI 创建故障转移策略以实现高可用性

此示例 CLI 脚本创建一个 Azure Cosmos DB 帐户，然后将其配置为实现高可用性。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>示例脚本

```azurecli-interactive
#!/bin/bash

# Set variables for the new account, database, and collection
resourceGroupName='myResourceGroup'
location='southcentralus'
name='docdb-test'
databaseName='docdb-test-database'
collectionName='docdb-test-collection'

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

# Update failover configuration
az cosmosdb update \
    --name $name \
    --resource-group $resourceGroupName \
    --locations "South Central US"=0 "North Central US"=1 "East US"=2 "West US"=3
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
| [az cosmosdb create](https://docs.microsoft.com/cli/azure/sql/server#create) | 创建 Azure Cosmos DB 帐户。 |
| [az cosmosdb update](https://docs.microsoft.com/cli/azure/cosmosdb#update) | 更新 Azure Cosmos DB 帐户。 |
| [az group delete](https://docs.microsoft.com/cli/azure/resource#delete) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.microsoft.com/cli/azure/overview)。

可以在 [Azure Cosmos DB CLI 文档](../cli-samples.md)中找到其他 Azure Cosmos DB CLI 脚本示例。
