---
title: Azure Database for MySQL 的审核日志
description: 介绍了 Azure Database for MySQL 中提供的审核日志，以及用于启用日志记录级别的可用参数。
author: WenJason
ms.author: v-jay
ms.service: mysql
ms.topic: conceptual
origin.date: 06/11/2019
ms.date: 07/15/2019
ms.openlocfilehash: 770735eb7264781ee8b42a5fba97a3ba3fcdc7c2
ms.sourcegitcommit: f4351979a313ac7b5700deab684d1153ae51d725
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/12/2019
ms.locfileid: "67845549"
---
# <a name="audit-logs-in-azure-database-for-mysql"></a>Azure Database for MySQL 中的审核日志

在 Azure Database for MySQL 中，审核日志可供用户使用。 审核日志可以用来跟踪数据库级别的活动，通常用于确保符合性。

> [!IMPORTANT]
> 审核日志功能目前为预览版。

## <a name="configure-audit-logging"></a>配置审核日志记录

默认情况下，审核日志被禁用。 若要启用它，请将 `audit_log_enabled` 设置为 ON。

可以调整的其他参数包括：

- `audit_log_events`：控制要记录的事件。 请查看下表以了解具体的审核事件。
- `audit_log_exclude_users`：不对 MySQL 用户进行日志记录。 最多允许对四个用户这样做。 参数的最大长度为 256 个字符。

| **事件** | **说明** |
|---|---|
| `CONNECTION` | - 启动连接（成功或不成功） <br> - 在会话期间使用不同的用户/密码对用户重新进行身份验证 <br> - 终止连接 |
| `DML_SELECT`| SELECT 查询 |
| `DML_NONSELECT` | INSERT/DELETE/UPDATE 查询 |
| `DML` | DML = DML_SELECT + DML_NONSELECT |
| `DDL` | 类似“DROP DATABASE”的查询 |
| `DCL` | 类似“GRANT PERMISSION”的查询 |
| `ADMIN` | 类似“SHOW STATUS”的查询 |
| `GENERAL` | All in DML_SELECT, DML_NONSELECT, DML, DDL, DCL, and ADMIN |
| `TABLE_ACCESS` | - 仅适用于 MySQL 5.7 <br> - 表读取语句，例如 SELECT 或 INSERT INTO ...SELECT <br> - 表删除语句，例如 DELETE 或 TRUNCATE TABLE <br> - 表插入语句，例如 INSERT 或 REPLACE <br> - 表更新语句，例如 UPDATE |

## <a name="access-audit-logs"></a>访问审核日志

审核日志与 Azure Monitor 诊断日志集成。 在 MySQL 服务器上启用审核日志后，可以将它们发送到 Azure Monitor 日志、事件中心或 Azure 存储。 若要详细了解如何在 Azure 门户中启用诊断日志，请参阅[审核日志门户文章](howto-configure-audit-logs-portal.md#set-up-diagnostic-logs)。

## <a name="schemas"></a>架构

以下部分介绍基于事件类型的 MySQL 审核日志的输出是什么。 根据输出方法，包含的字段以及这些字段出现的顺序可能会有所不同。

### <a name="connection"></a>连接

| **属性** | **说明** |
|---|---|
| `TenantId` | 租户 ID |
| `SourceSystem` | `Azure` |
| `TimeGenerated` [UTC] | 记录日志时的时间戳 (UTC) |
| `Type` | 日志的类型。 始终是 `AzureDiagnostics` |
| `SubscriptionId` | 服务器所属的订阅的 GUID |
| `ResourceGroup` | 服务器所属的资源组的名称 |
| `ResourceProvider` | 资源提供程序的名称。 始终是 `MICROSOFT.DBFORMYSQL` |
| `ResourceType` | `Servers` |
| `ResourceId` | 资源 URI |
| `Resource` | 服务器的名称 |
| `Category` | `MySqlAuditLogs` |
| `OperationName` | `LogEvent` |
| `event_class` | `connection_log` |
| `event_subclass` | `CONNECT`、`DISCONNECT`、`CHANGE USER`（仅适用于 MySQL 5.7） |
| `connection_id` | MySQL 生成的唯一连接 ID |
| `host` | 空白 |
| `ip` | 连接到 MySQL 的客户端的 IP 地址 |
| `user` | 执行查询的用户的名称 |
| `db` | 连接的数据库的名称 |
| `\_ResourceId` | 资源 URI |

### <a name="general"></a>常规

下面的架构适用于 GENERAL、DML_SELECT、DML_NONSELECT、DML、DDL、DCL 和 ADMIN 事件类型。

| **属性** | **说明** |
|---|---|
| `TenantId` | 租户 ID |
| `SourceSystem` | `Azure` |
| `TimeGenerated` [UTC] | 记录日志时的时间戳 (UTC) |
| `Type` | 日志的类型。 始终是 `AzureDiagnostics` |
| `SubscriptionId` | 服务器所属的订阅的 GUID |
| `ResourceGroup` | 服务器所属的资源组的名称 |
| `ResourceProvider` | 资源提供程序的名称。 始终是 `MICROSOFT.DBFORMYSQL` |
| `ResourceType` | `Servers` |
| `ResourceId` | 资源 URI |
| `Resource` | 服务器的名称 |
| `Category` | `MySqlAuditLogs` |
| `OperationName` | `LogEvent` |
| `event_class` | `general_log` |
| `event_subclass` | `LOG`、`ERROR`、`RESULT`（仅适用于 MySQL 5.6） |
| `event_time` | 以 UNIX 时间戳表示的查询开始时的秒数 |
| `error_code` | 查询失败时的错误代码。 `0` 意味着无错误 |
| `thread_id` | 执行了查询的线程的 ID |
| `host` | 空白 |
| `ip` | 连接到 MySQL 的客户端的 IP 地址 |
| `user` | 执行查询的用户的名称 |
| `sql_text` | 完整查询文本 |
| `\_ResourceId` | 资源 URI |

### <a name="table-access"></a>表访问权限

| **属性** | **说明** |
|---|---|
| `TenantId` | 租户 ID |
| `SourceSystem` | `Azure` |
| `TimeGenerated` [UTC] | 记录日志时的时间戳 (UTC) |
| `Type` | 日志的类型。 始终是 `AzureDiagnostics` |
| `SubscriptionId` | 服务器所属的订阅的 GUID |
| `ResourceGroup` | 服务器所属的资源组的名称 |
| `ResourceProvider` | 资源提供程序的名称。 始终是 `MICROSOFT.DBFORMYSQL` |
| `ResourceType` | `Servers` |
| `ResourceId` | 资源 URI |
| `Resource` | 服务器的名称 |
| `Category` | `MySqlAuditLogs` |
| `OperationName` | `LogEvent` |
| `event_class` | `table_access_log` |
| `event_subclass` | `READ`、`INSERT`、`UPDATE` 或 `DELETE` |
| `connection_id` | MySQL 生成的唯一连接 ID |
| `db` | 访问的数据库的名称 |
| `table` | 访问的表的名称 |
| `sql_text` | 完整查询文本 |
| `\_ResourceId` | 资源 URI |

## <a name="next-steps"></a>后续步骤

- [如何在 Azure 门户中配置审核日志](howto-configure-audit-logs-portal.md)