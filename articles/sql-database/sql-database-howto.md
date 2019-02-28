---
title: 如何配置 Azure SQL 数据库 | Microsoft Docs
description: 了解如何配置和管理 Azure SQL 数据库。
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: howto
author: WenJason
ms.author: v-jay
ms.reviewer: carlr
manager: digimobile
origin.date: 01/25/2019
ms.date: 02/25/2019
ms.openlocfilehash: 67812909e6fcf742f231fc7d884c8358dcfc9f17
ms.sourcegitcommit: 5ea744a50dae041d862425d67548a288757e63d1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/22/2019
ms.locfileid: "56663713"
---
# <a name="how-to-use-azure-sql-database"></a>如何使用 Azure SQL 数据库

在本部分中，你可以找到可帮助你管理和配置 Azure SQL 数据库的各种指南、脚本和说明。 还可以找到适用于[单一数据库](sql-database-howto-single-database.md)的特定操作指南。

## <a name="load-data"></a>加载数据

- [在 Azure 中复制单一数据库或入池数据库](sql-database-copy.md)
- [从 BACPAC 导入 DB](sql-database-import.md)
- [将 DB 导出到 BACPAC](sql-database-export.md)
- [使用 BCP 加载数据](sql-database-load-from-csv-with-bcp.md)

### <a name="data-sync"></a>数据同步

- [SQL 数据同步](sql-database-sync-data.md)
- [Data Sync Agent](sql-database-data-sync-agent.md)
- [复制架构更改](sql-database-update-sync-schema.md)
- [数据同步最佳做法](sql-database-best-practices-data-sync.md)
- [数据同步故障排除](sql-database-troubleshoot-data-sync.md)

## <a name="monitoring-and-tuning"></a>监视和优化

- [手动优化](sql-database-performance-guidance.md)
- [使用 DMV 监视性能](sql-database-monitoring-with-dmvs.md)
- [使用查询存储监视性能](sql-database-operate-query-store.md)
- [使用智能见解排查性能问题](sql-database-intelligent-insights-troubleshoot-performance.md)
- [使用智能见解诊断日志](sql-database-intelligent-insights-use-diagnostics-log.md)
- [监视内存中 OLTP 空间](sql-database-in-memory-oltp-monitoring.md)

### <a name="extended-events"></a>扩展的事件

- [扩展的事件](sql-database-xevent-db-diff-from-svr.md)
- [将扩展的事件存储到事件文件](sql-database-xevent-code-event-file.md)
- [将扩展的事件存储到环形缓冲区](sql-database-xevent-code-ring-buffer.md)

## <a name="configure-features"></a>配置功能

- [配置 Azure AD 身份验证](sql-database-aad-authentication-configure.md)
- [多重 AAD 身份验证](sql-database-ssms-mfa-authentication.md)
- [配置多重身份验证](sql-database-ssms-mfa-authentication-configure.md)
- [配置时态保留策略](sql-database-temporal-tables-retention-policy.md)
- [配置内存中 OLTP](sql-database-in-memory-oltp-migration.md)
- [配置 Azure 自动化](sql-database-manage-automation.md)

## <a name="develop-applications"></a>开发应用程序

- [连接](sql-database-libraries.md)
- [使用 Spark 连接器](sql-database-spark-connector.md)
- [对应用进行身份验证](sql-database-client-id-keys.md)
- [错误消息](sql-database-develop-error-messages.md)
- [使用批处理提高性能](sql-database-use-batching-to-improve-performance.md)
- [连接指南](sql-database-connectivity-issues.md)
- [端口 - ADO.NET](sql-database-develop-direct-route-ports-adonet-v12.md)
- [C 和 C ++](sql-database-develop-cplusplus-simple.md)
- [Excel](sql-database-connect-excel.md)

## <a name="design-applications"></a>设计应用程序

- [设计灾难恢复](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- [设计弹性池](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md)
- [设计应用升级](sql-database-manage-application-rolling-upgrade.md)

## <a name="next-steps"></a>后续步骤

- 详细了解[单一数据库操作指南](sql-database-howto-single-database.md)。
