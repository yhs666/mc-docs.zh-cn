---
title: Azure CLI 脚本 - 缩放 Azure Cosmos 容器吞吐量 | Azure
description: Azure CLI 脚本示例 - 缩放 Azure Cosmos 容器吞吐量
author: rockboyfor
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
origin.date: 10/26/2018
ms.date: 09/09/2019
ms.author: v-yeche
ms.reviewer: sngun
ms.openlocfilehash: 666c0696cba72ed3681501e9ac0b396adb6b8888
ms.sourcegitcommit: 66192c23d7e5bf83d32311ae8fbb83e876e73534
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/04/2019
ms.locfileid: "70254650"
---
# <a name="scale-azure-cosmos-container-throughput-using-the-azure-cli"></a>使用 Azure CLI 缩放 Azure Cosmos 容器吞吐量

此示例可缩放任何类型的 Azure Cosmos 容器的容器吞吐量。  

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

如果选择在本地安装并使用 CLI，本主题要求运行 Azure CLI 2.0 版或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI](https://docs.azure.cn/cli/install-azure-cli?view=azure-cli-latest)。

## <a name="sample-script"></a>示例脚本

```azurecli
#!/bin/bash

# Set variables for the new account, database, and collection
resourceGroupName='myResourceGroup'
location='chinaeast'
accountName='mycosmosdbaccount'  #needs to be lower case characters
databaseName='myDatabase'
containerName='myContainer'
originalThroughput=1000 
newThroughput=5000

# Create a resource group
az group create \
    --name $resourceGroupName \
    --location $location

# Create a SQL API Cosmos DB account with session consistency and multi-master enabled
az cosmosdb create \
    --name $accountName \
    --kind GlobalDocumentDB \
    --locations regionName="China East" failoverPriority=0 \
    --locations regionName="China North" failoverPriority=1 \
    --resource-group $resourceGroupName \
    --default-consistency-level "Session" \
    --enable-multiple-write-locations true

# Create a database 
az cosmosdb database create \
    --name $accountName \
    --db-name $databaseName \
    --resource-group $resourceGroupName

# Create a fixed-size container and 5000 RU/s
az cosmosdb collection create \
    --resource-group $resourceGroupName \
    --collection-name $containerName \
    --name $accountName \
    --db-name $databaseName \
    --throughput $originalThroughput

read -p "Press any key to set new throughput..."

# Scale throughput
az cosmosdb collection update \
    --collection-name $containerName \
    --name $accountName \
    --db-name $databaseName \
    --resource-group $resourceGroupName \
    --throughput $newThroughput
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
| [az cosmosdb collection create](https://docs.azure.cn/cli/cosmosdb/collection?view=azure-cli-latest#az-cosmosdb-collection-create) | 创建 Azure Cosmos 容器。 |
| [az cosmosdb collection update](https://docs.azure.cn/cli/cosmosdb/collection?view=azure-cli-latest#az-cosmosdb-collection-update) | 更新 Azure Cosmos 容器。 |
| [az group delete](https://docs.azure.cn/cli/group?view=azure-cli-latest#az-group-delete) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/cli/index?view=azure-cli-latest)。

有关其他 Azure Cosmos DB CLI 脚本示例，请参见 [Azure Cosmos DB CLI 文档](../cli-samples.md)。

<!--Update_Description: update meta properties, wording update -->