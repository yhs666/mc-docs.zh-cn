---
title: 监视和性能优化 - Azure SQL 数据库 | Azure
description: 有关通过评估和改进来调整 Azure SQL 数据库性能的提示。
services: sql-database
author: forester123
manager: digimobile
editor: ''
keywords: sql 性能优化, 数据库性能优化, sql 性能优化提示, sql 数据库性能优化
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: troubleshooting
origin.date: 06/13/2017
ms.date: 11/06/2017
ms.author: v-johch
ms.openlocfilehash: f64abdcc996cd716a3565c5693a4abef0fd3728f
ms.sourcegitcommit: 2793c9971ee7a0624bd0777d9c32221561b36621
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/08/2018
---
# <a name="monitoring-and-performance-tuning"></a>监视和性能优化

Azure SQL 数据库是自动托管且灵活的数据服务，你可以在其中轻松监视使用情况，添加或删除资源（CPU、内存、IO），查找可以改进你的数据库性能的建议，或者让数据库适应你的工作负荷并自动优化性能。

本文概述了 Azure SQL 数据库中提供的监视和性能优化选项。

## <a name="monitoring-and-troubleshooting-database-performance"></a>监视数据库性能并对其进行故障排除

Azure SQL 数据库使得你可以轻松监视数据库使用情况并找出可能导致性能问题的查询。 可以使用 Azure 门户或系统视图监视数据库性能。 在监视数据库性能并对其进行故障排除方面，有以下选项可供选择：

1. 在 [Azure 门户](https://portal.azure.cn)中，单击 **SQL 数据库**，选择该数据库，然后使用“监视”图表查找接近其上限的资源。 默认情况下将显示 DTU 消耗量。 单击“编辑”可更改所显示的时间范围和值。
2. 使用 [Query Performance Insight](sql-database-query-performance.md) 可找出占用资源最多的查询。
3. 可以使用动态管理视图 (DMV)、扩展事件 (`XEvents`) 和 SSMS 中的查询存储实时获取性能参数。

如果使用这些报表或视图找出了一些问题，请参阅[性能指南主题](sql-database-performance-guidance.md)来查找可以用来改进 Azure SQL 数据库性能的技术。

> [!IMPORTANT] 
> 建议始终使用最新版本的 Management Studio 以保持与 Azure 和 SQL 数据库的更新同步。 [更新 SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)。
>

## <a name="optimize-database-to-improve-performance"></a>优化数据库以改进性能

Azure SQL 数据库使得你可以通过查看[性能优化建议](sql-database-advisor.md)找到机会来改进和优化查询性能，无需更改资源。 缺少索引与查询优化不足是数据库性能不佳的常见原因。 可以应用这些优化建议来改进你的工作负荷的性能。
还可以通过应用所有已确定的建议并确认它们改进了数据库性能来让 Azure SQL 数据库[自动优化查询性能](sql-database-automatic-tuning.md)。 可以使用以下选项来改进数据库的性能：

1. 使用 [SQL 数据库顾问](sql-database-advisor-portal.md)查看有关创建和删除索引、参数化查询，以及解决架构问题的建议。
2. [启用自动优化](sql-database-automatic-tuning-enable.md)并让 Azure SQL 数据库自动修复查明的性能问题。

## <a name="improving-database-performance-with-more-resources"></a>使用更多资源改进数据库性能

最后，如果没有可操作项可以改进数据库性能，你可以更改 Azure SQL 数据库中提供的资源数量。 你随时可以通过更改独立数据库的[服务层](sql-database-service-tiers.md)或增大弹性池的 eDTU 来分配更多资源。
1. 对于独立数据库，可以根据需要[更改服务层](sql-database-service-tiers.md)来改进数据库性能。
2. 对于多个数据库，请考虑使用[弹性池](sql-database-elastic-pool-guidance.md)自动调整资源规模。

## <a name="tune-and-refactor-application-or-database-code"></a>优化和重构应用程序或数据库代码

你可以更改应用程序代码来以更佳方式使用数据库，更改索引，强制实施计划或使用提示来手动使数据库适合你的工作负荷。 可以在[性能指南主题](sql-database-performance-guidance.md)一文中找到有关手动优化和重新编写代码的一些指南和提示。

## <a name="next-steps"></a>后续步骤

- 若要在 Azure SQL 数据库中启用自动优化并让自动优化功能完全管理工作负荷，请参阅[启用自动优化](sql-database-automatic-tuning-enable.md)。
- 若要使用手动优化，可以查看 [Azure 门户中的优化建议](sql-database-advisor-portal.md)并手动应用可以改进查询性能的建议。
- 通过更改 [Azure SQL 数据库服务层](sql-database-performance-guidance.md)更改数据库中可用的资源