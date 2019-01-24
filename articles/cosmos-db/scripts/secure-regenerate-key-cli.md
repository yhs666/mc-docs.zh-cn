---
title: Azure CLI 脚本 - 重新生成 Azure Cosmos DB 帐户密钥
description: Azure CLI 脚本示例 - 重新生成 Azure Cosmos DB 帐户密钥
author: rockboyfor
ms.author: v-yeche
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
origin.date: 10/26/2018
ms.date: 01/21/2019
ms.reviewer: sngun
ms.openlocfilehash: 76be340e2afa03fd19249ff06125dcf5ba7f5c0e
ms.sourcegitcommit: d0d76d2fefe064d9cd710f5f41c5015d44c753aa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/21/2019
ms.locfileid: "54417341"
---
# <a name="regenerate-an-azure-cosmos-db-account-key-using-the-azure-cli"></a>使用 Azure CLI 重新生成 Azure Cosmos DB 帐户密钥

此示例可使用 Azure CLI 重新生成任何类型的 Azure Cosmos DB 帐户密钥。

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

如果选择在本地安装并使用 CLI，本主题要求运行 Azure CLI 2.0 版或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI](https://docs.azure.cn/zh-cn/cli/install-azure-cli?view=azure-cli-latest)。

## <a name="sample-script"></a>示例脚本

```azurecli
#!/bin/bash

# Set variables for the new account
resourceGroupName='myResourceGroup'
location='chinaeast'
accountName='mycosmosdbaccount' #needs to be lower case characters

# Create a resource group
az group create \
    --name $resourceGroupName \
    --location $location

# Create a SQL API Cosmos DB account with session consistency and multi-master enabled
az cosmosdb create \
    --name $accountName \
    --kind GlobalDocumentDB \
    --locations chinaeast=0 chinanorth=1 \
    --resource-group $resourceGroupName \
    --default-consistency-level "Session" \
    --enable-multiple-write-locations true

# List account keys
az cosmosdb list-keys \
    --name $accountName \
    --resource-group $resourceGroupName

read -p "Press any key to regenerate account keys..."

# Regenerate secondary account keys
# key-kind values: primary, primaryReadonly, secondary, secondaryReadonly
az cosmosdb regenerate-key \
    --name $accountName \
    --resource-group $resourceGroupName \
    --key-kind secondary
```

<!-- location ADVISE TO chinanorth -->
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
| [az cosmosdb create](https://docs.azure.cn/zh-cn/cli/cosmosdb?view=azure-cli-latest#az-cosmosdb-create) | 更新 Azure Cosmos DB 帐户。 |
| [az cosmosdb list-keys](https://docs.azure.cn/zh-cn/cli/cosmosdb?view=azure-cli-latest#az-cosmosdb-list-keys) | 列出 Cosmos DB 帐户的访问密钥。 |
| [az cosmosdb regenerate-key](https://docs.azure.cn/zh-cn/cli/cosmosdb?view=azure-cli-latest#az-cosmosdb-regenerate-key) | 重新生成 Azure Cosmos DB 帐户密钥。 |
| [az group delete](https://docs.azure.cn/zh-cn/cli/group?view=azure-cli-latest#az-group-delete) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/zh-cn/cli/index?view=azure-cli-latest)。

有关其他 Azure Cosmos DB CLI 脚本示例，请参见 [Azure Cosmos DB CLI 文档](../cli-samples.md)。

<!--Update_Description: update meta properties -->