---
title: "用于 Azure Cosmos DB 的 Azure CLI 示例 | Azure"
description: "Azure CLI 示例 - 创建和管理 Azure Cosmos DB 帐户、数据库、容器、区域和防火墙。"
services: cosmos-db
author: rockboyfor
manager: digimobile
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: database
origin.date: 06/07/2017
ms.date: 08/07/2017
ms.author: v-yeche
ms.openlocfilehash: bdcef97602b9097e6c8ccaa839cd8ded622481df
ms.sourcegitcommit: 5939c7db1252c1340f06bdce9ca2b079c0ab1684
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/04/2017
---
# <a name="azure-cli-samples-for-azure-cosmos-db"></a>用于 Azure Cosmos DB 的 Azure CLI 示例

下表包括用于 Azure Cosmos DB 的示例 Azure CLI 脚本的链接。 所有 Azure Cosmos DB CLI 命令参考页可在 [Azure CLI 2.0 参考](https://docs.microsoft.com/cli/azure/cosmosdb)中找到。

| |  |
|---|---|
|**创建 Azure Cosmos DB 帐户、数据库和容器**||
|[创建 DocumentDB 或表 API 帐户](scripts/create-database-account-collections-cli.md)| 创建单个 Azure Cosmos DB API 帐户、数据库和容器，以便与 DocumentDB 或表 API 配合使用。 |
| [创建 MongoDB API 帐户](scripts/create-mongodb-database-account-cli.md) | 创建单个 Azure Cosmos DB MongoDB API 帐户、数据库和集合。 |
|**缩放 Azure Cosmos DB**||
| [缩放容器吞吐量](scripts/scale-collection-throughput-cli.md) | 更改容器上预配的吞吐量。|
|[将 Azure Cosmos DB 数据库帐户复制到多个区域中并配置故障转移优先级](scripts/scale-multiregion-cli.md)|在全球范围内将帐户数据复制到具有指定故障转移优先级的多个区域中。|
|**保护 Azure Cosmos DB**||
| [获取帐户密钥](scripts/secure-get-account-key-cli.md) | 获取帐户的主要和辅助主写入密钥以及主要和辅助只读密钥。|
| [获取 MongoDB 连接字符串](scripts/secure-mongo-connection-string-cli.md) | 获取用于将 MongoDB 应用连接到 Azure Cosmos DB 帐户的连接字符串。|
|[重新生成帐户密钥](scripts/secure-regenerate-key-cli.md)|重新生成帐户的主密钥或只读密钥。|
|[创建防火墙](scripts/create-firewall-cli.md)| 创建入站 IP 访问控制策略，仅允许从获批准的一组计算机和/或云服务访问帐户。|
|**高可用性、灾难恢复、备份和还原**||
|[配置故障转移策略](scripts/ha-failover-policy-cli.md)|为在其中复制帐户的每个区域设置故障转移优先级。|
|**将 Azure Cosmos DB 连接到资源**||
|[将 Web 应用连接到 Azure Cosmos DB](/app-service-web/scripts/app-service-cli-app-service-documentdb?toc=%2fcli%2fazure%2ftoc.json)|创建并连接 Azure Cosmos DB 数据库和 Azure Web 应用。|
|||

<!-- Not Available ?toc=%2fcli%2fazure%2ftoc.json in docs.microsoft.com website -->

<!--Update_Description: update meta properties-->