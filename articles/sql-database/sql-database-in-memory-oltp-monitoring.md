---
title: "监视 XTP 内存中存储 | Azure"
description: "估算和监视 XTP 内存中存储用量与容量；解决容量错误 41823"
services: sql-database
documentationcenter: 
author: Hayley244
manager: digimobile
editor: 
ms.assetid: b617308e-692c-4938-8fa2-070034a3ecef
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 12/19/2016
ms.date: 07/03/2017
ms.author: v-johch
ms.openlocfilehash: 85b42650b5f7af5fbff431583449ccb36120a550
ms.sourcegitcommit: f119d4ef8ad3f5d7175261552ce4ca7e2231bc7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/30/2017
---
# 监视 In-Memory OLTP 存储
<a id="monitor-in-memory-oltp-storage" class="xliff"></a>
使用[内存中 OLTP](sql-database-in-memory.md) 时，内存优化表中的数据和表变量将驻留在内存中 OLTP 存储内。 每个高级服务层都有一个最大内存中 OLTP 存储大小限制，如 [SQL 数据库服务层](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels)一文中所述。 超过此限制后，插入和更新操作可能会开始失败（错误 41823）。 到时，你需要删除数据以回收内存，或升级数据库的性能层。

## 确定数据是否在内存中存储容量限制范围内
<a id="determine-whether-data-will-fit-within-the-in-memory-storage-cap" class="xliff"></a>
确定存储上限：有关不同高级服务层的存储上限的信息，请参阅 [SQL 数据库服务层](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels)一文。

估计内存优化表的内存要求，如同在 Azure SQL 数据库中估计 SQL Server 的内存要求一样。 花几分钟时间查看 [MSDN](https://msdn.microsoft.com/library/dn282389.aspx) 上的相关主题。

请注意，表和表变量行以及索引都将计入最大用户数据大小。 此外，ALTER TABLE 需要足够的空间来创建新版的完整表及其索引。

## 监视和警报
<a id="monitoring-and-alerting" class="xliff"></a>
可以在 Azure [门户](https://portal.azure.cn/)中，通过[性能层的存储上限](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels)百分比来监视内存中存储使用情况： 

* 在“数据库”边栏选项卡上，找出“资源使用率”框并单击“编辑”。
* 然后选择指标 `In-Memory OLTP Storage percentage`。
* 若要添加警报，请单击“资源使用率”框以打开“度量值”边栏选项卡，然后单击“添加警报”。

或者，使用以下查询来显示内存中存储的使用率：

```
SELECT xtp_storage_percent FROM sys.dm_db_resource_stats
```

## 更正内存不足情况 - 错误 41823
<a id="correct-out-of-memory-situations---error-41823" class="xliff"></a>
内存不足会导致插入、更新和创建操作失败，并出现错误消息 41823。

错误消息 41823 指示内存优化表和表变量已超过大小上限。

若要解决此错误，请执行以下操作之一：

* 从内存优化表中删除数据，为此，可以将数据卸载到传统的基于磁盘的表；或者，
* 将服务层升级到具有足够内存中存储的服务层，使保存你需要保留在内存优化表中的数据。

## 后续步骤
<a id="next-steps" class="xliff"></a>
有关监视指南，请参阅[使用动态管理视图监视 Azure SQL 数据库](sql-database-monitoring-with-dmvs.md)。