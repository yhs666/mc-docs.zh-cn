---
title: Azure SQL 数据库指标和诊断日志记录 | Microsoft Docs
description: 了解如何配置 Azure SQL 数据库以存储资源使用情况和查询执行统计信息。
services: sql-database
ms.service: sql-database
ms.subservice: monitor
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: WenJason
ms.author: v-jay
ms.reviewer: jrasnik, carlrab
manager: digimobile
origin.date: 02/07/2019
ms.date: 02/25/2019
ms.openlocfilehash: 3db74beaf9fc2e5d94d74694a3b79c337d22989b
ms.sourcegitcommit: 5ea744a50dae041d862425d67548a288757e63d1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/22/2019
ms.locfileid: "56663848"
---
# <a name="azure-sql-database-metrics-and-diagnostics-logging"></a>Azure SQL 数据库指标和诊断日志记录

单一数据库和弹性池中的入池数据库可以流式传输指标和诊断日志，方便进行性能监视。 可以配置数据库，以将资源使用情况、辅助角色和会话以及连接性传输到以下 Azure 资源之一：

- **Azure 事件中心**：将 SQL 数据库遥测与自定义监视解决方案或热管道相集成。
- **Azure 存储**：低价存档大量遥测数据。

有关各种 Azure 服务支持的指标和日志类别的详细信息，请参阅：

- [Azure 中的指标概述](../monitoring-and-diagnostics/monitoring-overview-metrics.md)
- [Azure 诊断日志概述](../azure-monitor/platform/diagnostic-logs-overview.md)

本文提供的指导可帮助你为 Azure SQL 数据库和弹性池启用诊断遥测。

## <a name="enable-logging-of-diagnostics-telemetry"></a>启用诊断遥测的日志记录

可使用以下方法之一来启用和管理指标与诊断遥测日志记录：

- Azure 门户
- PowerShell
- Azure CLI
- Azure Monitor REST API
- Azure Resource Manager 模板

启用指标和诊断日志记录时，需要指定收集诊断遥测数据的 Azure 资源目标。 可用选项包括：

- Azure 事件中心
- Azure 存储

可预配新的 Azure 资源或选择现有资源。 使用“诊断设置”选项选择资源之后，指定要收集的数据。

> [!NOTE]
> 如果还使用弹性池，建议也为这些资源启用诊断遥测。 弹性池中的数据库容器具有自身的独立诊断遥测。

## <a name="enable-logging-for-azure-sql-databases"></a>为 Azure SQL 数据库启用日志记录

对 SQL 数据库启用指标与诊断日志记录 - 默认未启用此功能。

可将 Azure SQL 数据库设置为收集以下诊断遥测数据：

| 数据库的监视遥测 | 单一数据库和入池数据库支持 |
| :------------------- | ------------------- | ------------------- |
| [所有指标](#all-metrics)：包含 DTU/CPU 百分比、DTU/CPU 限制、物理数据读取百分比、日志写入百分比、成功/失败/防火墙阻止的连接数、会话百分比、辅助角色百分比、存储、存储百分比和 XTP 存储百分比。 | 是 |
| [QueryStoreRuntimeStatistics](#query-store-runtime-statistics)：包含有关查询运行时统计信息的信息，例如 CPU 使用率、查询持续时间统计信息。 | 是 |
| [QueryStoreWaitStatistics](#query-store-wait-statistics)：包含有关查询等待统计信息的信息（查询正在等待什么），例如 CPU、日志和锁定。 | 是 |
| [Errors](#errors-dataset):包含有关数据库发生的 SQL 错误的信息。 | 是 |
| [DatabaseWaitStatistics](#database-wait-statistics-dataset)：包含有关数据库针对不同等待类型花费多少时间等待的信息。 | 是 |
| [Timeouts](#time-outs-dataset)：包含有关数据库发生的超时的信息。 | 是 |
| [Blocks](#blockings-dataset)：包含有关数据库发生的阻塞事件的信息。 | 是 |
| [SQLInsights](#intelligent-insights-dataset)：包含性能的智能见解。 有关详细信息，请参阅[智能见解](sql-database-intelligent-insights.md)。 | 是 |

### <a name="azure-portal"></a>Azure 门户

在 Azure 门户中使用每个单一数据库或入池数据库的“诊断设置”菜单，为 Azure SQL 数据库配置诊断遥测数据的流式传输。 可设置以下目标：Azure 存储和 Azure 事件中心。

### <a name="configure-streaming-of-diagnostics-telemetry-for-single-or-pooled"></a>为单一或入池数据库配置诊断遥测数据的流式传输

   ![SQL 数据库图标](./media/sql-database-metrics-diag-logging/icon-sql-database-text.png)

若要为单一数据库或入池数据库启用诊断遥测数据的流式传输，请执行以下步骤：

1. 转到 Azure SQL 数据库资源。
1. 选择“诊断设置”。
1. 选择“启用诊断”（如果不存在以前的设置），或选择“编辑设置”来编辑以前的设置
   - 最多可以创建两个并行连接，用于流式传输诊断遥测数据。
   - 选择“+添加诊断设置”，配置为将诊断数据并行流式传输到多个资源。

   ![为单一数据库或入池数据库启用诊断](./media/sql-database-metrics-diag-logging/diagnostics-settings-database-sql-enable.png)
1. 输入设置名称供自己参考。
1. 选择诊断数据要流式传输到的目标资源：“存档到存储帐户”或“流式传输到事件中心”。
1. 对于标准的基于事件的监视体验，请选中数据库诊断日志遥测对应的以下复选框：“SQLInsights”、“AutomaticTuning”、“QueryStoreRuntimeStatistics”、“QueryStoreWaitStatistics”、“Errors”、“DatabaseWaitStatistics”、“Timeouts”、“Blocks”和“Deadlocks”。
1. 对于高级的一分钟间隔监视体验，请选中“AllMetrics”对应的复选框。
1. 选择“其他安全性验证” 。

   ![为单一数据库或入池数据库配置诊断](./media/sql-database-metrics-diag-logging/diagnostics-settings-database-sql-selection.png)

> [!NOTE]
> 无法从数据库诊断设置启用安全审核日志。 若要启用审核日志流式传输，请参阅[为数据库设置审核](sql-database-auditing.md#subheading-2)。
> [!TIP]
> 针对要监视的每个 Azure SQL 数据库重复上述步骤。

### <a name="rest-api"></a>REST API

阅读有关如何[使用 Azure Monitor REST API 更改诊断设置](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings)的信息。

### <a name="resource-manager-template"></a>Resource Manager 模板

阅读有关如何[在创建资源时使用资源管理器模板启用诊断设置](../azure-monitor/platform/diagnostic-logs-stream-template.md)的信息。

## <a name="stream-into-event-hubs"></a>流式传输到事件中心

在 Azure 门户中使用内置的“流式传输到事件中心”选项可将 SQL 数据库指标和诊断日志流式传输到事件中心。 此外，还可以通过 PowerShell cmdlet、Azure CLI 或 Azure Monitor REST API 使用诊断设置来启用服务总线规则 ID。

### <a name="what-to-do-with-metrics-and-diagnostics-logs-in-event-hubs"></a>如何处理事件中心内的指标和诊断日志

将选定的数据流式传输到事件中心后，就离启动高级监视方案更进一步了。 事件中心充当事件管道的前门。 将数据收集到事件中心后，可以使用实时分析提供程序或存储适配器转换和存储这些数据。 事件中心将事件流的生成从这些事件的使用中分离出来。 通过这种方式，事件使用者可以访问自己的计划中的事件。 有关事件中心的详细信息，请参阅：

- [什么是 Azure 事件中心？](../event-hubs/event-hubs-what-is-event-hubs.md)
- [事件中心入门](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

使用在事件中心流式传输的指标可以：

- **将日志流式传输到第三方日志记录和遥测流**。 使用事件中心流式传输，可将指标和诊断日志引入不同的第三方监视。

- **生成自定义遥测和日志记录平台**。 是否已有一个自定义生成的遥测平台，或者正在考虑生成一个？ 可以利用事件中心高度可缩放的发布-订阅功能灵活引入诊断日志。

## <a name="stream-into-storage"></a>流式传输到存储

在 Azure 门户中使用内置的“存档到存储帐户”选项，可以在 Azure 存储中存储 SQL 数据库指标和诊断日志。 此外，还可以通过 PowerShell cmdlet、Azure CLI 或 Azure Monitor REST API 使用诊断设置来启用存储。

### <a name="schema-of-metrics-and-diagnostics-logs-in-the-storage-account"></a>存储帐户中指标和诊断日志的架构

设置指标和诊断日志集合后，当第一行数据可用时，将在你选择的存储帐户中创建一个存储容器。 这些 Blob 的结构为：

```powershell
insights-{metrics|logs}-{category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/ RESOURCEGROUPS/{resource group name}/PROVIDERS/Microsoft.SQL/servers/{resource_server}/ databases/{database_name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

或者使用更简单的形式：

```powershell
insights-{metrics|logs}-{category name}/resourceId=/{resource Id}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

例如，所有指标的 blob 名称可能是：

```powershell
insights-metrics-minute/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.SQL/ servers/Server1/databases/database1/y=2016/m=08/d=22/h=18/m=00/PT1H.json
```

如果存储弹性池中的数据，Blob 名称类似于：

```powershell
insights-{metrics|logs}-{category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/ RESOURCEGROUPS/{resource group name}/PROVIDERS/Microsoft.SQL/servers/{resource_server}/ elasticPools/{elastic_pool_name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

### <a name="download-metrics-and-logs-from-storage"></a>从存储下载指标和日志

了解如何[从存储下载指标和诊断日志](../storage/blobs/storage-quickstart-blobs-dotnet.md#download-the-sample-application)。

## <a name="data-retention-policy"></a>数据保留策略

选择事件中心或存储帐户时，可以指定保留策略。 此策略删除早于选定时间段的数据。

## <a name="all-metrics"></a>所有指标

请参阅下表来详细了解按资源列出的所有指标。

### <a name="all-metrics-for-elastic-pools"></a>弹性池的所有指标

|**资源**|**度量值**|
|---|---|
|弹性池|eDTU 百分比、已用 eDTU、eDTU 限制、CPU 百分比、物理数据读取百分比、日志写入百分比、会话百分比、辅助角色百分比、存储、存储百分比、存储限制、XTP存储百分比 |

### <a name="all-metrics-for-azure-sql-databases"></a>Azure SQL 数据库的所有指标

|**资源**|**度量值**|
|---|---|
|Azure SQL 数据库|DTU 百分比、已用 DTU、DTU 限制、CPU 百分比、物理数据读取百分比、日志写入百分比、成功/失败/防火墙阻止的连接数、会话百分比、辅助角色百分比、存储、存储百分比、XTP 存储百分比和死锁 |

## <a name="logs-for-single-and-pooled-databases"></a>单一数据库和入池数据库的日志

请参阅下面这些表来详细了解 Azure SQL 单一数据库或入池数据库的日志。

### <a name="query-store-runtime-statistics"></a>查询数据存储运行时统计信息

|属性|说明|
|---|---|
|TenantId|租户 ID |
|SourceSystem|始终为：Azure |
|TimeGenerated [UTC]|记录日志时的时间戳 |
|类型|始终为：AzureDiagnostics |
|ResourceProvider|资源提供程序的名称。 始终为：MICROSOFT.SQL |
|类别|类别的名称。 始终为：QueryStoreRuntimeStatistics |
|OperationName|操作的名称。 始终为：QueryStoreRuntimeStatisticsEvent |
|资源|资源名称 |
|ResourceType|资源类型的名称。 始终为：SERVERS/DATABASES |
|SubscriptionId|数据库的订阅 GUID |
|resourceGroup|数据库的资源组名称 |
|LogicalServerName_s|数据库的服务器名称 |
|ElasticPoolName_s|数据库的弹性池（如果有）名称 |
|DatabaseName_s|数据库的名称 |
|ResourceId|资源 URI |
|query_hash_s|查询哈希 |
|query_plan_hash_s|查询计划哈希 |
|statement_sql_handle_s|语句 SQL 句柄 |
|interval_start_time_d|间隔的开始 datetimeoffset，以从 1900-1-1 开始的时钟周期数计 |
|interval_end_time_d|间隔的结束 datetimeoffset，以从 1900-1-1 开始的时钟周期数计 |
|logical_io_writes_d|逻辑 IO 写入总次数 |
|max_logical_io_writes_d|每次执行逻辑 IO 写入的最大次数 |
|physical_io_reads_d|物理 IO 读取总次数 |
|max_physical_io_reads_d|每次执行逻辑 IO 读取的最大次数 |
|logical_io_reads_d|逻辑 IO 读取总次数 |
|max_logical_io_reads_d|每次执行逻辑 IO 读取的最大次数 |
|execution_type_d|执行类型 |
|count_executions_d|执行查询的次数 |
|cpu_time_d|查询使用的总 CPU 时间（以微秒为单位） |
|max_cpu_time_d|单个执行消耗的最大 CPU 时间（以微秒为单位） |
|dop_d|并行度总和 |
|max_dop_d|用于单个执行的最大并行度 |
|rowcount_d|返回的总行数 |
|max_rowcount_d|单个执行中返回的最大行数 |
|query_max_used_memory_d|已使用的内存总量（以 KB 为单位） |
|max_query_max_used_memory_d|单个执行使用的最大内存量（以 KB 为单位） |
|duration_d|总执行时间（以微秒为单位） |
|max_duration_d|单个执行的最大执行时间 |
|num_physical_io_reads_d|总物理读取次数 |
|max_num_physical_io_reads_d|每次执行最大物理读取次数 |
|log_bytes_used_d|使用的日志字节总量 |
|max_log_bytes_used_d|每次执行使用的日志字节最大数量 |
|query_id_d|查询存储中查询的 ID |
|plan_id_d|查询存储中计划的 ID |

详细了解[查询存储运行时统计信息数据](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-runtime-stats-transact-sql)。

### <a name="query-store-wait-statistics"></a>查询存储等待统计信息

|属性|说明|
|---|---|
|TenantId|租户 ID |
|SourceSystem|始终为：Azure |
|TimeGenerated [UTC]|记录日志时的时间戳 |
|类型|始终为：AzureDiagnostics |
|ResourceProvider|资源提供程序的名称。 始终为：MICROSOFT.SQL |
|类别|类别的名称。 始终为：QueryStoreWaitStatistics |
|OperationName|操作的名称。 始终为：QueryStoreWaitStatisticsEvent |
|资源|资源名称 |
|ResourceType|资源类型的名称。 始终为：SERVERS/DATABASES |
|SubscriptionId|数据库的订阅 GUID |
|resourceGroup|数据库的资源组名称 |
|LogicalServerName_s|数据库的服务器名称 |
|ElasticPoolName_s|数据库的弹性池（如果有）名称 |
|DatabaseName_s|数据库的名称 |
|ResourceId|资源 URI |
|wait_category_s|等待的类别 |
|is_parameterizable_s|查询是否可以参数化 |
|statement_type_s|语句的类型 |
|statement_key_hash_s|语句密钥哈希 |
|exec_type_d|执行类型 |
|total_query_wait_time_ms_d|查询特定等待类别的总等待时间 |
|max_query_wait_time_ms_d|单个执行中针对具体等待类别的查询的最大等待时间 |
|query_param_type_d|0 |
|query_hash_s|查询存储中的查询哈希 |
|query_plan_hash_s|查询存储中的查询计划哈希。 |
|statement_sql_handle_s|查询存储中的语句句柄 |
|interval_start_time_d|间隔的开始 datetimeoffset，以从 1900-1-1 开始的时钟周期数计 |
|interval_end_time_d|间隔的结束 datetimeoffset，以从 1900-1-1 开始的时钟周期数计 |
|count_executions_d|执行查询的次数 |
|query_id_d|查询存储中查询的 ID |
|plan_id_d|查询存储中计划的 ID |

详细了解[查询存储等待统计数据](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql)。

### <a name="errors-dataset"></a>错误数据集

|属性|说明|
|---|---|
|TenantId|租户 ID |
|SourceSystem|始终为：Azure |
|TimeGenerated [UTC]|记录日志时的时间戳 |
|类型|始终为：AzureDiagnostics |
|ResourceProvider|资源提供程序的名称。 始终为：MICROSOFT.SQ |
|类别|类别的名称。 始终为：错误 |
|OperationName|操作的名称。 始终为：ErrorEvent |
|资源|资源名称 |
|ResourceType|资源类型的名称。 始终为：SERVERS/DATABASES |
|SubscriptionId|数据库的订阅 GUID |
|resourceGroup|数据库的资源组名称 |
|LogicalServerName_s|数据库的服务器名称 |
|ElasticPoolName_s|数据库的弹性池（如果有）名称 |
|DatabaseName_s|数据库的名称 |
|ResourceId|资源 URI |
|消息|纯文本格式的错误消息 |
|user_defined_b|是否是用户定义位错误 |
|error_number_d|错误代码 |
|严重性|错误的严重性 |
|state_d|错误的状态 |
|query_hash_s|失败查询的查询哈希（如果有） |
|query_plan_hash_s|失败查询的查询计划哈希（如果有） |

详细了解 [SQL Server 错误消息](https://msdn.microsoft.com/library/cc645603.aspx)。

### <a name="database-wait-statistics-dataset"></a>数据库等待统计数据集

|属性|说明|
|---|---|
|TenantId|租户 ID |
|SourceSystem|始终为：Azure |
|TimeGenerated [UTC]|记录日志时的时间戳 |
|类型|始终为：AzureDiagnostics |
|ResourceProvider|资源提供程序的名称。 始终为：MICROSOFT.SQL |
|类别|类别的名称。 始终为：DatabaseWaitStatistics |
|OperationName|操作的名称。 始终为：DatabaseWaitStatisticsEvent |
|资源|资源名称 |
|ResourceType|资源类型的名称。 始终为：SERVERS/DATABASES |
|SubscriptionId|数据库的订阅 GUID |
|resourceGroup|数据库的资源组名称 |
|LogicalServerName_s|数据库的服务器名称 |
|ElasticPoolName_s|数据库的弹性池（如果有）名称 |
|DatabaseName_s|数据库的名称 |
|ResourceId|资源 URI |
|wait_type_s|等待类型的名称 |
|start_utc_date_t [UTC]|测量周期开始时间 |
|end_utc_date_t [UTC]|测量周期结束时间 |
|delta_max_wait_time_ms_d|每次执行最大等待时间 |
|delta_signal_wait_time_ms_d|总信号等待时间 |
|delta_wait_time_ms_d|期间内的总等待时间 |
|delta_waiting_tasks_count_d|等待任务数 |

详细了解[数据库等待统计信息](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql)。

### <a name="time-outs-dataset"></a>超时数据集

|属性|说明|
|---|---|
|TenantId|租户 ID |
|SourceSystem|始终为：Azure |
|TimeGenerated [UTC]|记录日志时的时间戳 |
|类型|始终为：AzureDiagnostics |
|ResourceProvider|资源提供程序的名称。 始终为：MICROSOFT.SQL |
|类别|类别的名称。 始终为：超时 |
|OperationName|操作的名称。 始终为：TimeoutEvent |
|资源|资源名称 |
|ResourceType|资源类型的名称。 始终为：SERVERS/DATABASES |
|SubscriptionId|数据库的订阅 GUID |
|resourceGroup|数据库的资源组名称 |
|LogicalServerName_s|数据库的服务器名称 |
|ElasticPoolName_s|数据库的弹性池（如果有）名称 |
|DatabaseName_s|数据库的名称 |
|ResourceId|资源 URI |
|error_state_d|错误状态代码 |
|query_hash_s|查询哈希（如果有） |
|query_plan_hash_s|查询计划哈希（如果有） |

### <a name="blockings-dataset"></a>阻塞数据集

|属性|说明|
|---|---|
|TenantId|租户 ID |
|SourceSystem|始终为：Azure |
|TimeGenerated [UTC]|记录日志时的时间戳 |
|类型|始终为：AzureDiagnostics |
|ResourceProvider|资源提供程序的名称。 始终为：MICROSOFT.SQL |
|类别|类别的名称。 始终为：块 |
|OperationName|操作的名称。 始终为：BlockEvent |
|资源|资源名称 |
|ResourceType|资源类型的名称。 始终为：SERVERS/DATABASES |
|SubscriptionId|数据库的订阅 GUID |
|resourceGroup|数据库的资源组名称 |
|LogicalServerName_s|数据库的服务器名称 |
|ElasticPoolName_s|数据库的弹性池（如果有）名称 |
|DatabaseName_s|数据库的名称 |
|ResourceId|资源 URI |
|lock_mode_s|查询所使用的锁模式 |
|resource_owner_type_s|锁的所有者 |
|blocked_process_filtered_s|阻塞进程报告 XML |
|duration_d|锁定持续时间（以毫秒为单位） |

### <a name="deadlocks-dataset"></a>死锁数据集

|属性|说明|
|---|---|
|TenantId|租户 ID |
|SourceSystem|始终为：Azure |
|TimeGenerated [UTC] |记录日志时的时间戳 |
|类型|始终为：AzureDiagnostics |
|ResourceProvider|资源提供程序的名称。 始终为：MICROSOFT.SQL |
|类别|类别的名称。 始终为：死锁数 |
|OperationName|操作的名称。 始终为：DeadlockEvent |
|资源|资源名称 |
|ResourceType|资源类型的名称。 始终为：SERVERS/DATABASES |
|SubscriptionId|数据库的订阅 GUID |
|resourceGroup|数据库的资源组名称 |
|LogicalServerName_s|数据库的服务器名称 |
|ElasticPoolName_s|数据库的弹性池（如果有）名称 |
|DatabaseName_s|数据库的名称 |
|ResourceId|资源 URI |
|deadlock_xml_s|死锁报告 XML |

### <a name="automatic-tuning-dataset"></a>自动优化数据集

|属性|说明|
|---|---|
|TenantId|租户 ID |
|SourceSystem|始终为：Azure |
|TimeGenerated [UTC]|记录日志时的时间戳 |
|类型|始终为：AzureDiagnostics |
|ResourceProvider|资源提供程序的名称。 始终为：MICROSOFT.SQL |
|类别|类别的名称。 始终为：AutomaticTuning |
|资源|资源名称 |
|ResourceType|资源类型的名称。 始终为：SERVERS/DATABASES |
|SubscriptionId|数据库的订阅 GUID |
|resourceGroup|数据库的资源组名称 |
|LogicalServerName_s|数据库的服务器名称 |
|LogicalDatabaseName_s|数据库的名称 |
|ElasticPoolName_s|数据库的弹性池（如果有）名称 |
|DatabaseName_s|数据库的名称 |
|ResourceId|资源 URI |
|RecommendationHash_s|自动优化建议的唯一哈希值 |
|OptionName_s|自动优化操作 |
|Schema_s|数据库架构 |
|Table_s|受影响的表 |
|IndexName_s|索引名称 |
|IndexColumns_s|列名称 |
|IncludedColumns_s|包括的列 |
|EstimatedImpact_s|自动优化建议 JSON 的估计影响 |
|Event_s|自动优化事件的类型 |
|Timestamp_t|上次更新时间戳 |

### <a name="intelligent-insights-dataset"></a>智能见解数据集

详细了解 [Intelligent Insights 日志格式](sql-database-intelligent-insights-use-diagnostics-log.md)。

## <a name="next-steps"></a>后续步骤

若要了解如何启用日志记录并了解各种 Azure 服务支持的指标和日志类别，请参阅：

- [Microsoft Azure 中的指标概述](../monitoring-and-diagnostics/monitoring-overview-metrics.md)
- [Azure 诊断日志概述](../azure-monitor/platform/diagnostic-logs-overview.md)

若要了解事件中心，请阅读以下主题：

- [什么是 Azure 事件中心？](../event-hubs/event-hubs-what-is-event-hubs.md)
- [事件中心入门](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

若要详细了解 Azure 存储，请参阅[如何从存储下载指标和诊断日志](../storage/blobs/storage-quickstart-blobs-dotnet.md#download-the-sample-application)。
