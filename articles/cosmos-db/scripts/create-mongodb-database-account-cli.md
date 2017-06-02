---
title: "Azure CLI 脚本 - 创建 Azure Cosmos DB MongoDB API 帐户、数据库和集合 | Azure"
description: "Azure CLI 脚本示例 - 创建 Azure Cosmos DB MongoDB API 帐户、数据库和集合"
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
ms.openlocfilehash: d21812b261ea0810152497cb386ffa716af817bd
ms.contentlocale: zh-cn
ms.lasthandoff: 05/19/2017

---

# <a name="azure-cosmos-db-create-an-mongodb-api-account-using-the-azure-cli"></a>Azure Cosmos DB：使用 Azure CLI 创建 MongoDB API 帐户

此示例 CLI 脚本创建了一个 Azure Cosmos DB MongoDB API 帐户、数据库和集合。 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>示例脚本

```azurecli-interactive
#!/bin/bash

# Set variables for the new account, database, and collection
resourceGroupName='myResourceGroup'
location='southcentralus'
name='docdb-test'
databaseName='docdb-mongodb-database'
collectionName='docdb-mongodb-collection'

# Create a resource group
az group create \
    --name $resourceGroupName \
    --location $location

# Create a MongoDB API Cosmos DB account
az cosmosdb create \
    --name $name \
    --kind MongoDB \
    --locations "South Central US"=0 "North Central US"=1 \
    --resource-group $resourceGroupName \
    --max-interval 10 \
    --max-staleness-prefix 200

# Create a database 
az cosmosdb database create \
    --name $name \
    --db-name $databaseName \
    --resource-group $resourceGroupName

# Create a collection
az cosmosdb collection create \
    --collection-name $collectionName \
    --name $name \
    --db-name $databaseName \
    --resource-group $resourceGroupName
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
| [az cosmosdb create](https://docs.microsoft.com/cli/azure/cosmosdb#create) | 创建 Azure Cosmos DB 帐户。 |
| [az group delete](https://docs.microsoft.com/cli/azure/resource#delete) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.microsoft.com/cli/azure/overview)。

可以在 [Azure Cosmos DB CLI 文档](../cli-samples.md)中找到其他 Azure Cosmos DB CLI 脚本示例。
