---
title: 使用 Azure CLI 管理 Azure Cosmos DB 资源
description: 使用 Azure CLI 管理 Azure Cosmos DB 帐户、数据库和容器。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 05/23/2019
ms.date: 07/29/2019
ms.author: v-yeche
ms.openlocfilehash: 0492fd56ecc4bf4da36e1af0ae7e1e3ac7f25f29
ms.sourcegitcommit: 5a4a826eea3914911fd93592e0f835efc9173133
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/31/2019
ms.locfileid: "68672220"
---
# <a name="manage-azure-cosmos-resources-using-azure-cli"></a>使用 Azure CLI 管理 Azure Cosmos 资源

以下指南介绍了使用 Azure CLI 自动管理 Azure Cosmos DB 帐户、数据库和容器的常见命令。 [Azure CLI 参考](https://docs.azure.cn/zh-cn/cli/cosmosdb?view=azure-cli-latest)中收录了所有 Azure Cosmos DB CLI 命令的参考页。 还可以在[针对 Azure Cosmos DB 的 Azure CLI 示例](cli-samples.md)中找到更多示例，包括如何为 MongoDB、Gremlin、Cassandra 和表 API 创建和管理 Cosmos DB 帐户、数据库和容器。

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

如果选择在本地安装并使用 CLI，本主题要求运行 Azure CLI 2.0 版或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI](https://docs.azure.cn/zh-cn/cli/install-azure-cli?view=azure-cli-latest)。

## <a name="create-an-azure-cosmos-db-account"></a>创建 Azure Cosmos DB 帐户

若要在“中国东部”和“中国北部”区域创建启用了 SQL API 和会话一致性的 Azure Cosmos DB 帐户，请运行以下命令：

<!-- Not Available on cloud shell-->

```azurecli
az cosmosdb create \
   --name mycosmosdbaccount \
   --resource-group myResourceGroup \
   --kind GlobalDocumentDB \
   --default-consistency-level Session \
   --locations regionName=chinaeast failoverPriority=0 isZoneRedundant=False \
   --locations regionName=chinanorth failoverPriority=1 isZoneRedundant=False \
   --enable-multiple-write-locations false
```

> [!IMPORTANT]
> Azure Cosmos 帐户名称必须小写。

## <a name="create-a-database"></a>创建数据库

若要创建 Cosmos DB 数据库，请运行以下命令：

<!-- Not Available on cloud shell-->

```azurecli
az cosmosdb database create \
   --name mycosmosdbaccount \
   --db-name myDatabase \
   --resource-group myResourceGroup
```

## <a name="create-a-container"></a>创建容器

若要创建吞吐量为 400 RU/秒且具有分区键的 Cosmos DB 容器，请运行以下命令：

<!-- Not Available on cloud shell-->

```azurecli
# Create a container
az cosmosdb collection create \
   --collection-name myContainer \
   --name mycosmosdbaccount \
   --db-name myDatabase \
   --resource-group myResourceGroup \
   --partition-key-path /myPartitionKey \
   --throughput 400
```

## <a name="change-the-throughput-of-a-container"></a>更改容器的吞吐量

<!-- Not Available on cloud shell-->

若要将 Cosmos DB 容器的吞吐量更改为 1000 RU/秒，请运行以下命令：

```azurecli
# Update container throughput
az cosmosdb collection update \
   --collection-name myContainer \
   --name mycosmosdbaccount \
   --db-name myDatabase \
   --resource-group myResourceGroup \
   --throughput 1000
```

## <a name="list-account-keys"></a>列出帐户密钥

若要获取 Cosmos 帐户的密钥，请运行以下命令：

```azurecli
# List account keys
az cosmosdb keys list \
   --name  mycosmosdbaccount \
   --resource-group myResourceGroup
```

## <a name="list-connection-strings"></a>列出连接字符串

若要获取 Cosmos 帐户的连接字符串，请运行以下命令：

```azurecli
# List connection strings
az cosmosdb list-connection-strings \
   --name mycosmosdbaccount \
   --resource-group myResourceGroup
```

## <a name="regenerate-account-key"></a>重新生成帐户密钥

若要重新生成 Cosmos 帐户的新主密钥，请运行以下命令：

```azurecli
# Regenerate account key
az cosmosdb regenerate-key \
   --name mycosmosdbaccount \
   --resource-group myResourceGroup \
   --key-kind primary
```

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅：

- [安装 Azure CLI](https://docs.azure.cn/zh-cn/cli/install-azure-cli?view=azure-cli-latest)
- [Azure CLI 参考](https://docs.azure.cn/zh-cn/cli/cosmosdb?view=azure-cli-latest)
- [针对 Azure Cosmos DB 的其他 Azure CLI 示例](cli-samples.md)

<!-- Update_Description: update meta properties, wording update -->
