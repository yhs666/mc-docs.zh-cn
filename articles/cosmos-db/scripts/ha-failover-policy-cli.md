---
title: Azure CLI 脚本 - 创建故障转移策略以实现高可用性
description: Azure CLI 脚本示例 - 创建故障转移策略以实现高可用性
author: rockboyfor
ms.author: v-yeche
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
origin.date: 10/26/2018
ms.date: 01/21/2019
ms.reviewer: sngun
ms.openlocfilehash: ce6c57f523a46bd050e1dedbb37ee9f4e53ff0ea
ms.sourcegitcommit: 66192c23d7e5bf83d32311ae8fbb83e876e73534
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/04/2019
ms.locfileid: "70254862"
---
# <a name="create-a-failover-policy-for-high-availability-using-the-azure-cli"></a>使用 Azure CLI 创建故障转移策略以实现高可用性

此示例 CLI 脚本创建一个 Azure Cosmos DB 帐户，并将其配置为实现高可用性。

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
    --resource-group $resourceGroupName \
    --name $accountName \
    --kind GlobalDocumentDB \
    --locations chinaeast=0 chinanorth=1 \
    --default-consistency-level "Session"

# Update failover configuration
az cosmosdb update \
    --name $accountName \
    --resource-group $resourceGroupName \
    --locations chinaeast=0 chinanorth=1 chinaeast2=2 chinanorth2=3
```

<!-- location ADVISE TO chinaeast -->
<!-- location MUST be the style of --locations chinaeast=0 chinanorth=1 chinaeast2=2 chinanorth2=3 -->
<!-- OR it will popup the index out of range error-->
<!--verify successuflly-->
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
| [az cosmosdb create](https://docs.azure.cn/cli/sql/server?view=azure-cli-latest#az-sql-server-create) | 创建 Azure Cosmos DB 帐户。 |
| [az cosmosdb update](https://docs.azure.cn/cli/cosmosdb?view=azure-cli-latest#az-cosmosdb-update) | 更新 Azure Cosmos DB 帐户。 |
| [az group delete](https://docs.azure.cn/cli/resource?view=azure-cli-latest#az-resource-delete) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/cli/index?view=azure-cli-latest)。

有关其他 Azure Cosmos DB CLI 脚本示例，请参见 [Azure Cosmos DB CLI 文档](../cli-samples.md)。

<!--Update_Description: update meta properties -->