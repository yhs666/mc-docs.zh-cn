---
title: 监视和性能优化 - Azure SQL 数据库 | Microsoft Docs
description: 有关通过评估和改进来调整 Azure SQL 数据库性能的提示。
services: sql-database
author: WenJason
manager: digimobile
editor: ''
keywords: sql 性能优化, 数据库性能优化, sql 性能优化提示, sql 数据库性能优化
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: conceptual
origin.date: 07/16/2018
ms.date: 10/15/2018
ms.author: v-jay
ms.reviewer: carlrab
ms.openlocfilehash: 2c3ec8ed9d865ae36d6eab0d865c59bb198c1319
ms.sourcegitcommit: d8b4e1fbda8720bb92cc28631c314fa56fa374ed
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/10/2018
ms.locfileid: "48914102"
---
# <a name="monitoring-and-performance-tuning"></a>监视和性能优化

Azure SQL 数据库是自动托管且灵活的数据服务，你可以在其中轻松监视使用情况，添加或删除资源（CPU、内存、IO），查找可以改进你的数据库性能的建议，或者让数据库适应你的工作负荷并自动优化性能。

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

* CPU 百分比
* DTU 百分比
* 数据 IO 百分比
* 数据库大小百分比

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
