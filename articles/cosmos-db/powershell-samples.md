---
title: 适用于 Azure Cosmos DB 的 Azure PowerShell 示例 | Azure
description: Azure PowerShell 示例 - 可用于创建和管理 Azure Cosmos DB 帐户的脚本。
services: cosmos-db
author: rockboyfor
manager: digimobile
tags: azure-service-management
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: na
ms.topic: sample
origin.date: 10/16/2017
ms.date: 09/03/2018
ms.author: v-yeche
ms.openlocfilehash: e10f966a4cea05b71eb0fc09e7b3f5773f7df8c7
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52655937"
---
# <a name="azure-powershell-samples-for-azure-cosmos-db"></a>适用于 Azure Cosmos DB 的 Azure PowerShell 示例

下表包含指向适用于 Azure Cosmos DB 的示例 Azure PowerShell 脚本的链接。 目前只能通过 PowerShell 管理 Azure Cosmos DB 帐户；不能通过 PowerShell 管理数据库和容器等其他资源。

| |  |
|---|---|
|创建 Azure Cosmos DB 帐户||
|[创建 SQL API 帐户](scripts/create-database-account-powershell.md)| 创建单个要用于 SQL API 的 Azure Cosmos DB 帐户。 |
|[创建 MongoDB API 帐户](scripts/create-mongodb-database-account-powershell.md)| 创建单个要用于 MongoDB API 的 Azure Cosmos DB 帐户。 |
|缩放 Azure Cosmos DB||
|[将 Azure Cosmos DB 帐户复制到多个区域中并配置故障转移优先级](scripts/scale-multiregion-powershell.md)|多数据中心将帐户数据复制到具有指定故障转移优先级的多个区域中。|
|**保护 Azure Cosmos DB**||
| [获取帐户密钥](scripts/secure-get-account-key-powershell.md) | 获取帐户的主要和辅助主写入密钥以及主要和辅助只读密钥。|
| [获取 MongoDB 连接字符串](scripts/secure-mongo-connection-string-powershell.md) | 获取用于将 MongoDB 应用连接到 Azure Cosmos DB 帐户的连接字符串。|
|[重新生成帐户密钥](scripts/secure-regenerate-key-powershell.md)|重新生成帐户的主密钥或只读密钥。|
|[创建防火墙](scripts/create-firewall-powershell.md)| 创建入站 IP 访问控制策略，仅允许从获批准的一组计算机和/或云服务访问帐户。|
|**高可用性、灾难恢复、备份和还原**||
|[配置故障转移策略](scripts/ha-failover-policy-powershell.md)|为在其中复制帐户的每个区域设置故障转移优先级。|
|||
<!-- Notice: 全球范围 to 多个数据中心范围 [将 Azure Cosmos DB 帐户复制到多个区域中并配置故障转移优先级] -->

<!--Not Available for external TOC file ?toc=%2fpowershell%2fmodule%2ftoc.json -->
<!--Update_Description: update meta properties -->