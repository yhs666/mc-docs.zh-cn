---
title: Azure CLI 脚本 - 创建故障转移策略以实现高可用性 | Azure
description: Azure CLI 脚本示例 - 创建故障转移策略以实现高可用性
services: cosmos-db
documentationcenter: cosmosdb
author: rockboyfor
manager: digimobile
tags: azure-service-management
ms.assetid: ''
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: cosmosdb
ms.workload: database
origin.date: 06/02/2017
ms.date: 04/23/2018
ms.author: v-yeche
ms.openlocfilehash: 127378d6728c4774e57c46d5fc86190b7d205839
ms.sourcegitcommit: c4437642dcdb90abe79a86ead4ce2010dc7a35b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2018
---
# <a name="create-a-failover-policy-for-high-availability-using-the-azure-cli"></a>使用 Azure CLI 创建故障转移策略以实现高可用性

此示例 CLI 脚本创建一个 Azure Cosmos DB 帐户，并将其配置为实现高可用性。

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

如果选择在本地安装并使用 CLI，本主题要求运行 Azure CLI 2.0 版或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI 2.0](https://docs.azure.cn/zh-cn/cli/install-azure-cli?view=azure-cli-latest)。 

## <a name="sample-script"></a>示例脚本

```azurecli
#!/bin/bash

# Set variables for the new account, database, and collection
resourceGroupName='myResourceGroup'
location='chinanorth'
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
    --locations chinanorth=0 chinaeast=1
```
<!-- location ADVISE TO chinanorth -->
<!-- location MUST be the style of --locations chinanorth=0 chinaeast=1 -->
<!-- OR it will popup the index out of range error-->


## <a name="clean-up-deployment"></a>清理部署

运行脚本示例后，可以使用以下命令删除资源组以及与其关联的所有资源。

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [az group create](https://docs.azure.cn/zh-cn/cli/group?view=azure-cli-latest#az_group_create) | 创建用于存储所有资源的资源组。 |
| [az cosmosdb create](https://docs.azure.cn/zh-cn/cli/sql/server?view=azure-cli-latest#az_sql_server_create) | 创建 Azure Cosmos DB 帐户。 |
| [az cosmosdb update](https://docs.azure.cn/zh-cn/cli/cosmosdb?view=azure-cli-latest#az_cosmosdb_update) | 更新 Azure Cosmos DB 帐户。 |
| [az group delete](https://docs.azure.cn/zh-cn/cli/resource?view=azure-cli-latest#az_resource_delete) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/zh-cn/cli/overview?view=azure-cli-latest)。

有关其他 Azure Cosmos DB CLI 脚本示例，请参见 [Azure Cosmos DB CLI 文档](../cli-samples.md)。

<!--Update_Description: update link, wording update-->