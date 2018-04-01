---
title: 用于 Azure Cosmos DB 的 Azure CLI 示例 | Azure
description: Azure CLI 示例 - 创建和管理 Azure Cosmos DB 帐户、数据库、容器、区域和防火墙。
services: cosmos-db
author: rockboyfor
manager: digimobile
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: database
origin.date: 11/29/2017
ms.date: 03/26/2018
ms.author: v-yeche
ms.openlocfilehash: 28ec419689d2a25ea10797f2959a3e8e97c717c5
ms.sourcegitcommit: 6d7f98c83372c978ac4030d3935c9829d6415bf4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="azure-cli-samples-for-azure-cosmos-db"></a>用于 Azure Cosmos DB 的 Azure CLI 示例

[!INCLUDE [cosmos-db-sql-api](../../includes/cosmos-db-sql-api.md)] 

下表包括用于 Azure Cosmos DB 的示例 Azure CLI 脚本的链接。 所有 Azure Cosmos DB CLI 命令参考页可在 [Azure CLI 2.0 参考](https://docs.azure.cn/zh-cn/cli/cosmosdb?view=azure-cli-latest)中找到。

| |  |
|---|---|
|**创建 Azure Cosmos DB 帐户、数据库和容器**||
|[创建 SQL API 帐户](scripts/create-database-account-collections-cli.md)| 创建单个 Azure Cosmos DB API 帐户、数据库及容器，以用于 SQL API。 |
| [创建 MongoDB API 帐户](scripts/create-mongodb-database-account-cli.md) | 创建单个 Azure Cosmos DB MongoDB API 帐户、数据库和集合。 |
|**缩放 Azure Cosmos DB**||
| [缩放容器吞吐量](scripts/scale-collection-throughput-cli.md) | 更改容器上预配的吞吐量。|
|[将 Azure Cosmos DB 数据库帐户复制到多个区域中并配置故障转移优先级](scripts/scale-multiregion-cli.md)|多数据中心将帐户数据复制到具有指定故障转移优先级的多个区域中。|
|**保护 Azure Cosmos DB**||
| [获取帐户密钥](scripts/secure-get-account-key-cli.md) | 获取帐户的主要和辅助主写入密钥以及主要和辅助只读密钥。|
| [获取 MongoDB 连接字符串](scripts/secure-mongo-connection-string-cli.md) | 获取用于将 MongoDB 应用连接到 Azure Cosmos DB 帐户的连接字符串。|
|[重新生成帐户密钥](scripts/secure-regenerate-key-cli.md)|重新生成帐户的主密钥或只读密钥。|
|[创建防火墙](scripts/create-firewall-cli.md)| 创建入站 IP 访问控制策略，仅允许从获批准的一组计算机和/或云服务访问帐户。|
|**高可用性、灾难恢复、备份和还原**||
|[配置故障转移策略](scripts/ha-failover-policy-cli.md)|为帐户所复制的每个区域设置故障转移优先级。|
|**将 Azure Cosmos DB 连接到资源**||
|[将 Web 应用连接到 Azure Cosmos DB](../app-service/scripts/app-service-cli-app-service-documentdb.md)|创建并连接 Azure Cosmos DB 数据库和 Azure Web 应用。|
|||
<!--NOTICES: Line 35 全球范围 to 多个数据中心范围  -->
<!--NOTICE: Line 35 Globally TO Multiple data-center-->


<!--Update_Description: update meta properties, wording update, update link -->