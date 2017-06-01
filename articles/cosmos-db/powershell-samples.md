---
title: "适用于 Azure Cosmos DB 的 Azure PowerShell 示例 | Azure"
description: "Azure PowerShell 示例 - 可用于创建和管理 Azure Cosmos DB 帐户的脚本。"
services: cosmosdb
author: mimig1
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: cosmosdb
ms.custom: sample
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: database
ms.date: 05/10/2017
wacn.date: 
ms.author: mimig
ms.translationtype: Human Translation
ms.sourcegitcommit: 4a18b6116e37e365e2d4c4e2d144d7588310292e
ms.openlocfilehash: fecba57844d7ffbbdb55e445dc2fa22c1b4ba71a
ms.contentlocale: zh-cn
ms.lasthandoff: 05/19/2017

---

# <a name="azure-powershell-samples-for-azure-cosmos-db"></a>适用于 Azure Cosmos DB 的 Azure PowerShell 示例

下表包含指向适用于 Azure Cosmos DB 的示例 Azure PowerShell 脚本的链接。

| |  |
|---|---|
|创建 Azure Cosmos DB 帐户||
|[创建 DocumentDB API 帐户](scripts/create-database-account-powershell.md)| 创建单个要用于 DocumentDB API 的 Azure Cosmos DB 帐户。 |
|缩放 Azure Cosmos DB||
|[将 Azure Cosmos DB 帐户复制到多个区域中并配置故障转移优先级](scripts/scale-multiregion-powershell.md)|在全局范围内将帐户数据复制到具有指定故障转移优先级的多个区域中。|
|保护 Azure Cosmos DB||
| [获取帐户密钥](scripts/secure-get-account-key-powershell.md) | 获取帐户的主要和辅助主写入密钥以及主要和辅助只读密钥。|
| [获取 MongoDB 连接字符串](scripts/secure-mongo-connection-string-powershell.md) | 获取用于将 MongoDB 应用连接到 Azure Cosmos DB 帐户的连接字符串。|
|[重新生成帐户密钥](scripts/secure-regenerate-key-powershell.md)|重新生成帐户的主密钥或只读密钥。|
|[创建防火墙](scripts/create-firewall-powershell.md)| 创建入站 IP 访问控制策略，仅允许从获批准的一组计算机和/或云服务访问帐户。|
|高可用性、灾难恢复、备份和还原||
|[配置故障转移策略](scripts/ha-failover-policy-powershell.md)|为在其中复制帐户的每个区域设置故障转移优先级。|
|||
