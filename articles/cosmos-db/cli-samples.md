---
title: 用于 Azure Cosmos DB 的 Azure CLI 示例
description: Azure CLI 示例 - 创建和管理 Azure Cosmos DB 帐户、数据库、容器、区域和防火墙。
author: rockboyfor
ms.service: cosmos-db
ms.topic: sample
origin.date: 10/26/2018
ms.date: 01/07/2019
ms.author: v-yeche
ms.openlocfilehash: 2ba5fe24b53bd8df51c1584e41b3e69b11aac1f4
ms.sourcegitcommit: ce4b37e31d0965e78b82335c9a0537f26e7d54cb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/04/2019
ms.locfileid: "54026758"
---
# <a name="azure-cli-samples-for-azure-cosmos-db"></a>用于 Azure Cosmos DB 的 Azure CLI 示例

下表包括用于 Azure Cosmos DB 的示例 Azure CLI 脚本的链接。 [Azure CLI 参考](https://docs.azure.cn/zh-cn/cli/cosmosdb?view=azure-cli-latest)中收录了所有 Azure Cosmos DB CLI 命令的参考页。

| |  |
|---|---|
|**创建 Azure Cosmos DB 帐户、数据库和容器**||
| [使用 SQL API 创建 Azure Cosmos DB 帐户](scripts/create-database-account-collections-cli.md?toc=%2fcli%2ftoc.json)| 创建单个 Azure Cosmos DB 帐户、数据库和容器。 |
| [使用 Cosmos DB 的用于 MongoDB 的 API 创建 Azure Cosmos DB 帐户](scripts/create-mongodb-database-account-cli.md?toc=%2fcli%2ftoc.json) | 创建单个 Azure Cosmos DB 帐户、数据库和集合。 |
|缩放 Azure Cosmos DB||
| [缩放容器吞吐量](scripts/scale-collection-throughput-cli.md?toc=%2fcli%2ftoc.json) | 更改容器上预配的吞吐量。|
| [将 Azure Cosmos DB 数据库帐户复制到多个区域中并配置故障转移优先级](scripts/scale-multiregion-cli.md?toc=%2fcli%2ftoc.json)|多数据中心将帐户数据复制到具有指定故障转移优先级的多个区域中。|
|**保护 Azure Cosmos DB**||
| [获取帐户密钥](scripts/secure-get-account-key-cli.md?toc=%2fcli%2ftoc.json) | 获取帐户的主要和辅助主写入密钥以及主要和辅助只读密钥。|
| [获取使用 Azure Cosmos DB 的用于 MongoDB 的 API 配置的 Cosmos 帐户的连接字符串](scripts/secure-mongo-connection-string-cli.md?toc=%2fcli%2ftoc.json) | 获取用于将 MongoDB 应用连接到 Azure Cosmos DB 帐户的连接字符串。|
| [重新生成帐户密钥](scripts/secure-regenerate-key-cli.md?toc=%2fcli%2ftoc.json)|重新生成帐户密钥。|
| [创建防火墙](scripts/create-firewall-cli.md?toc=%2fcli%2ftoc.json)| 创建入站 IP 访问控制策略，仅允许从获批准的一组计算机和/或云服务访问帐户。|
|**高可用性、灾难恢复、备份和还原**||
| [配置故障转移策略](scripts/ha-failover-policy-cli.md?toc=%2fcli%2ftoc.json)|为帐户所复制的每个区域设置故障转移优先级。|
|**将 Azure Cosmos DB 连接到资源**||
| [将 Web 应用连接到 Azure Cosmos DB](../app-service/scripts/app-service-cli-app-service-documentdb.md?toc=%2fcli%2ftoc.json)|创建并连接 Azure Cosmos DB 数据库和 Azure Web 应用。|
|||

<!--Not Available on [Create a Gremlin API account](scripts/create-graph-database-account-cli.md)-->
<!--Not Available on [Create a Cassandra API account](scripts/create-cassandra-database-account-cli.md)-->
<!--Not Available on [Create a Table API account](scripts/create-table-database-account-cli.md)-->
<!--NOTICES: Line 35 全球范围 to 多个数据中心范围  -->
<!--NOTICE: Line 35 Globally TO Multiple data-center-->

<!--Update_Description: update meta properties, wording update -->