---
title: "SQL 数据库 - 自动优化 | Azure"
description: "SQL 数据库可分析 SQL 查询并自动适应用户工作负荷。"
services: sql-database
documentationcenter: 
author: Hayley244
manager: digimobile
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
origin.date: 06/05/2017
ms.date: 07/03/2017
ms.author: v-johch
ms.openlocfilehash: 9e78e4867fdd5d93a435b857760ca4391133748d
ms.sourcegitcommit: 73b1d0f7686dea85647ef194111528c83dbec03b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/28/2017
---
# 自动优化
<a id="automatic-tuning" class="xliff"></a>

Azure SQL 数据库是完全托管的数据服务，可以监视在数据库中执行的查询，并可自动改进工作负荷的性能。 Azure SQL 数据库具有内置的智能机制，能够让数据库动态地适应工作负荷，从而自动优化和改进查询的性能。 Azure SQL 数据库中的自动优化可能是最重要的功能之一，在 Azure SQL 数据库中启用该功能即可优化查询的性能。

查看此文可以了解通过 Azure 门户[启用自动优化](sql-database-automatic-tuning-enable.md)的步骤。

## 为何使用自动优化？
<a id="why-automatic-tuning" class="xliff"></a>

在经典数据库管理中，一个主要的任务就是监视工作负荷，确定关键的 SQL 查询、为了改进性能而应添加的索引，以及罕用的索引。 对于需要监视的查询和索引，Azure SQL 数据库提供详细的见解。 然而，持续监视数据库是一项艰巨且乏味的任务，尤其是在处理多个数据库时。 高效管理大量数据库也许是无法办到的事，即使使用 Azure SQL 数据库和 Azure 门户提供的所有可用工具和报表也是如此。 可考虑使用自动优化功能将某些监视和优化操作委派给 Azure SQL 数据库，而不是手动监视和优化数据库。 

> [!VIDEO https://channel9.msdn.com/Blogs/Azure-in-the-Enterprise/Enabling-Azure-SQL-Database-Auto-Tuning-at-Scale-for-Microsoft-IT/player]
>

## 自动优化工作原理
<a id="how-does-automatic-tuning-work" class="xliff"></a>

Azure SQL 数据库有一个持续的性能监视和分析过程，可持续了解工作负荷的特性并确定可能的问题和改进措施。

![自动优化过程](./media/sql-database-automatic-tuning/tuning-process.png)

Azure SQL 数据库可以通过此过程找出哪些索引和计划可以改进工作负荷的性能，以及哪些索引会影响工作负荷，从而动态地适应工作负荷。 自动优化可以根据这些观察结果应用优化操作，改进工作负荷的性能。 此外，Azure SQL 数据库还会在自动优化进行更改后持续监视性能，确保工作负荷的性能得到改进。 将会自动还原对改进性能无补的任何操作。 此验证过程是一项重要功能，可以确保自动优化所做的任何更改不会降低工作负荷的性能。

Azure SQL 数据库可以在两方面进行自动优化：

 -  自动索引管理：标识应在数据库中添加的索引以及应删除的索引。
 -  自动更正计划（即将推出，已在 SQL Server 2017 中提供）：标识有问题的计划并修复 SQL 计划性能问题。

## 自动索引管理
<a id="automatic-index-management" class="xliff"></a>

在 Azure SQL 数据库中进行索引管理很容易，因为 Azure SQL 数据库会了解工作负荷的情况，确保始终以最佳方式编制数据的索引。 正确的索引设计对于优化工作负荷的性能很关键，而自动索引管理则有助于优化索引。 自动索引管理可以修复索引编制不正确的数据库中的性能问题，还可以维护和改进现有数据库架构的索引。 

### 为什么需要索引管理？
<a id="why-do-you-need-index-management" class="xliff"></a>

索引可以加快某些从表读取数据的查询的速度，但也会降低需更新数据的查询的速度。 对于何时创建索引以及需在索引中包括哪些列，需仔细分析。 某些索引在过一段时间后可能就不需要了。 因此，需定期标识并删除那些无益的索引。 如果忽视不使用的索引，则会降低那些更新数据的查询的性能，同时也无益于那些读取数据的查询。 不使用的索引还会影响系统整体性能，因为进行额外更新所需的日志记录是不必要的。

可能需要持续且复杂的分析，才能找出优化的索引集，既能改进那些从表中读取数据的查询的性能，又能尽量减少对更新的影响。

Azure SQL 数据库使用内置的智能和高级规则来分析查询，确定适用于当前工作负荷的索引，以及可以删除的索引。 Azure SQL 数据库可确保尽量缩小能够优化读取数据的查询的必需索引集，并尽量降低对其他查询的影响。

### 如何标识数据库中需更改的索引？
<a id="how-to-identify-indexes-that-need-to-be-changed-in-your-database" class="xliff"></a>

Azure SQL 数据库简化了索引管理过程。 Azure SQL 数据库会分析工作负荷，确定哪些查询在使用新索引时可加快执行速度，同时确定不使用的或重复的索引，不需用户对工作负荷进行单调的手动分析，也不需手动监视索引。 如需详细了解如何标识应更改的索引，请参阅[在 Azure 门户中查找索引建议](sql-database-advisor-portal.md)。

### 自动索引管理注意事项
<a id="automatic-index-management-considerations" class="xliff"></a>

如果你发现内置的规则可以改进数据库的性能，则可让 Azure SQL 数据库自动管理索引。

在 Azure SQL 数据库中创建必需的索引时，需执行多种操作，这可能会占用资源，暂时影响工作负荷性能。 为了尽量降低创建索引对工作负荷性能的影响，Azure SQL 数据库会找出适当的时间窗口来执行索引管理操作。 如果数据库需要资源来执行工作负荷，则会推迟优化操作，等到数据库有足够的未使用资源来执行维护任务时再启动优化操作。 自动索引管理的一项重要功能是对操作进行验证。 当 Azure SQL 数据库创建或删除索引时，会通过监视过程分析工作负荷的性能，验证操作是否改进了性能。 如果性能并无明显改进，则会立即还原操作。 Azure SQL 数据库通过这种方式确保自动操作不会对工作负荷的性能造成负面影响。 通过自动优化创建的索引是透明的，便于在基础架构上进行维护操作。 架构更改（例如删除或重命名列）不会因自动创建的索引的存在而受到阻止。 删除相关的表或列时，会立即删除 Azure SQL 数据库自动创建的索引。

## 自动更正所选计划
<a id="automatic-plan-choice-correction" class="xliff"></a>

自动更正计划是 SQL Server 2017 CTP2.0 中新增的自动优化功能，可标识不适当的 SQL 计划。 Azure SQL 数据库中很快会提供此自动优化选项。 有关详细信息，请参阅 [SQL Server 2017 中的自动优化](/sql/relational-databases/automatic-tuning/automatic-tuning)。

## 后续步骤
<a id="next-steps" class="xliff"></a>

- 若要在 Azure SQL 数据库中启用自动优化并让自动优化功能完全管理工作负荷，请参阅[启用自动优化](sql-database-automatic-tuning-enable.md)。
- 若要使用手动优化，可以查看 [Azure 门户中的优化建议](sql-database-advisor-portal.md)并手动应用可以改进查询性能的建议。