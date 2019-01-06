---
title: Azure CLI 脚本 - 获取 Azure Cosmos DB 的帐户密钥
description: Azure CLI 脚本示例 - 获取 Azure Cosmos DB 的帐户密钥
author: rockboyfor
ms.author: v-yeche
ms.service: cosmos-db
ms.topic: sample
origin.date: 10/26/2018
ms.date: 12/31/2018
ms.openlocfilehash: 7fb248e47fd063b61218388fee38b6e6e9300fb7
ms.sourcegitcommit: 54ddd3dc2452d7af3a6fa66dae908ad0c4ef99dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/29/2018
ms.locfileid: "53814732"
---
# <a name="get-account-keys-for-azure-cosmos-db-using-the-azure-cli"></a>使用 Azure CLI 获取 Azure Cosmos DB 的帐户密钥

此示例可获取任何类型的 Azure Cosmos DB 帐户的帐户密钥。  

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

如果选择在本地安装并使用 CLI，本主题要求运行 Azure CLI 2.0 版或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI](https://docs.azure.cn/zh-cn/cli/install-azure-cli?view=azure-cli-latest)。

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

# Create a SQL API Cosmos DB account with session consistency in two regions
az cosmosdb create \
    --name $accountName \
    --kind GlobalDocumentDB \
    --locations chinaeast=0 chinanorth=1 \
    --resource-group $resourceGroupName \
    --default-consistency-level "Session"

# List account keys
az cosmosdb list-keys \
    --name $accountName \
    --resource-group $resourceGroupName 
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
| [az cosmosdb list-keys](https://docs.azure.cn/zh-cn/cli/cosmosdb?view=azure-cli-latest#az-cosmosdb-list-keys) | 列出 Cosmos DB 帐户的访问密钥。 |
| [az group delete](https://docs.azure.cn/zh-cn/cli/group?view=azure-cli-latest#az-group-delete) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/zh-cn/cli/index?view=azure-cli-latest)。

有关其他 Azure Cosmos DB CLI 脚本示例，请参见 [Azure Cosmos DB CLI 文档](../cli-samples.md)。

<!--Update_Description: update meta properties -->