---
title: Azure CLI 脚本 - Azure Cosmos DB 的多区域复制
description: Azure CLI 脚本示例 - Azure Cosmos DB 的多区域复制
author: rockboyfor
ms.author: v-yeche
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
origin.date: 10/26/2018
ms.date: 09/09/2019
ms.reviewer: sngun
ms.openlocfilehash: ec991e0a64ad7c3438d43c1815b4c89c524ad278
ms.sourcegitcommit: 66192c23d7e5bf83d32311ae8fbb83e876e73534
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/04/2019
ms.locfileid: "70254653"
---
# <a name="replicate-an-azure-cosmos-database-account-in-multiple-regions-and-configure-failover-priorities-using-the-azure-cli"></a>使用 Azure CLI 将 Azure Cosmos 数据库帐户复制到多个区域中并配置故障转移优先级

此示例可使用 Azure CLI 将任何类型的 Azure Cosmos 数据库帐户复制到多个区域中并配置故障转移优先级。

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

如果选择在本地安装并使用 CLI，本主题要求运行 Azure CLI 2.0 版或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI](https://docs.azure.cn/cli/install-azure-cli?view=azure-cli-latest)。

## <a name="sample-script"></a>示例脚本

```azurecli
#!/bin/bash

# Set variables for the new account
resourceGroupName='myResourceGroup'
location='chinaeast'
accountName='myaccountname' #needs to be lower case

# Create a resource group
az group create \
    --name $resourceGroupName \
    --location $location

# Create a SQL API Cosmos DB account with session consistency
az cosmosdb create \
    --name $accountName \
    --kind GlobalDocumentDB \
    --resource-group $resourceGroupName \
    --default-consistency-level "Session"

read -p "Press any key to add locations..."

# Replicate in multiple regions
az cosmosdb update \
    --name $accountName \
    --resource-group $resourceGroupName \
    --locations regionName="China East" failoverPriority=0 \
    --locations regionName="China North" failoverPriority=1 \
    --locations regionName="China East 2" failoverPriority=2 \
    --locations regionName="China North 2" failoverPriority=3

read -p "Press any key to change failover regions..."

# Modify regional failover priorities
az cosmosdb update \
    --name $accountName \
    --resource-group $resourceGroupName \
    --locations regionName="China East" failoverPriority=3 \
    --locations regionName="China North" failoverPriority=2 \
    --locations regionName="China East 2" failoverPriority=1 \
    --locations regionName="China North 2" failoverPriority=0

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
| [az group create](https://docs.azure.cn/cli/group?view=azure-cli-latest#az-group-create) | 创建用于存储所有资源的资源组。 |
| [az cosmosdb update](https://docs.azure.cn/cli/cosmosdb?view=azure-cli-latest#az-cosmosdb-update) | 更新 Azure Cosmos DB 帐户。 |
| [az group delete](https://docs.azure.cn/cli/group?view=azure-cli-latest#az-group-delete) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/cli/index?view=azure-cli-latest)。

有关其他 Azure Cosmos DB CLI 脚本示例，请参见 [Azure Cosmos DB CLI 文档](../cli-samples.md)。

<!--Update_Description: update meta properties, wording update -->