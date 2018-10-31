---
title: 监视和性能优化 - Azure SQL 数据库 | Microsoft Docs
description: 有关通过评估和改进来调整 Azure SQL 数据库性能的提示。
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: WenJason
ms.author: v-jay
ms.reviewer: carlrab
manager: digimobile
origin.date: 10/15/2018
ms.date: 10/29/2018
ms.openlocfilehash: a31c13349e8d67907c6d2ebf3e8a52f24f30a4cb
ms.sourcegitcommit: b8f95f5d6058b1ac1ce28aafea3f82b9a1e9ae24
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/26/2018
ms.locfileid: "50135872"
---
# <a name="monitoring-and-performance-tuning"></a>监视和性能优化

Azure SQL 数据库由系统自动管理，它是一个灵活的数据服务，可在其中轻松监视使用情况、添加或删除资源（CPU、内存、I/O）、查找可以提高数据库性能的建议，或者让数据库适应你的工作负荷并自动优化性能。

## <a name="the-state-of-an-active-query"></a>活动查询的状态

若要提高 Azure SQL 数据库的性能，请了解来自应用程序的每个活动查询请求是处于运行状态还是等待状态。 在 Azure SQL 数据库中排查性能问题时，请注意以下图表：

![工作负荷状态](./media/sql-database-monitor-tune-overview/workload-states.png)

对于出现性能问题的工作负荷，问题的原因可能是 CPU 争用（**运行相关的**条件）或单个查询正在等待某个资源（**等待相关的**条件）。

- **Azure SQL 数据库中的 CPU 利用率过高**：

  在以下情况下，过高的 CPU 利用率可能会导致性能问题：

  - 运行查询过多
  - 编译查询过多
  - 一个或多个执行查询使用了欠佳的查询计划

  如果工作负荷出现此情况，则你的目标应该是识别并优化相关的查询，或者升级计算大小或服务层，以增加 Azure SQL 数据库的容量来满足 CPU 要求。 有关缩放单一数据库资源的详细信息，请参阅[缩放 Azure SQL 数据库中的单一数据库资源](sql-database-single-database-scale.md)；有关缩放弹性池资源的信息，请参阅[缩放 Azure SQL 数据库中的弹性池资源](sql-database-elastic-pool-scale.md)。

- **单个查询正在等待某个资源**

  单个查询可能会由于需要等待某个资源而出现性能问题。 在这种情况下，目标应该是消除或减少等待时间。

### <a name="determine-if-you-have-a-running-related-performance-issue"></a>确定是否出现运行相关的性能问题

可以使用多种方法来识别运行相关的性能问题。 最常见的方法包括：

- 使用 [Azure 门户](#monitor-databases-using-the-azure-portal)监视 CPU 利用率百分比。
- 使用以下[动态管理视图](sql-database-monitoring-with-dmvs.md)：

  - [sys.dm_db_resource_stats](sql-database-monitoring-with-dmvs.md#monitor-resource-use) 返回 Azure SQL 数据库中数据库的 CPU、I/O 和内存消耗量。 即使数据库中没有活动，也会每隔 15 秒返回一行数据。 历史数据将保留一小时。
  - [sys.resource_stats](sql-database-monitoring-with-dmvs.md#monitor-resource-use) 返回 Azure SQL 数据库的 CPU 使用率和存储数据。 在五分钟间隔内收集并聚合数据。

> [!TIP]
> 一般原则是，如果 CPU 利用率一贯达到或超过 80%，则表示出现了运行相关的性能问题。

### <a name="determine-if-you-have-a-waiting-related-performance-issue"></a>确定是否出现等待相关的性能问题

首先，确定这不是一个 CPU 消耗量较高的运行相关性能问题。 如果不是，则下一步是识别与应用程序工作负荷关联的最常见等待类型。  用于显示最常见等待类型类别的常用方法：

- [查询存储](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store)提供每个查询在不同时间段的等待统计信息。 在查询存储中，等待类型合并成等待类别。 等待类别到等待类型的映射在 [sys.query_store_wait_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql.md#wait-categories-mapping-table) 中提供。
- [sys.dm_db_wait_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-wait-stats-azure-sql-database) 返回有关操作期间执行的线程遇到的所有等待的信息。 可以使用此聚合视图来诊断 Azure SQL 数据库以及特定查询和批的性能问题。
- [sys.dm_os_waiting_tasks](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-waiting-tasks-transact-sql) 返回有关正在等待某个资源的任务的等待队列的信息。

如上面的图表中所示，是最常见的等待是：

- 锁（阻塞）
- I/O
- tempdb 相关的争用
- 内存授予等待

根据显示的内容，每个等待类别具有不同的故障排除路径。

## <a name="overview-of-monitoring-database-performance-in-azure-sql-database"></a>在 Azure SQL 数据库中监视数据库性能概述

若要监视 Azure 中的 SQL 数据库的性能，首先需要监视所选数据库性能级别相关的资源利用率。 监视功能可帮助确定数据库是否超出容量，或者因资源超限而遇到问题，并确定是否有必要调整[基于 DTU 的购买模型](sql-database-service-tiers-dtu.md)或[基于 vCore 的购买模型](sql-database-service-tiers-vcore.md)中数据库的计算大小和服务层。 可以使用以下图形工具或使用 SQL [动态管理视图 (DMV)](sql-database-monitoring-with-dmvs.md) 在 [Azure 门户](https://portal.azure.com)中监视数据库。

Azure SQL 数据库使得你可以通过查看[性能优化建议](sql-database-advisor.md)找到机会来改进和优化查询性能，无需更改资源。 缺少索引与查询优化不足是数据库性能不佳的常见原因。 可以应用这些优化建议来改进你的工作负荷的性能。
还可以通过应用所有已确定的建议并确认它们改进了数据库性能来让 Azure SQL 数据库[自动优化查询性能](sql-database-automatic-tuning.md)。 在监视数据库性能并对其进行故障排除方面，有以下选项可供选择：

- 在 [Azure 门户](https://portal.azure.cn)中，单击 **SQL 数据库**，选择该数据库，然后使用“监视”图表查找接近其上限的资源。 默认情况下将显示 DTU 消耗量。 单击“编辑”可更改所显示的时间范围和值。
- 使用 [Query Performance Insight](sql-database-query-performance.md) 可找出占用资源最多的查询。
- 使用 [SQL 数据库顾问](sql-database-advisor-portal.md)查看有关创建和删除索引、参数化查询，以及解决架构问题的建议。
- [启用自动优化](sql-database-automatic-tuning-enable.md)并让 Azure SQL 数据库自动修复查明的性能问题。
- 还可以使用[动态管理视图 (DMV)](sql-database-monitoring-with-dmvs.md)、[扩展事件 (`XEvents`)(sql-database-xevent-db-diff-from-svr.md) 和[查询存储](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store)实时获取性能参数。 若要使用这些报告或视图识别某个问题，请参阅[性能指南](sql-database-performance-guidance.md)，了解可用于提高 Azure SQL 数据库性能的方法。

## <a name="monitor-databases-using-the-azure-portal"></a>使用 Azure 门户监视数据库

在 [Azure 门户](https://portal.azure.cn/)中，可以通过选择数据库并单击“监视”图表来监视单一数据库的利用率。 这将显示“指标”窗口，可通过单击“编辑图表”按钮来对其进行更改。 添加以下指标：

- CPU 百分比
- DTU 百分比
- 数据 IO 百分比
- 数据库大小百分比

添加这些指标后，可以继续在“监视”图表上查看它们，并可在“指标”窗口上查看更多详细信息。 **DTU** 的平均利用率百分比。 有关服务层的详细信息，请参阅[基于 DTU 的购买模型](sql-database-service-tiers-dtu.md)和[基于 vCore 的购买模型](sql-database-service-tiers-vcore.md)文章。  

![在服务层监视数据库性能。](./media/sql-database-single-database-monitoring/sqldb_service_tier_monitoring.png)

还可针对性能指标配置警报。 在“指标”窗口中单击“添加警报”按钮。 按照向导说明来配置警报。 可选择在指标超出或低于特定阈值时显示警报。

例如，如果期望数据库上的工作负荷增长，可选择配置在数据库的任意性能指标达到 80% 时发出电子邮件警报。 可以将此警报用作预警，以确定你何时需要切换到下一个更高的计算大小。

性能指标还可以帮助你确定是否能够降级到更低的计算大小。 假定正在使用标准 S2 数据库，所有性能指标均显示该数据库在任意给定时间的平均使用率都不超过 10%。 采用标准 S1 很可能使该数据库正常工作。 但是，在决定转换到更低的计算大小之前，请注意出现峰值或波动情况的工作负荷。

## <a name="improving-database-performance-with-more-resources"></a>使用更多资源改进数据库性能

最后，如果没有可操作项可以改进数据库性能，你可以更改 Azure SQL 数据库中提供的资源数量。 随时可以通过更改单一数据库的 [DTU 服务层](sql-database-service-tiers-dtu.md)或者增加弹性池的 eDTU 数目，来分配更多资源。 或者，如果使用[基于 vCore 的购买模型](sql-database-service-tiers-vcore.md)，则可更改服务层或增加分配给数据库的资源。

1. 对于单一数据库，可以根据需要[更改服务层](sql-database-service-tiers-dtu.md)或[计算资源](sql-database-service-tiers-vcore.md)以提高数据库性能。
2. 对于多个数据库，请考虑使用[弹性池](sql-database-elastic-pool-guidance.md)自动调整资源规模。

## <a name="tune-and-refactor-application-or-database-code"></a>优化和重构应用程序或数据库代码

你可以更改应用程序代码来以更佳方式使用数据库，更改索引，强制实施计划或使用提示来手动使数据库适合你的工作负荷。 可以在[性能指南主题](sql-database-performance-guidance.md)一文中找到有关手动优化和重新编写代码的一些指南和提示。

## <a name="next-steps"></a>后续步骤

- 若要在 Azure SQL 数据库中启用自动优化并让自动优化功能完全管理工作负荷，请参阅[启用自动优化](sql-database-automatic-tuning-enable.md)。
- 若要使用手动优化，可以查看 [Azure 门户中的优化建议](sql-database-advisor-portal.md)并手动应用可以改进查询性能的建议。
- 通过更改 [Azure SQL 数据库服务层](sql-database-performance-guidance.md)更改数据库中可用的资源
