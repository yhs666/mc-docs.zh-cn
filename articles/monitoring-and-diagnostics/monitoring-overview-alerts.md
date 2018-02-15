---
title: "Azure 和 Azure Monitor 中的警报概述"
description: "使用警报可以监视 Azure 资源指标、事件或日志，并在符合指定的条件时接收通知。"
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: a6dea224-57bf-43d8-a292-06523037d70b
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 08/02/2017
ms.date: 02/26/2018
ms.author: v-yiso
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c594d179d5fe9d97497319c84c642e46f480f934
ms.sourcegitcommit: 3629fd4a81f66a7d87a4daa00471042d1f79c8bb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2018
---
# <a name="what-are-alerts-in-azure"></a>Azure 中的警报是什么？
本文介绍了 Azure 中的各种警报来源、这些警报的目的、警报优点以及警报的使用方法。 本文特别适用于 Azure Monitor，但也提供了其他带有警报服务的链接。 警报提供了一种在 Azure 中监视的方法，允许配置数据条件，并在条件与最新监视数据匹配时发出通知。


## <a name="taxonomy-of-azure-alerts"></a>Azure 警报的分类
Azure 使用以下术语来描述警报和及其功能：
* **警报** - 符合标准（一个或多个规则或条件）时被激活的定义。
* **活动** - 满足警报定义的标准时的状态。
* **已解决** - 在先前满足警报定义的标准后不再满足该标准的状态。
* **通知** - 警报变为活动状态时采取的操作。
* **操作** - 发送给通知接收方的特定通话（例如，通过电子邮件发送地址或发布到 Webhook URL）。 通知通常可以触发多个操作。

    > [!NOTE]
    > 作为 Azure 中警报进化的一部分，在预览版中提供了新的统一体验。 新的“警报(预览)”体验使用不同的分类。 详细了解[警报（预览）](monitoring-overview-unified-alerts.md)。 
    >

## <a name="alerts-in-different-azure-services"></a>不同 Azure 服务中的警报
警报可在多个 Azure 监视服务中使用。 有关如何以及何时使用这些服务的信息，请参阅[此文](./monitoring-overview.md)。 以下是 Azure 中可用的警报类型的明细：


| 服务 | 警报类型 | 支持的服务 | 说明 |
|---|---|---|---|
| Azure Monitor | [指标警报](./insights-alerts-portal.md) | [Azure Monitor 中支持的指标](./monitoring-supported-metrics.md) | 任何平台级指标满足特定条件时（例如，VM 上的 CPU % 在过去 5 分钟内大于 90），就会收到通知。 |
|Azure Monitor | [准实时指标警报（预览版）](./monitoring-near-real-time-metric-alerts.md)| [Azure Monitor 中支持的资源](./monitoring-near-real-time-metric-alerts.md#what-resources-can-i-create-near-real-time-metric-alerts-for) | 当一个或多个平台级指标满足指定条件时（例如，VM 的 CPU 百分比大于 90 并且过去 5 分钟网络传入大于 500 MB ），就会收到通知，速度比指标警报更快。 |
| Azure Monitor | [活动日志警报](./monitoring-activity-log-alerts.md) | Azure 资源管理器中可用的所有资源类型 | 当 [Azure 活动日记](./monitoring-overview-activity-logs.md)中的任何新事件符合特定条件时（例如，myProductionResourceGroup 中出现“删除 VM”操作或出现状态为“Active”的新服务运行状况事件时）就会收到通知。 |

## <a name="alerts-on-azure-monitor-data"></a>关于 Azure Monitor 数据的警报
Azure Monitor 提供的数据有三种类型的警报 - 指标警报、准实时指标警报（预览版）和活动日志警报。

* **指标警报** - 当指定的指标值越过了分配的阈值时，就会触发此警报。 当警报“激活”（当阈值越过并满足警报条件时）以及“已解决”（当阈值再次超过并且不再满足条件）时，警报将生成通知。 有关 Azure Monitor 支持的不断增长的可用指标列表，请参阅 [Azure Monitor 支持的指标列表](monitoring-supported-metrics.md)。
* 准实时指标警报（预览版） - 这些警报类似于指标警报，但在以下几个方面有所不同。 首先，顾名思义，这些警报可准实时触发（最快 1 分钟）。 它们还支持监视多个（目前两个）指标。  当警报“激活”（同时越过每个指标的阈值并满足警报条件时）以及“已解决”（至少一个指标再次超过阈值并且不再满足条件）时，警报将生成通知。

    > [!NOTE]
    > 准实时指标警报目前以公开预览版提供。 功能和用户体验可能会发生变化。
    >
    >

* **活动日志警报** - 当生成与分配的筛选器条件匹配的活动日志事件时，触发的流式处理日志警报。 这些警报只有“已激活”这一个状态，因为警报引擎只需将筛选器条件应用到任何新事件。 出现新的服务运行状况事件时，或用户或应用程序在订阅中执行操作（例如“删除虚拟机”）时，可以使用这些警报通知。

对于通过 Azure Monitor 提供的诊断日志数据，建议将数据路由到 Log Analytics 并使用 Log Analytics 警报。 下图总结了 Azure Monitor 中的数据源，从概念上总结了从数据取消警报的方法。

![警报介绍](./media/monitoring-overview-alerts/Alerts_Overview_Resource_v4.png)

## <a name="how-do-i-receive-a-notification-on-an-azure-monitor-alert"></a>如何实现在 Azure Monitor 警报中接收通知？
从历史上来看，来自不同服务的 Azure 警报使用自己的内置通知方法。 今后，Azure Monitor 提供了一个名为“操作组”的可重复使用的通知组。 操作组指定一组通知接收方（任意数量的电子邮件地址、用于接收短信的电话号码或 Webhook URL），任何时间引用操作组时都会激活警报，所有接收方都会收到该通知。 可在多个警报对象中重复使用一组接收方（例如，在线工程师列表）。 目前，只有活动日志警报可以使用操作组，但还有几个其他 Azure 警报类型也在研究如何使用操作组。

操作组除通过电子邮件地址和短信号码通知外，还可发布到 Webhook URL 来支持通知。 这可实现自动化和修复，例如使用：
    - Azure 自动化 Runbook
    - 第三方服务

准实时指标警报（预览版）和活动日志警报使用操作组。

指标警报尚不可使用操作组。 在单个指标警报中，可将通知配置为：
* 将电子邮件通知发送到服务管理员、共同管理员或指定的其他电子邮件。
* 调用 Webhook，以便用户启动其他自动化操作

## <a name="next-steps"></a>后续步骤
了解警报规则以及如何使用以下工具来配置这些规则：

* 详细了解[指标](monitoring-overview-metrics.md)
* [通过 Azure 门户配置指标警报](insights-alerts-portal.md)
* 配置[指标警报 PowerShell](insights-alerts-powershell.md)
* 配置[指标警报命令行接口 (CLI)](insights-alerts-command-line-interface.md)
* 配置[指标警报 Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931945.aspx)
* 详细了解[活动日志](monitoring-overview-activity-logs.md)
* [通过 Azure 门户配置活动日志警报](monitoring-activity-log-alerts.md)
* [通过 Resource Manager 配置活动日志警报](monitoring-create-activity-log-alerts-with-resource-manager-template.md)
* 查看[活动日志警报 webhook 架构](monitoring-activity-log-alerts-webhook.md)
* 详细了解[准实时指标警报](monitoring-near-real-time-metric-alerts.md)
* 详细了解[服务通知](monitoring-service-notifications.md)
* 详细了解[操作组](monitoring-action-groups.md)
* 配置[通过警报（预览）发出警报](monitor-alerts-unified-usage.md)
