---
title: "Azure CLI 脚本示例 - 获取 MongoDB 应用的 Azure Cosmos DB 连接字符串 | Azure"
description: "Azure CLI 脚本示例 - 获取 MongoDB 应用的 Azure Cosmos DB 连接字符串"
services: cosmos-db
documentationcenter: cosmosdb
author: rockboyfor
manager: digimobile
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: cosmosdb
ms.workload: database
origin.date: 06/02/2017
ms.date: 07/17/2017
ms.author: v-yeche
ms.openlocfilehash: 1200aa560a66b15c63a23eae5db3d64a847c759b
ms.sourcegitcommit: b15d77b0f003bef2dfb9206da97d2fe0af60365a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/07/2017
---
# 使用 Azure CLI 获取 MongoDB 应用的 Azure Cosmos DB 连接字符串
<a id="get-an-azure-cosmos-db-connection-string-for-mongodb-apps-using-the-azure-cli" class="xliff"></a>

此示例使用 Azure CLI 获取 MongoDB 应用的 Azure Cosmos DB 连接字符串。 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]
<!-- Not Available [!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)] -->

## 示例脚本
<a id="sample-script" class="xliff"></a>

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

# Get the connection string for MongoDB apps
az cosmosdb list-connection-strings \
    --name $name \
    --resource-group $resourceGroupName 

```

## 清理部署
<a id="clean-up-deployment" class="xliff"></a>

运行脚本示例后，可以使用以下命令删除资源组以及与其关联的所有资源。

```azurecli-interactive
az group delete --name myResourceGroup
```

## 脚本说明
<a id="script-explanation" class="xliff"></a>

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 说明 |
|---|---|
| [az group create](https://docs.microsoft.com/zh-cn/cli/azure/group#create) | 创建用于存储所有资源的资源组。 |
| [az cosmosdb update](https://docs.microsoft.com/zh-cn/cli/azure/cosmosdb/name#update) | 更新 Azure Cosmos DB 帐户。 |
| [az cosmosdb list-connection-strings](https://docs.microsoft.com/zh-cn/cli/azure/cosmosdb/list-connection-strings) | 获取帐户的连接字符串。|
| [az group delete](https://docs.microsoft.com/zh-cn/cli/azure/resource#delete) | 删除资源组，包括所有嵌套的资源。 |

## 后续步骤
<a id="next-steps" class="xliff"></a>

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.microsoft.com/zh-cn/cli/azure/overview)。

有关其他 Azure Cosmos DB CLI 脚本示例，请参见 [Azure Cosmos DB CLI 文档](../cli-samples.md)。