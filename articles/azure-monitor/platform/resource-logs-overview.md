---
title: Azure 资源日志概述 | Microsoft Docs
description: 了解 Azure 资源日志支持的服务和事件架构。
ms.service: azure-monitor
ms.subservice: logs
ms.topic: reference
author: lingliw
ms.author: v-lingwu
origin.date: 09/20/2019
ms.date: 10/20/2019
ms.openlocfilehash: 3f75b50cdd6b153155d1b7c13904f159f543a95f
ms.sourcegitcommit: b09d4b056ac695ba379119eb9e458a945b0a61d9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/28/2019
ms.locfileid: "72971149"
---
# <a name="azure-resource-logs-overview"></a>Azure 资源日志概述
Azure 资源日志是 Azure 资源发出的描述内部操作的[平台日志](platform-logs-overview.md)。 所有资源日志共享通用的顶级架构，且每个服务都能灵活地为各自的事件发出唯一属性。

> [!NOTE]
> 资源日志以前称为诊断日志。

## <a name="collecting-resource-logs"></a>收集资源日志
资源日志由支持的 Azure 资源自动生成，但除非你通过[诊断设置](diagnostic-settings.md)配置它们，否则系统不会收集它们。 请为每个 Azure 资源创建诊断设置，以便将这些日志转发到以下目标：

| 目标 | 方案 |
|:---|:---|:---|
| [Log Analytics 工作区](resource-logs-collect-storage.md) | 借助其他监视数据对日志进行分析，并利用 Azure Monitor 功能（例如日志查询和日志警报）。 |
| [Azure 存储](resource-logs-collect-storage.md) | 将日志存档供审核或备份。 |
| [事件中心](resource-logs-stream-event-hubs.md) | 将日志流式传输到第三方日志记录和遥测系统。  |

## <a name="compute-resources"></a>计算资源
资源日志不同于 Azure 计算资源中的来宾 OS 级别日志。 计算资源需要通过代理从其来宾 OS 中收集日志和指标，包括事件日志、syslog 和性能计数器之类的数据。 请使用[诊断扩展](agents-overview.md#azure-diagnostic-extension)路由 Azure 虚拟机中的日志数据，使用 [Log Analytics 代理](agents-overview.md#log-analytics-agent)将 Azure 中的虚拟机、其他云中的虚拟机和本地的虚拟机中的日志和指标收集到 Log Analytics 工作区。 有关详细信息，请参阅 [Azure Monitor 的监视数据源](data-sources.md)。

## <a name="resource-logs-schema"></a>资源日志架构
将资源日志写入到目标之一时，每项 Azure 服务都有自己的架构，但它们都共享下表中的顶级架构： 请参阅下面的[特定于服务的架构](#service-specific-schemas)，获取每项服务的架构的链接。 

| Name | 必需/可选 | 说明 |
|---|---|---|
| time | 必须 | 事件时间戳 (UTC)。 |
| ResourceId | 必须 | 发出事件的资源的资源 ID。 对于租户服务，其形式为 /tenants/tenant-id/providers/provider-name。 |
| tenantId | 对于租户日志而言是必需的 | 此事件关联到的 Active Directory 租户的租户 ID。 此属性仅用于租户级日志，它不会出现在资源级日志中。 |
| operationName | 必须 | 此事件表示的操作的名称。 如果该事件表示 RBAC 操作，则这是 RBAC 操作名称 （例如 Microsoft.Storage/storageAccounts/blobServices/blobs/Read）。 通常以资源管理器操作的形式建模，即使它们不是实际记录的资源管理器操作 (`Microsoft.<providerName>/<resourceType>/<subtype>/<Write/Read/Delete/Action>`) |
| operationVersion | 可选 | 如果使用 API 执行 operationName，则 api-version 与该操作关联（例如 `http://myservice.windowsazure.net/object?api-version=2016-06-01`）。 如果没有与此操作相对应的 API，则该版本表示该操作的版本，以防与操作相关联的属性在将来发生更改。 |
| category | 必须 | 事件的日志类别。 类别是可以在特定资源上启用或禁用日志的粒度。 在事件的属性 blob 内显示的属性在特定日志类别和资源类型中相同。 典型的日志类别是“Audit”、“Operational”、“Execution”和“Request”。 |
| resultType | 可选 | 事件的状态。 典型值包括“Started”、“In Progress”、“Succeeded”、“Failed”、“Active”和“Resolved”。 |
| resultSignature | 可选 | 事件的子状态。 如果此操作对应于 REST API 调用，则这是相应 REST 调用的 HTTP 状态代码。 |
| resultDescription | 可选 | 此操作的静态文本说明，例如 “获取存储文件”。 |
| durationMs | 可选 | 操作持续时间，以毫秒为单位。 |
| callerIpAddress | 可选 | 调用方 IP 地址，如果该操作对应于来自具有公开 IP 地址的实体的 API 调用。 |
| correlationId | 可选 | 用于将一组相关事件组合在一起的 GUID。 通常情况下，如果两个事件具有相同 operationName，但具有两个不同状态（例如 “Started”和“Succeeded”），则它们共享相同的关联 ID。 这也可以代表事件之间的其他关系。 |
| identity | 可选 | 描述执行操作的用户或应用程序的标识的 JSON Blob。 通常，这将包括 Active Directory 中的授权和声明/JWT 令牌。 |
| Level | 可选 | 事件的严重级别。 必须是信息性、警告、错误或严重。 |
| location | 可选 | 发出事件的资源区域，例如 “美国东部”或“法国南部” |
| properties | 可选 | 与此特定类别的事件相关的任何扩展属性。 所有自定义/唯一属性都必须放入此架构的“B 部分”。 |

## <a name="service-specific-schemas"></a>特定于服务的架构
资源诊断日志的架构因 `resourceId` 属性和 `category` 属性所定义的资源类型而异。 下面这个列表显示了支持资源日志的所有 Azure 服务，并提供了服务和特定于类别的架构（如果可用）的链接。

| 服务 | 架构和文档 |
| --- | --- |
| Analysis Services | https://azure.microsoft.com/blog/azure-analysis-services-integration-with-azure-diagnostic-logs/ |
| API 管理 | [API 管理诊断日志](../../api-management/api-management-howto-use-azure-monitor.md#diagnostic-logs) |
| 应用程序网关 |[应用程序网关的诊断日志记录](../../application-gateway/application-gateway-diagnostics.md) |
| Azure 自动化 |[适用于 Azure 自动化的 Log Analytics](../../automation/automation-manage-send-joblogs-log-analytics.md) |
| Azure Batch |[Azure Batch 诊断日志记录](../../batch/batch-diagnostics.md) |
| Azure Database for MySQL | [Azure Database for MySQL 诊断日志](../../mysql/concepts-server-logs.md#diagnostic-logs) |
| Azure Database for PostgreSQL | [Azure Database for PostgreSQL 诊断日志](../../postgresql/concepts-server-logs.md#diagnostic-logs) |
| 认知服务 | [Azure 认知服务的诊断日志记录](../../cognitive-services/diagnostic-logging.md) |
| CosmosDB | [Azure Cosmos DB 日志记录](../../cosmos-db/logging.md) |
| Express Route | 架构不可用。 |
| Azure 防火墙 | 架构不可用。 |
| IoT 中心 | [IoT 中心操作](../../iot-hub/iot-hub-monitor-resource-health.md#use-azure-monitor) |
| 网络安全组 |[网络安全组 (NSG) 的 Log Analytics](../../virtual-network/virtual-network-nsg-manage-log.md) |
| Power BI 专用 | [Azure 中 Power BI Embedded 的诊断日志记录](https://docs.microsoft.com/power-bi/developer/azure-pbie-diag-logs) |
| 恢复服务 | [Azure 备份的数据模型](../../backup/backup-azure-reports-data-model.md)|
| 搜索 |[允许并使用搜索流量分析](../../search/search-traffic-analytics.md) |
| 服务总线 |[Azure 服务总线诊断日志](../../service-bus-messaging/service-bus-diagnostic-logs.md) |
| SQL 数据库 | [Azure SQL 数据库诊断日志记录](../../sql-database/sql-database-metrics-diag-logging.md) |
| 流分析 |[作业诊断日志](../../stream-analytics/stream-analytics-job-diagnostic-logs.md) |
| 虚拟网络 | 架构不可用。 |
| 虚拟网络网关 | 架构不可用。 |

## <a name="next-steps"></a>后续步骤

* [详细了解其他 Azure 平台日志](platform-logs-overview.md)，这些日志可以用来监视 Azure。
