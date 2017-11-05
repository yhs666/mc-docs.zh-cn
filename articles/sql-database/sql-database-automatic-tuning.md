---
title: "Azure SQL 数据库 - 自动优化 | Microsoft Docs"
description: "Azure SQL 数据库可分析 SQL 查询并自动适应用户工作负荷。"
services: sql-database
documentationcenter: 
author: forester123
manager: digimobile
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
origin.date: 09/19/2017
ms.date: 11/06/2017
ms.author: v-johch
ms.openlocfilehash: b4801d3d292f69298270887a1b052c4e7ea75305
ms.sourcegitcommit: 5671b584a09260954f1e8e1ce936ce85d74b6328
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2017
---
# <a name="automatic-tuning-in-azure-sql-database"></a>Azure SQL 数据库中的自动优化

Azure SQL 数据库是完全托管的数据服务，用于监视在数据库上执行的查询，并且可自动提高数据库工作负荷的性能。 Azure SQL 数据库具有内置[自动优化](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning)智能机制，此机制可动态调整数据库使其适应工作负荷，以这种方式自动优化和提高查询性能。 Azure SQL 数据库中的自动优化可能是最重要的功能之一，在 Azure SQL 数据库中启用该功能即可优化查询的性能。

## <a name="automatic-tuning-options"></a>自动优化选项

Azure SQL 数据库中可用的[自动优化](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning)选项包含：
 1. 创建索引：用于标识可以提高工作负载性能的索引，创建索引，并验证它们是否可提高查询性能。
 2. 删除索引：标识冗余和重复索引，以及长期未使用的索引。
 3. 计划回归更正：标识正在使用执行计划的 SQL 查询（该执行计划速度慢于以前运行状态良好的计划），并使用上次已知的运行状态良好计划而不是回归计划。

Azure SQL 数据库标识可优化数据库的“创建索引”、“删除索引”和“计划回归更正”建议，并在 Azure 门户中显示它们。 如需详细了解如何标识应更改的索引，请参阅[在 Azure 门户中查找索引建议](sql-database-advisor-portal.md)。 可以使用门户手动应用建议，也可以让 Azure SQL 数据库自动应用建议，在更改后监视工作负载，并验证该建议是否改进了工作负载的性能。

每个数据库都可以独立打开或关闭自动优化选项，也可以在逻辑服务器上配置这些选项，并将其应用于从服务器继承设置的每个数据库。 在服务器上配置[自动优化](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning)选项，并继承服务器中数据库的设置，这是配置自动优化的建议方法，因为它可简化对大量数据库的自动优化选项的管理。

查看此文可以了解通过 Azure 门户[启用自动优化](sql-database-automatic-tuning-enable.md)的步骤。

## <a name="next-steps"></a>后续步骤

- 若要在 Azure SQL 数据库中启用自动优化并让自动优化功能完全管理工作负荷，请参阅[启用自动优化](sql-database-automatic-tuning-enable.md)。
- 若要使用手动优化，可以查看 [Azure 门户中的优化建议](sql-database-advisor-portal.md)并手动应用可以改进查询性能的建议。
- 详细了解 Azure SQL 数据库和 SQL Server 2017 中的[自动优化](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning)。
<!--Update_Description: whole content refine-->