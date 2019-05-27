---
title: Azure Cosmos DB 的 Azure 资源管理器模板
description: 使用 Azure 资源管理器模板创建和配置 Azure Cosmos DB。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 05/06/2019
ms.date: 05/13/2019
ms.author: v-yeche
ms.openlocfilehash: e416e8e21f9cd3016c9b35a1c37c5bd0478f40ca
ms.sourcegitcommit: 71172ca8af82d93d3da548222fbc82ed596d6256
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2019
ms.locfileid: "65669040"
---
<!--Verify successfully-->
# <a name="azure-resource-manager-templates-for-azure-cosmos-db"></a>Azure Cosmos DB 的 Azure 资源管理器模板

下表包含 Azure Cosmos DB 的 Azure 资源管理器模板链接。

|**API 类型** | **示例链接**| **说明** |
|---|---| ---|
| Core (SQL) API| [创建 Azure Cosmos DB 帐户（多主数据库）](manage-sql-with-resource-manager.md) | 此模板在两个区域创建一个启用了多主数据库的 SQL API 帐户。 Azure Cosmos 帐户会有两个共享数据库级吞吐量的容器。 |
| MongoDB API | [创建 Azure Cosmos DB 帐户（多主数据库）](manage-mongodb-with-resource-manager.md) | 此模板使用 Azure Cosmos DB 的 API for MongoDB 在两个区域创建一个启用了多主数据库的帐户。 Azure Cosmos 帐户会有两个共享数据库级吞吐量的容器。 |
| Cassandra API | [创建 Azure Cosmos DB 帐户（多主数据库）](manage-cassandra-with-resource-manager.md) | 此模板在两个区域创建一个启用了多主数据库的 Cassandra API 帐户。 Azure Cosmos 帐户会有两个共享密钥空间级吞吐量的表。 |
| Gremlin API| [创建 Azure Cosmos DB 帐户（多主数据库）](manage-gremlin-with-resource-manager.md) | 此模板在两个区域创建一个启用了多主数据库的 Gremlin API 帐户。 Azure Cosmos 帐户会有两个共享数据库级吞吐量的图形。 |
| 表 API | [创建 Azure Cosmos DB 帐户（多主数据库）](manage-table-with-resource-manager.md) | 此模板在两个区域创建一个启用了多主数据库的表 API 帐户。 Azure Cosmos 帐户会有一个表。 |

> [!TIP]
> 若要在使用表 API 时启用共享吞吐量，请在 Azure 门户中启用帐户级吞吐量。

<!--Not Available on [ARM reference for Azure Cosmos DB](https://docs.microsoft.com/zh-cn/azure/templates/microsoft.documentdb/allversions)-->