---
title: "在 Azure SQL 数据库中监视数据库性能 | Azure"
description: "了解使用 Azure 工具和动态管理视图监视数据库时可用的选项。"
keywords: "数据库监视,云数据库性能"
services: sql-database
documentationcenter: 
author: Hayley244
manager: digimobile
editor: 
ms.assetid: a2e47475-c955-4a8d-a65c-cbef9a6d9b9f
ms.service: sql-database
ms.custom: monitor and tune
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
origin.date: 01/10/2017
ms.date: 07/03/2017
ms.author: v-johch
ms.openlocfilehash: d5f72ea1fef01add2452366d07bebdd960593f02
ms.sourcegitcommit: 73b1d0f7686dea85647ef194111528c83dbec03b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/28/2017
---
# 在 Azure SQL 数据库中监视数据库性能
<a id="monitoring-database-performance-in-azure-sql-database" class="xliff"></a>
若要监视 Azure 中的 SQL 数据库的性能，首先需要监视所选数据库性能级别相关的资源利用率。 监视功能可帮助你确定数据库是否超出容量，或者因资源超限而遇到问题，然后确定是否有必要调整数据库的性能级别和[服务层](sql-database-service-tiers.md)。 可以使用 [Azure 门户](https://portal.azure.cn)中的图形工具或使用 SQL [动态管理视图](https://msdn.microsoft.com/library/ms188754.aspx)来监视数据库。

## 使用 Azure 门户监视数据库
<a id="monitor-databases-using-the-azure-portal" class="xliff"></a>
在 [Azure 门户](https://portal.azure.cn)中，可以通过选择数据库并单击“监视”图表来监视单一数据库的利用率。 这将显示“指标”窗口，可通过单击“编辑图表”按钮来对其进行更改。 添加以下指标：

* CPU 百分比
* DTU 百分比
* 数据 IO 百分比
* 数据库大小百分比

添加这些指标后，可以继续在“监视”图表上查看它们，并可在“指标”窗口上查看更多详细信息。 **DTU** 的平均利用率百分比。 请参阅[服务层](sql-database-service-tiers.md)一文，了解有关 DTU 的详细信息。

![在服务层监视数据库性能。](./media/sql-database-service-tiers/sqldb_service_tier_monitoring.png)

还可针对性能指标配置警报。 在“指标”窗口中单击“添加警报”按钮。 按照向导说明来配置警报。 可选择在指标超出或低于特定阈值时显示警报。

例如，如果期望数据库上的工作负荷增长，可选择配置在数据库的任意性能指标达到 80% 时发出电子邮件警报。 可将此警报用作预警，来确定何时需要切换到高一级的性能级别。

性能指标还可以帮助确定能否降级到更低性能级别。 假定正在使用标准 S2 数据库，所有性能指标均显示该数据库在任意给定时间的平均使用率都不超过 10%。 采用标准 S1 很可能使该数据库正常工作。 但是，在决定转换到更低的性能级别之前，请注意出现峰值或波动情况的工作负荷。

## 使用 DMV 监视数据库
<a id="monitor-databases-using-dmvs" class="xliff"></a>
在门户中公开的相同指标也可以通过系统视图查看：服务器的逻辑 [master](https://msdn.microsoft.com/library/dn269979.aspx) 数据库中的 **sys.resource_stats**，以及用户数据库中的 [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx)。 如果需要在更长时间段内监视更粗粒度的数据，请使用 **sys.resource_stats**。 如果需要在较小的时间范围内监视更细粒度的数据，请使用 **sys.dm_db_resource_stats**。 有关详细信息，请参阅 [Azure SQL 数据库性能指南](sql-database-single-database-monitor.md#monitor-resource-use)。

> [!NOTE]
> 如果在已停用的 Web 和 Business Edition 数据库中使用，**sys.dm_db_resource_stats** 将返回空结果集。

对于弹性池，可以使用本部分中所述的技术来监视池中的单个数据库。 但也可总体监视该池。 有关信息，请参阅[监视和管理弹性池](sql-database-elastic-pool-manage-portal.md)。