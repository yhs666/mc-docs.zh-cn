---
title: Azure SQL 数据库发行说明 | Microsoft Docs
description: 了解 Azure SQL 数据库服务和 Azure SQL 数据库文档中的新功能和改进
services: sql-database
author: WenJason
manager: digimobile
ms.service: sql-database
ms.subservice: service
ms.devlang: ''
ms.topic: conceptual
origin.date: 03/05/2019
ms.date: 03/25/2019
ms.author: carlrab
ms.openlocfilehash: c3007e991e312178ebe6c987450dafd7af4f0b52
ms.sourcegitcommit: 02c8419aea45ad075325f67ccc1ad0698a4878f4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/21/2019
ms.locfileid: "58319039"
---
# <a name="sql-database-release-notes"></a>SQL 数据库发行说明

本文列出了 SQL 数据库服务和 SQL 数据库文档中的新功能和改进。

## <a name="march-2019"></a>2019 年 3 月

### <a name="service-improvements"></a>服务改进

| 服务改进 | 详细信息 |
| --- | --- |
| 即将支持 ||
| &nbsp; |

### <a name="documentation-improvements"></a>文档改进

| 文档改进 | 详细信息 |
| --- | --- |
| 添加了单一数据库的日志限制|有关详细信息，请参阅[单一数据库 vCore 资源限制](sql-database-vcore-resource-limits-single-databases.md)。|
| 添加了弹性池和共用数据库的日志限制|有关详细信息，请参阅[弹性池 vCore 资源限制](sql-database-vcore-resource-limits-elastic-pools.md)。|
| 添加了事务日志速率调控| 为[事务日志速率调控](sql-database-resource-limits-database-server.md#transaction-log-rate-governance)添加了新内容。|
| 更新了单一数据库和弹性池的 PowerShell 示例，使其能够使用 az.sql 模块 | 有关详细信息，请参阅[单一数据库和弹性池的 PowerShell 示例](sql-database-powershell-samples.md#single-database-and-elastic-pools)。|
| &nbsp; |

## <a name="february-2019"></a>2019 年 2 月

### <a name="service-improvements"></a>服务改进

| 服务改进 | 详细信息 |
| --- | --- |
|托管实例支持数据库重命名 | 有关更多详细信息，请参阅 [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-mi-current) 和 [sp_rename](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-rename-transact-sql) 语法。|
|SQL 数据库充当流分析的参考数据源。 | 有关详细信息，请参阅[流分析](/stream-analytics/)。|
| &nbsp; |

### <a name="documentation-improvements"></a>文档改进

| 文档改进 | 详细信息 |
| --- | --- |
|更新了基于 DTU 的购买模型的 tempdb 大小 | 有关详细信息，请参阅 [SQL 数据库中的 Tempdb 数据库](https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database?view=sql-server-2017#tempdb-database-in-sql-database)。|
| &nbsp; |


## <a name="january-2019"></a>2019 年 1 月

### <a name="service-improvements"></a>服务改进

| 服务改进 | 详细信息 |
| --- | --- |
| 计算资源的其他粒度选项 | [单一数据库](sql-database-vcore-resource-limits-single-databases.md)和[弹性池](sql-database-vcore-resource-limits-elastic-pools.md)的“常规用途”和“业务关键”服务层现在有更多细致的计算选项。|
| 高级威胁检测功能已重命名为“高级数据安全” | 单一数据库和弹性池的高级威胁检测功能已重命名为[高级数据安全](sql-advanced-threat-protection.md)。 |
| &nbsp; |

### <a name="documentation-improvements"></a>文档改进

| 文档改进 | 详细信息 |
| --- | --- |
| 更新了使用 Transact-SQL 脚本进行的作业自动化的内容 | 更新并澄清了将[使用 Transact-SQL 脚本进行的作业自动化](sql-database-job-automation-overview.md)用于单一数据库和弹性池的内容。 |
| 刷新了所有快速入门和教程 | [文档](/sql-database)中的所有快速入门和教程都已根据 Azure 门户中的更改进行更新和刷新 |
| 添加了快速入门概述指南 | 添加了[单一数据库](sql-database-quickstart-guide.md)的快速入门概述指南 |
| 添加了 SQL 数据库术语表 | 此[术语表](sql-database-glossary-terms.md)文章提供了一个最终列表，其中包含 SQL 数据库术语以及在上下文中解释术语的主要概念页的链接。 |
| &nbsp; |

## <a name="contribute-to-content-improvement"></a>参与内容改进

Azure SQL 文档集是开源的。 采用开源方式有几项优势：

- 开源存储库公开进行计划，可以通过反馈了解用户最需要什么样的文档。
- 开源存储库公开进行评审，可以在初次发布时就发布最有用的内容。
- 开源存储库公开进行更新，更容易持续改进内容。

