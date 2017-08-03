---
title: "适用于 SQL 数据库的 Azure PowerShell 脚本示例 | Azure"
description: "可帮助创建并管理 Azure SQL 数据库服务器、弹性池、数据库和防火墙的 Azure PowerShell 脚本示例。"
services: sql-database
documentationcenter: sql-database
author: Hayley244
manager: digimobile
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: sample
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: sql-database
ms.workload: database
origin.date: 06/23/2017
ms.date: 07/31/2017
ms.author: v-haiqya
ms.openlocfilehash: 56ced00c8e5800264dff0735da1971c62720f3a1
ms.sourcegitcommit: 2e85ecef03893abe8d3536dc390b187ddf40421f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2017
---
# <a name="azure-powershell-samples-for-azure-sql-database"></a>适用于 Azure SQL 数据库的 Azure PowerShell 示例

下表包括了适用于 Azure SQL 数据库的示例 Azure PowerShell 脚本的链接。

| |  |
|---|---|
|**创建单一数据库和弹性池**||
| [创建单一数据库和配置防火墙规则](scripts/sql-database-create-and-configure-database-powershell.md) | 此 PowerShell 脚本创建单一 Azure SQL 数据库，并配置服务器级防火墙规则。 |
| [创建弹性池并移动入池数据库](scripts/sql-database-move-database-between-pools-powershell.md) | 此 PowerShell 脚本创建 Azure SQL 数据库弹性池，移动入池数据库并更改性能级别。|
|**配置异地复制和故障转移**||
| [配置单一数据库并使用活动异地复制对其进行故障转移](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)| 此 PowerShell 脚本为单一 Azure SQL 数据库配置活动异地复制，并将其故障转移到次要副本。 |
| [配置入池数据库并使用活动异地复制对其进行故障转移](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md)| 此 PowerShell 脚本为 SQL 弹性池中的 Azure SQL 数据库配置活动异地复制，并将其故障转移到次要副本。 |
| [为单一数据库配置和故障转移故障转移组（预览版）](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md) | 此 PowerShell 脚本为 Azure SQL 数据库服务器实例配置故障转移组，将数据库添加到故障转移组，并将其故障转移到辅助服务器 |
|**缩放单一数据库和弹性池**||
| [缩放单一数据库](scripts/sql-database-monitor-and-scale-database-powershell.md) | 此 PowerShell 脚本监视 Azure SQL 数据库的性能指标，将其缩放为更高的性能级别，并基于性能指标之一创建预警规则。 |
| [缩放弹性池](scripts/sql-database-monitor-and-scale-pool-powershell.md) | 此 PowerShell 脚本监视 Azure SQL 数据库弹性池的性能指标，将其缩放为更高的性能级别，并基于性能指标之一创建预警规则。  |
| **审核和威胁检测** |
| [配置审核和威胁检测](scripts/sql-database-auditing-and-threat-detection-powershell.md)| 此 PowerShell 脚本为 Azure SQL 数据库配置审核和威胁检测策略。 |
| **还原、复制和导入数据库**||
| [还原数据库](scripts/sql-database-restore-database-powershell.md)| 此 PowerShell 脚本从异地冗余备份还原 Azure SQL 数据库，并根据最新备份还原已删除的 Azure SQL 数据库。 |
| [将数据库复制到新服务器](scripts/sql-database-copy-database-to-new-server-powershell.md)| 此 PowerShell 脚本在新的 Azure SQL 服务器中创建现有 Azure SQL 数据库的副本。 |
| [从 bacpac 文件导入数据库](scripts/sql-database-import-from-bacpac-powershell.md)| 此 PowerShell 脚本将数据库从 bacpac 文件导入到 Azure SQL 服务器。 |
|||

<!--Update_Description: wording update-->