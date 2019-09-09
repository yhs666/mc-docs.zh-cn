---
title: Azure CLI 脚本 - 使用 Azure Cosmos DB 的用于 MongoDB 的 API 创建 Cosmos 帐户
description: Azure CLI 脚本示例 - 使用 Azure Cosmos DB 的用于 MongoDB 的 API 创建 Cosmos 帐户
author: rockboyfor
ms.author: v-yeche
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: sample
origin.date: 07/02/2019
ms.date: 09/09/2019
ms.reviewer: sngun
ms.openlocfilehash: e4cafd9dcc693f68e358ccb2820faadef2ac648b
ms.sourcegitcommit: 66192c23d7e5bf83d32311ae8fbb83e876e73534
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/04/2019
ms.locfileid: "70254865"
---
# <a name="create-an-azure-cosmos-db-account-with-azure-cosmos-dbs-api-for-mongodb-using-azure-cli"></a>使用 Azure CLI 通过 Azure Cosmos DB 的用于 MongoDB 的 API 创建 Azure Cosmos DB 帐户

此示例 CLI 脚本通过 Azure Cosmos DB 的用于 MongoDB 的 API 创建 Cosmos 帐户。

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

如果选择在本地安装并使用 CLI，本主题要求运行 Azure CLI 2.0 版或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI](https://docs.azure.cn/cli/install-azure-cli?view=azure-cli-latest)。

[!NOTE] 若要了解有关数据库和容器命名约定的详细信息，请参阅[使用 Azure Cosmos DB 中的数据库、容器和项目](../databases-containers-items.md)。

## <a name="sample-script"></a>示例脚本

```azurecli
#!/bin/bash

# Set variables for the new MongoDB API account and database
resourceGroupName='myResourceGroup'
location='chinaeast'
accountName='mycosmosdbaccount' #needs to be lower case
databaseName='myDatabase'

# Create a resource group
az group create \
    --name $resourceGroupName \
    --location $location

# Create a MongoDB API Cosmos DB account with consistent prefix (Local) consistency and multi-master enabled
az cosmosdb create \
    --resource-group $resourceGroupName \
    --name $accountName \
    --kind MongoDB \
    --locations regionName="China East" failoverPriority=0 \
    --locations regionName="China North" failoverPriority=1 \
    --default-consistency-level "ConsistentPrefix" \
    --enable-multiple-write-locations true

# Create a database 
az cosmosdb database create \
    --resource-group $resourceGroupName \
    --name $accountName \
    --db-name $databaseName

# Create a MongoDB container with a partition key and 400 RU/s
az cosmosdb collection create \
    --resource-group $resourceGroupName \
    --collection-name $containerName \
    --name $accountName \
    --db-name $databaseName \
    --partition-key-path /mypartitionkey \
    --throughput 400

```

<!--Verify sucessfully-->
<!--Correct on --locations regionName="China East" failoverPriority=0-->

## <a name="clean-up-deployment"></a>清理部署

运行脚本示例后，可以使用以下命令删除资源组以及与其关联的所有资源。

```azurecli
az group delete --name $resourceGroupName
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [az group create](https://docs.azure.cn/cli/group?view=azure-cli-latest#az-group-create) | 创建用于存储所有资源的资源组。 |
| [az cosmosdb create](https://docs.azure.cn/cli/cosmosdb?view=azure-cli-latest#az-cosmosdb-create) | 创建 Azure Cosmos DB 帐户。 |
| [az cosmosdb database create](https://docs.azure.cn/cli/cosmosdb/database?view=azure-cli-latest#az-cosmosdb-database-create) | 创建 Azure Cosmos 数据库。 |
| [az cosmosdb collection create](https://docs.azure.cn/cli/cosmosdb/collection?view=azure-cli-latest#az-cosmosdb-collection-create) | 创建用于 MongoDB 的 Azure Cosmos 容器。 |
| [az group delete](https://docs.azure.cn/cli/group?view=azure-cli-latest#az-group-delete) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/cli/index?view=azure-cli-latest)。

有关其他 Azure Cosmos DB CLI 脚本示例，请参见 [Azure Cosmos DB CLI 文档](../cli-samples.md)。

<!--Update_Description: update meta properties， wording update -->