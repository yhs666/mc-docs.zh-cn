---
title: Azure CLI 脚本 - Azure Cosmos DB 的多区域复制 | Azure
description: Azure CLI 脚本示例 - Azure Cosmos DB 的多区域复制
services: cosmos-db
documentationcenter: cosmosdb
author: rockboyfor
manager: digimobile
tags: azure-service-management
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: cosmosdb
ms.workload: database
origin.date: 06/02/2017
ms.date: 11/05/2018
ms.author: v-yeche
ms.openlocfilehash: c3237848245c90dccb0e63aa34e14e4d07c2d720
ms.sourcegitcommit: c1020b13c8810d50b64e1f27718e9f25b5f9f043
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2018
ms.locfileid: "50204813"
---
# <a name="replicate-an-azure-cosmos-db-database-account-in-multiple-regions-and-configure-failover-priorities-using-the-azure-cli"></a>使用 Azure CLI 将 Azure Cosmos DB 数据库帐户复制到多个区域中并配置故障转移优先级

此示例可使用 Azure CLI 将任何类型的 Azure Cosmos DB 数据库帐户复制到多个区域中并配置故障转移优先级。

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

如果选择在本地安装并使用 CLI，本主题要求运行 Azure CLI 2.0 版或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI](https://docs.azure.cn/zh-cn/cli/install-azure-cli?view=azure-cli-latest)。 

## <a name="sample-script"></a>示例脚本

```azurecli
#!/bin/bash

# Set variables for the new account, database, and collection
resourceGroupName='myResourceGroup'
location='chinanorth'
name='docdb-test'

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

# Replicate in multiple regions
az cosmosdb update \
    --name $name \
    --resource-group $resourceGroupName \
    --locations chinanorth=0 chinaeast=1 chinanorth2=2 chinaeast2=3

# Modify regional failover priorities
az cosmosdb update \
    --name $name \
    --resource-group $resourceGroupName \
    --locations chinanorth=3 chinaeast=2 chinanorth2=1 chinaeast2=0

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
| [az group create](https://docs.azure.cn/zh-cn/cli/group?view=azure-cli-latest#az-group-create) | 创建用于存储所有资源的资源组。 |
| [az cosmosdb update](https://docs.azure.cn/zh-cn/cli/cosmosdb?view=azure-cli-latest#az-cosmosdb-update) | 更新 Azure Cosmos DB 帐户。 |
| [az group delete](https://docs.azure.cn/zh-cn/cli/group?view=azure-cli-latest#az-group-delete) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/zh-cn/cli/index?view=azure-cli-latest)。

有关其他 Azure Cosmos DB CLI 脚本示例，请参见 [Azure Cosmos DB CLI 文档](../cli-samples.md)。

<!--Update_Description: update link, wording update-->