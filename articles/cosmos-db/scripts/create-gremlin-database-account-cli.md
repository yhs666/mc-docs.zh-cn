---
title: Azure CLI 脚本 - 创建 Azure Cosmos DB Gremlin API 帐户、数据库和图形
description: Azure CLI 脚本示例 - 创建 Azure Cosmos DB Gremlin API 帐户、数据库和图形
author: rockboyfor
ms.author: v-yeche
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: sample
origin.date: 10/26/2018
ms.date: 3/18/2019
ms.reviewer: sngun
ms.openlocfilehash: 2e81bb3126d590b28aa91c040bb503372ba93e8c
ms.sourcegitcommit: c5646ca7d1b4b19c2cb9136ce8c887e7fcf3a990
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2019
ms.locfileid: "58005115"
---
# <a name="azure-cosmos-db-create-a-gremlin-api-account-using-azure-cli"></a>Azure Cosmos DB：使用 Azure CLI 创建 Gremlin API 帐户

此示例 CLI 脚本创建了 Azure Cosmos DB Gremlin API 帐户、数据库和图形。

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

如果选择在本地安装并使用 CLI，本主题要求运行 Azure CLI 2.0 版或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI](https://docs.azure.cn/zh-cn/cli/install-azure-cli?view=azure-cli-latest)。

## <a name="sample-script"></a>示例脚本

```azurecli
#!/bin/bash

# Set variables for the new account, database, and graph
resourceGroupName='myResourceGroup'
location='chinaeast'
accountName='myaccountname' #needs to be lower case
databaseName='myDatabase'
graphName='myGraph'

# Create a resource group
az group create \
    --name $resourceGroupName \
    --location $location

# Create a Gremlin API Cosmos DB account with session consistency and multi-master enabled
az cosmosdb create \
    --resource-group $resourceGroupName \
    --name $accountName \
    --capabilities EnableGremlin \
    --locations "China East"=0 "China North"=1 \
    --default-consistency-level "Session" \
    --enable-multiple-write-locations true

# Create a database 
az cosmosdb database create \
    --name $accountName \
    --db-name $databaseName \
    --resource-group $resourceGroupName

# Create a graph with a partition key and 1000 RU/s
az cosmosdb collection create \
    --collection-name $graphName \
    --name $accountName \
    --db-name $databaseName \
    --resource-group $resourceGroupName \
    --partition-key-path /mypartitionkey \
    --throughput 1000
```

## <a name="clean-up-deployment"></a>清理部署

运行脚本示例后，可以使用以下命令删除资源组以及与其关联的所有资源。

```azurecli
az group delete --name $resourceGroupName
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [az group create](https://docs.azure.cn/zh-cn/cli/group?view=azure-cli-latest#az-group-create) | 创建用于存储所有资源的资源组。 |
| [az cosmosdb create](https://docs.azure.cn/zh-cn/cli/cosmosdb?view=azure-cli-latest#az-cosmosdb-create) | 创建 Azure Cosmos DB 帐户。 |
| [az cosmosdb database create](https://docs.azure.cn/zh-cn/cli/cosmosdb/database?view=azure-cli-latest#az-cosmosdb-database-create) | 创建 Azure Cosmos DB 数据库。 |
| [az cosmosdb collection create](https://docs.azure.cn/zh-cn/cli/cosmosdb/collection?view=azure-cli-latest#az-cosmosdb-collection-create) | 为 Gremlin 创建 Azure Cosmos DB 图形。 |
| [az group delete](https://docs.azure.cn/zh-cn/cli/resource?view=azure-cli-latest#az-resource-delete) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/zh-cn/cli/index?view=azure-cli-latest#az-CLI-documentation)。

有关其他 Azure Cosmos DB CLI 脚本示例，请参见 [Azure Cosmos DB CLI 文档](../cli-samples.md)。