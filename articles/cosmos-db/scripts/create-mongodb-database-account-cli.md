---
title: Azure CLI 脚本 - 创建 Azure Cosmos DB MongoDB API 帐户、数据库和集合 | Azure
description: Azure CLI 脚本示例 - 创建 Azure Cosmos DB MongoDB API 帐户、数据库和集合
author: rockboyfor
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.topic: sample
origin.date: 10/26/2018
ms.date: 12/03/2018
ms.author: v-yeche
ms.openlocfilehash: bc28ec9972b4f03b8d2f2fe5dce4e2c1a9368631
ms.sourcegitcommit: 59db70ef3ed61538666fd1071dcf8d03864f10a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52675298"
---
# <a name="azure-cosmos-db-create-an-mongodb-api-account-using-azure-cli"></a>Azure Cosmos DB：使用 Azure CLI 创建 MongoDB API 帐户

此示例 CLI 脚本创建了一个 Azure Cosmos DB MongoDB API 帐户、数据库和集合。

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

如果选择在本地安装并使用 CLI，本主题要求运行 Azure CLI 2.0 版或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI](https://docs.azure.cn/zh-cn/cli/install-azure-cli?view=azure-cli-latest)。

## <a name="sample-script"></a>示例脚本

```azurecli
#!/bin/bash

# Set variables for the new MongoDB API account, database, and collection
resourceGroupName='myResourceGroup'
location='chinaeast'
accountName='mycosmosdbaccount' #needs to be lower case
databaseName='myDatabase'
collectionName='myCollection'

# Create a resource group
az group create \
    --name $resourceGroupName \
    --location $location

# Create a MongoDB API Cosmos DB account with consistent prefix (Local) consistency and multi-master enabled
az cosmosdb create \
    --resource-group $resourceGroupName \
    --name $accountName \
    --kind MongoDB \
    --locations chinaeast=0 chinanorth=1 \
    --default-consistency-level "ConsistentPrefix" \
    --enable-multiple-write-locations true

# Create a database 
az cosmosdb database create \
    --resource-group $resourceGroupName \
    --name $accountName \
    --db-name $databaseName

# Create a collection with a partition key and 1000 RU/s
az cosmosdb collection create \
    --resource-group $resourceGroupName \
    --collection-name $collectionName \
    --name $accountName \
    --db-name $databaseName \
    --partition-key-path /mypartitionkey \
    --throughput 1000
```
<!-- location ADVISE TO chinaeast -->
<!-- location MUST be the style of --locations chinaeast=0 chinanorth=1 -->
<!-- OR it will popup the index out of range error-->

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
| [az cosmosdb collection create](https://docs.azure.cn/zh-cn/cli/cosmosdb/collection?view=azure-cli-latest#az-cosmosdb-collection-create) | 创建用于 MongoDB 的 Azure Cosmos DB 集合。 |
| [az group delete](https://docs.azure.cn/zh-cn/cli/resource?view=azure-cli-latest#az-resource-delete) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/zh-cn/cli/index?view=azure-cli-latest)。

有关其他 Azure Cosmos DB CLI 脚本示例，请参见 [Azure Cosmos DB CLI 文档](../cli-samples.md)。

<!--Update_Description: update link, wording update, update link -->