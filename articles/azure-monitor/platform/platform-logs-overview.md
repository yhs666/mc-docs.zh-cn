---
title: Azure 平台日志概述 | Microsoft Docs
description: Azure 中的诊断日志概述提供了有关 Azure 资源操作的丰富、频繁的数据。
author: lingliw
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
origin.date: 09/20/2019
ms.date: 10/28/2019
ms.author: v-lingwu
ms.subservice: logs
ms.openlocfilehash: a25e83a00824d7ce097cbfab749fca0e27f3269e
ms.sourcegitcommit: b09d4b056ac695ba379119eb9e458a945b0a61d9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/28/2019
ms.locfileid: "72971133"
---
# <a name="overview-of-azure-platform-logs"></a>Azure 平台日志概述
平台日志提供 Azure 资源及其所依赖的 Azure 平台的详细诊断和审核信息。 它们是自动生成的，虽然你需要配置某些平台日志，以便将其转发到一个或多个目标进行保留。 本文概述了平台日志，其中包括它们提供什么信息，以及如何配置它们以方便收集和分析。

## <a name="types-of-platform-logs"></a>平台日志的类型
下表列出了在不同 Azure 层提供的特定的平台日志。

| 层 | 日志 | 说明 |
|:---|:---|:---|
| Azure 资源 | [资源日志](resource-logs-overview.md) | 深入了解在 Azure 资源（数据平面）内执行的操作，例如，从 Key Vault 获取机密，或向数据库发出请求。  资源日志的内容因 Azure 服务和资源类型而异。<br>资源日志以前称为诊断日志。   |
| Azure 订阅 | [活动日志](activity-logs-overview.md) | 了解从外部（管理平台）  对订阅中的每个 Azure 资源执行的操作，以及对服务运行状况事件进行的更新。 每个 Azure 订阅都有一个活动日志。   |
| Azure 租户 | [Azure Active Directory 日志](../../active-directory/reports-monitoring/overview-reports.md)  | 包含特定租户的 Azure Active Directory 中的登录活动和更改审核日志的历史记录。   |


![平台日志概述](media/platform-logs-overview/logs-overview.png)

## <a name="viewing-platform-logs"></a>查看平台日志
可以在 Azure 门户中查看[活动日志](activity-log-view.md)和 [Azure Active Directory 日志](../../active-directory/reports-monitoring/overview-reports.md)。 必须将资源日志发送到[目标](#destinations)才能查看它们。


## <a name="destinations"></a>Destinations
可以将平台日志发送到下表中的一个或多个目标，具体取决于监视要求。 

| 目标 | 方案 | 参考 |
|:---|:---|:---|:---|
| Log Analytics 工作区 | 借助其他监视数据对日志进行分析，并利用 Azure Monitor 功能（例如日志查询和警报）。 | [资源日志](resource-logs-collect-storage.md)<br>[活动日志](activity-log-collect.md)<br> |
| Azure 存储 | 将日志存档供审核、静态分析或备份。 |[资源日志](resource-logs-collect-storage.md)<br>[活动日志](activity-log-export.md)<br> |
| 事件中心 | 将日志流式传输到第三方日志记录和遥测系统。  |[资源日志](resource-logs-stream-event-hubs.md)<br>[活动日志](activity-log-export.md)<br>|


## <a name="diagnostic-settings-and-log-profiles"></a>诊断设置和日志配置文件
通过[创建诊断设置](diagnostic-settings.md)，配置资源日志和 Azure Active Directory 日志的目标。 创建活动日志的目标，方法是：[创建日志配置文件](activity-log-export.md)或[将其连接到 Log Analytics 工作区](activity-log-collect.md)。

诊断设置和日志配置文件定义以下属性：

- 要将所选日志和指标发送到其中的一个或多个目标。
- 资源中的哪些日志类别和指标发送到了目标。
- 如果将某个存储帐户选作目标，则每个日志类别应保留多长时间？



## <a name="next-steps"></a>后续步骤

* [详细了解活动日志](activity-logs-overview.md)
* [详细了解资源日志](resource-logs-overview.md)
* [查看不同 Azure 服务的资源日志架构](diagnostic-logs-schema.md)
