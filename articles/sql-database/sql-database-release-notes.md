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
ms.date: 04/29/2019
ms.author: carlrab
ms.openlocfilehash: 32a1ac6addacb5e149737f0cbc57e48bf6874565
ms.sourcegitcommit: 9642fa6b5991ee593a326b0e5c4f4f4910f50742
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2019
ms.locfileid: "64855455"
---
# <a name="sql-database-release-notes"></a>SQL 数据库发行说明

本文列出了 SQL 数据库服务和 SQL 数据库文档中的新功能和改进。

## <a name="features-in-public-preview"></a>处于公共预览版的功能

| 功能 | 详细信息 |
| ---| --- |
| 弹性数据库作业 | 有关信息，请参阅[创建、配置和管理弹性作业](elastic-jobs-overview.md) |
| 弹性事务 | [跨云数据库的分布式事务](sql-database-elastic-transactions-overview.md) |
| 弹性查询 | 有关信息，请参阅[弹性查询概述](sql-database-elastic-query-overview.md) |
| 数据发现和分类  |有关信息，请参阅 [Azure SQL 数据库和 SQL 数据仓库数据发现和分类](sql-database-data-discovery-and-classification.md)|
| 托管实例的透明数据加密 (TDE) 和自带密钥 (BYOK) |有关信息，请参阅[使用 Azure Key Vault 中由客户管理的密钥进行 Azure SQL 透明数据加密：自带密钥支持](transparent-data-encryption-byok-azure-sql.md)|
| Azure 门户中的查询编辑器 |有关信息，请参阅[使用 Azure 门户的 SQL 查询编辑器进行连接并查询数据](sql-database-connect-query-portal.md)|
| 估计非重复计数|有关信息，请参阅[估计非重复计数](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#approximate-query-processing)|
| 行存储上的批处理模式（在兼容性级别 150 下）|有关信息，请参阅[行存储上的批处理模式](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#batch-mode-on-rowstore)|
| 内存授予反馈（行模式）（在兼容性级别 150 下）|有关信息，请参阅[内存授予反馈（行模式）](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#row-mode-memory-grant-feedback)|
| 表变量延迟编译（在兼容性级别 150 下）|有关信息，请参阅[表变量延迟的编译](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#table-variable-deferred-compilation)|

## <a name="march-2019"></a>2019 年 3 月

### <a name="service-improvements"></a>服务改进

| 服务改进 | 详细信息 |
| --- | --- |
| 正式版：支持针对 Azure SQL 数据库的读取扩展 | 有关详细信息，请参阅[读取扩展](sql-database-read-scale-out.md)|
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

