---
title: 使用 Azure CLI 管理 Azure Cosmos DB 资源
description: 使用 Azure CLI 管理 Azure Cosmos DB 帐户、数据库和容器。
services: cosmos-db
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 10/23/2018
ms.date: 01/07/2019
ms.author: v-yeche
ms.openlocfilehash: 55987eb9f90c3ed5fbfcf649cd024f41fe346fe0
ms.sourcegitcommit: ce4b37e31d0965e78b82335c9a0537f26e7d54cb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/04/2019
ms.locfileid: "54026808"
---
# <a name="manage-azure-cosmos-db-resources-using-azure-cli"></a>使用 Azure CLI 管理 Azure Cosmos DB 资源

以下指南介绍了使用 Azure CLI 自动管理 Azure Cosmos DB 帐户、数据库和容器的命令。 它还包括用来缩放容器吞吐量的命令。 [Azure CLI 参考](https://docs.azure.cn/zh-cn/cli/cosmosdb?view=azure-cli-latest)中收录了所有 Azure Cosmos DB CLI 命令的参考页。 还可以在[针对 Azure Cosmos DB 的 Azure CLI 示例](cli-samples.md)中找到更多示例，包括如何为 MongoDB 创建和管理 Cosmos DB 帐户、数据库和容器。

<!-- Not Available on  Gremlin, Cassandra and Table API--> 此示例 CLI 脚本创建 Azure Cosmos DB SQL API 帐户、数据库和容器。  

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

如果选择在本地安装并使用 CLI，本主题要求运行 Azure CLI 2.0 版或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI](https://docs.azure.cn/zh-cn/cli/install-azure-cli?view=azure-cli-latest)。

## <a name="create-an-azure-cosmos-db-account"></a>创建 Azure Cosmos DB 帐户

若要在“中国东部”和“中国北部”创建启用了 SQL API、会话一致性和多主数据库的 Azure Cosmos DB 帐户，请打开 Azure CLI 并运行以下命令：

<!-- Not Available on cloud shell-->
```azurecli
az cosmosdb create \
   --name "myCosmosDbAccount" \
   --resource-group "myResourceGroup" \
   --kind GlobalDocumentDB \
   --default-consistency-level "Session" \
   --locations "ChinaEast=0" "ChinaNorth=1" \
   --enable-multiple-write-locations true \
```

## <a name="create-a-database"></a>创建数据库

若要创建 Cosmos DB 数据库，请打开 Azure CLI 并运行以下命令：

<!-- Not Available on cloud shell-->
```azurecli
az cosmosdb database create \
   --name "myCosmosDbAccount" \
   --db-name "myDatabase" \
   --resource-group "myResourceGroup"
```

## <a name="create-a-container"></a>创建容器

若要创建吞吐量为 1000 RU/秒且具有分区键的 Cosmos DB 容器，请打开 Azure CLI 并运行以下命令：

<!-- Not Available on cloud shell-->
```azurecli
# Create a container
az cosmosdb collection create \
   --collection-name "myContainer" \
   --name "myCosmosDbAccount" \
   --db-name "myDatabase" \
   --resource-group "myResourceGroup" \
   --partition-key-path = "/myPartitionKey" \
   --throughput 1000
```

## <a name="change-the-throughput-of-a-container"></a>更改容器的吞吐量

<!-- Not Available on cloud shell--> 若要将 Cosmos DB 容器的吞吐量更改为 400 RU/秒，请打开 Azure CLI 并运行以下命令：

```azurecli
# Update container throughput
az cosmosdb collection update \
   --collection-name "myContainer" \
   --name "myCosmosDbAccount" \
   --db-name "myDatabase" \
   --resource-group "myResourceGroup" \
   --throughput 400
```

## <a name="list-account-keys"></a>列出帐户密钥

创建 Azure Cosmos DB 帐户时，该服务会生成两个主访问密钥，用于访问 Azure Cosmos DB 帐户时的身份验证。 Azure Cosmos DB 提供有两个访问密钥，因此可在不中断 Azure Cosmos DB 帐户连接的情况下重新生成密钥。 还提供用于对只读操作进行身份验证的只读密钥。 有两个读写密钥（主密钥和辅助密钥）和两个只读密钥（主密钥和辅助密钥）。 可以通过运行以下命令来获取帐户的密钥：

```azurecli
# List account keys
az cosmosdb list-keys \
   --name "myCosmosDbAccount"\
   --resource-group "myResourceGroup"
```

## <a name="list-connection-strings"></a>列出连接字符串

可以使用以下命令检索用于将应用连接到 Cosmos DB 帐户的连接字符串。

```azurecli
# List connection strings
az cosmosdb list-connection-strings \
   --name "myCosmosDbAccount"\
   --resource-group "myResourceGroup"
```

## <a name="regenerate-account-key"></a>重新生成帐户密钥

应定期更改 Azure Cosmos DB 帐户的访问密钥，加强连接的安全性。 为分配两个访问密钥是为了让你使用一个访问密钥保持与 Azure Cosmos DB 帐户的连接，同时可以重新生成另一个访问密钥。

```azurecli
# Regenerate account key
az cosmosdb regenerate-key \
   --name "myCosmosDbAccount"\
   --resource-group "myResourceGroup" \
   --key-kind primary
```

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅：

- [安装 Azure CLI](https://docs.azure.cn/zh-cn/cli/install-azure-cli?view=azure-cli-latest)
- [Azure CLI 参考](https://docs.azure.cn/zh-cn/cli/cosmosdb?view=azure-cli-latest)
- [针对 Azure Cosmos DB 的其他 Azure CLI 示例](cli-samples.md)

<!-- Update_Description: update meta properties, wording update -->
