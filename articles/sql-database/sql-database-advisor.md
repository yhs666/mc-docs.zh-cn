---
title: "性能建议 - Azure SQL 数据库 | Azure"
description: "Azure SQL 数据库顾问为你的现有 SQL 数据库提供建议，这样可以提高当前的查询性能。"
services: sql-database
documentationCenter: 
author: Hayley244
manager: digimobile
editor: monicar
ms.service: sql-database
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
origin.date: 09/30/2016
ms.date: 07/03/2017
ms.author: v-johch
ms.openlocfilehash: 11bf28abff738bc622aa980d3e63a353fc35f48f
ms.sourcegitcommit: bb82041119027be7a62fc96945d92a8a452e7849
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/28/2017
---
<a id="performance-recommendations" class="xliff"></a>

# 性能建议

Azure SQL 数据库可以学习和适应你的应用程序并提供自定义的建议，使你能够将 SQL 数据库的性能最大化。 将通过分析 SQL 数据库使用情况历史记录持续对性能进行评估。 提供的建议基于数据库特有工作负荷模式并改进其性能。

> [!NOTE]
> 推荐使用建议的方式是在数据库上启用“自动优化”。 有关详细信息，请参阅[自动优化](sql-database-automatic-tuning.md)。
>

<a id="create-index-recommendations" class="xliff"></a>

## 创建索引建议
Azure SQL 数据库持续监视执行的查询并找出可以改进性能的索引。 一旦对于缺少特定的索引建立了足够的置信度，便会创建一个新的**创建索引**建议。 Azure SQL 数据库通过评估索引随时间流逝会带来的性能提升来构建置信度。 根据评估得到的性能提升，可以将建议归类为“高”、“中”或“低”。 

使用建议创建的索引始终标记为 auto_created 索引。 可以通过查看 sys.indexes 视图来查看哪些索引是 auto_created 的。 自动创建的索引不会阻止 ALTER/RENAME 命令。 如果尝试删除自动创建某个索引时所基于的列，则命令会通过，并且还会通过该命令删除自动创建的索引。 常规索引会阻止对具有索引的列执行的 ALTER/RENAME 命令。

在应用创建索引建议后，Azure SQL 数据库会将查询性能与基线性能进行比较。 如果新索引带来了性能改进，则建议将被标记为成功的，并且会提供影响报告。 如果索引没有带来好处，则它将自动恢复。 通过这种方式，Azure SQL 数据库可以确保使用建议只会改进数据库性能。

任何**创建索引**建议都有一个放弃策略，如果数据库或池 DTU 使用率在过去 20 分钟内高于 80% 或者存储使用率高于 90%，则该策略不允许应用建议。 在这种情况下，建议将被推迟。

<a id="drop-index-recommendations" class="xliff"></a>

## 删除索引建议
除了检测缺少的索引外，Azure SQL 数据库还会持续分析现有索引的性能。 如果某个索引未使用，Azure SQL 数据库会建议删除该索引。 在两种情况下会建议删除索引：
* 索引是另一个索引的副本（索引列以及包含的列、分区架构和筛选器相同）
* 索引在很长一段时间内未使用（93 天）

删除索引建议在实现后也要进行验证。 如果性能得以改进，则会提供影响报告。 如果检测到性能降级，则会恢复建议。

<a id="parameterize-queries-recommendations" class="xliff"></a>

## 参数化查询建议

**参数化查询** 建议。 这种状态提供了一个应用强制参数化的机会，其允许查询计划进行缓存并在将来可以被重复使用，从而改善性能和减少资源使用。 

对 SQL Server 发出的每个查询一开始需要进行编译，生成执行计划。 每个生成的计划将添加到计划缓存中，相同查询的后续执行可以重复使用该缓存中的此计划，而无需进一步编译。 

发送包含非参数化值的查询的应用程序可能会导致性能开销，其中对于使用不同参数值的每个此类查询，将重新编译执行计划。 在许多情况下，使用不同参数值的相同查询将生成相同的执行计划，但这些计划将仍然分别添加到计划缓存中。 重新编译执行计划会占用数据库资源、增加查询持续时间并使计划缓存溢出，从而导致系统从缓存中逐出计划。 可以通过对数据库设置强制参数化选项来更改 SQL Server 的此行为。 

为了帮助你估计此建议的影响，将为你提供实际 CPU 使用率和预计 CPU 使用率（就像已应用建议一样）之间的比较。 除了节省 CPU 外，查询持续时间会因编译花费的时间而减少。 计划缓存上的开销也会更低，从而允许大部分计划保留在缓存中并可重复使用。 可以通过单击“应用”命令来快速轻松地应用此建议。 

应用此建议后，它将在几分钟之内对数据库启用强制参数化，并将启动监视进程（大约持续 24 小时）。 经过这段时间后，即可看到验证报告，该报表显示应用此建议之前和之后的 24 小时内数据库的 CPU 使用率。 SQL 数据库顾问具有安全机制，在检测到性能衰退的情况下，可以自动还原已应用的建议。

<a id="fix-schema-issues-recommendations" class="xliff"></a>

## 修复架构问题建议

当 SQL 数据库服务发现 Azure SQL 数据库上架构相关 SQL 错误的数量发生异常时，就会出现**修复架构问题**建议。 此建议通常在数据库在一个小时内遭遇到多个架构相关的错误（无效的列名称、无效的对象名称等）时出现。

“架构问题”是 SQL Server 中的一类语法错误，当 SQL 查询的定义与数据库架构的定义不一致时发生。 例如，目标表中可能缺少查询所需的某个列，反之亦然。 

当 Azure SQL 数据库服务发现 Azure SQL 数据库上发生的架构相关 SQL 错误的数量有异常时，就会出现“修复架构问题”建议。 下表显示与架构问题相关的错误：

| SQL 错误代码 | 消息 |
| --- | --- |
| 201 |过程或函数“*”需要参数“*”，但未提供该参数。 |
| 207 |列名称“*”无效。 |
| 208 |对象名“*”无效。 |
| 213 |列名称或所提供值的数目与表定义不匹配。 |
| 2812 |找不到存储过程“*”。 |
| 8144 |为过程或函数 * 指定了过多的参数。 |

<a id="next-steps" class="xliff"></a>

## 后续步骤
监视建议并继续应用它们以优化性能。 数据库工作负荷是动态的，并且不断地更改。 SQL 数据库顾问将继续监视和提供可能提高数据库性能的建议。 

* 有关如何使用 Azure 门户中的性能建议的步骤，请参阅 [Azure 门户中的性能建议](sql-database-advisor-portal.md)。
* 若要了解和查看排名靠前的查询的性能影响，请参阅[查询性能见解](sql-database-query-performance.md)。

<a id="additional-resources" class="xliff"></a>

## 其他资源
* [查询存储](https://msdn.microsoft.com/library/dn817826.aspx)
* [CREATE INDEX](https://msdn.microsoft.com/library/ms188783.aspx)
* [基于角色的访问控制](../active-directory/role-based-access-control-configure.md)