---
title: 如何配置 Azure SQL 数据库 | Microsoft Docs
description: 了解如何配置和管理 Azure SQL 数据库。
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: howto
author: WenJason
ms.author: v-jay
ms.reviewer: carlr
manager: digimobile
origin.date: 12/14/2018
ms.date: 12/31/2018
ms.openlocfilehash: cb604f5c38e47b510422c3f0c695aa56130e778e
ms.sourcegitcommit: e96e0c91b8c3c5737243f986519104041424ddd5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/28/2018
ms.locfileid: "53806412"
---
# <a name="how-to-use-azure-sql-database"></a>如何使用 Azure SQL 数据库

在本部分中，你可以找到可帮助你管理和配置 Azure SQL 数据库的各种指南、脚本和说明。 还可以找到适用于[单一数据库](sql-database-howto-single-database.md)的特定操作指南。

## <a name="load-data"></a>加载数据

- [在 Azure 中复制单一数据库](/sql-database/sql-database-copy)
- [从 BACPAC 导入 DB](/sql-database/sql-database-import)
- [将 DB 导出到 BACPAC](/sql-database/sql-database-export)
- [使用 BCP 加载数据](/sql-database/sql-database-load-from-csv-with-bcp)

### <a name="data-sync"></a>数据同步

- [SQL 数据同步](/sql-database/sql-database-sync-data)
- [复制架构更改](/sql-database/sql-database-update-sync-schema)
- [数据同步最佳做法](/sql-database/sql-database-best-practices-data-sync)
- [数据同步故障排除](/sql-database/sql-database-troubleshoot-data-sync)

## <a name="monitoring-and-tuning"></a>监视和优化

- [手动优化](/sql-database/sql-database-performance-guidance)
- [使用 DMV 监视性能](/sql-database/sql-database-monitoring-with-dmvs)
- [使用查询存储监视性能](/sql-database/sql-database-operate-query-store)
- [监视内存中 OLTP 空间](/sql-database/sql-database-in-memory-oltp-monitoring)

### <a name="extended-events"></a>扩展的事件

- [扩展的事件](/sql-database/sql-database-xevent-db-diff-from-svr)
- [将扩展的事件存储到事件文件](/sql-database/sql-database-xevent-code-event-file)
- [将扩展的事件存储到环形缓冲区](/sql-database/sql-database-xevent-code-ring-buffer)

## <a name="configure-features"></a>配置功能

- [配置 Azure AD 身份验证](/sql-database/sql-database-aad-authentication-configure)
- [多重 AAD 身份验证](/sql-database/sql-database-ssms-mfa-authentication)
- [配置多重身份验证](/sql-database/sql-database-ssms-mfa-authentication-configure)
- [配置时态保留策略](/sql-database/sql-database-temporal-tables-retention-policy)
- [配置内存中 OLTP](/sql-database/sql-database-in-memory-oltp-migration)
- [配置 Azure 自动化](/sql-database/sql-database-manage-automation)

## <a name="develop-applications"></a>开发应用程序

- [连接](/sql-database/sql-database-libraries)
- [使用 Spark 连接器](/sql-database/sql-database-spark-connector)
- [对应用进行身份验证](/sql-database/sql-database-client-id-keys)
- [错误消息](/sql-database/sql-database-develop-error-messages)
- [使用批处理提高性能](/sql-database/sql-database-use-batching-to-improve-performance)
- [连接指南](/sql-database/sql-database-connectivity-issues)
- [端口 - ADO.NET](/sql-database/sql-database-develop-direct-route-ports-adonet-v12)
- [C 和 C ++](/sql-database/sql-database-develop-cplusplus-simple)
- [Excel](/sql-database/sql-database-connect-excel)

## <a name="design-applications"></a>设计应用程序

- [设计灾难恢复](/sql-database/sql-database-designing-cloud-solutions-for-disaster-recovery)
- [设计弹性池](/sql-database/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool)
- [设计应用升级](/sql-database/sql-database-manage-application-rolling-upgrade)

## <a name="next-steps"></a>后续步骤
- 详细了解[单一数据库中的操作指南](sql-database-howto-single-database.md)。
