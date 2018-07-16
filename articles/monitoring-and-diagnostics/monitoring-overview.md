---
title: 监视 Azure 应用程序和资源
description: 适用于 Azure 服务和应用程序的完整监视策略所涉及的 Microsoft 服务及功能的概述。
author: robb
manager: carmonm
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 1b962c74-8d36-4778-b816-a893f738f92d
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 03/05/2018
ms.author: v-yiso
ms.date: 07/23/2018
ms.openlocfilehash: 37ae7208f8ec22da0b16276c0c267a0bac5e00d4
ms.sourcegitcommit: 479954e938e4e3469d6998733aa797826e4f300b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/13/2018
ms.locfileid: "39031747"
---
# <a name="monitoring-azure-applications-and-resources"></a>监视 Azure 应用程序和资源

监视是一种数据收集和分析操作，用于确定商业应用程序及其所依赖资源的性能、运行状况和可用性。 有效的监视策略有助于了解应用程序组件的详细运行状况， 并且还可以主动向你发送关键情况的通知，让你在这些情况成为问题之前解决它们，从而提高运行时间。

Azure 包括多项可以在监视空间单独执行特定角色或任务的服务。 这些服务组合在一起就可以提供一个全面的解决方案，用于从应用程序以及支持应用程序的 Azure 资源收集遥测数据，对这些数据进行分析并执行操作。 这些服务还监视关键的本地资源，以提供混合监视环境。 为应用程序开发完整监视策略的第一步是了解可用的工具和数据。 

下图显示的是这些组件的概念视图，它们共同作用，对 Azure 资源进行监视。 以下部分介绍这些组件，并提供了详细技术信息的链接。

![监视概述](media/monitoring-overview/monitoring-products-overview.png)


## <a name="shared-capabilities"></a>共享功能
核心监视服务与深度监视服务共享功能，从而提供以下功能。 

### <a name="alerts"></a>警报
[Azure 警报](../monitoring-and-diagnostics/monitoring-overview-alerts.md)会主动发送有关关键情况的通知，并可能采取纠正措施。 警报规则可使用来自多个源（包括指标和日志）的数据。 它们使用[操作组](../monitoring-and-diagnostics/monitoring-action-groups.md)，其中包含唯一的收件人集合，以及用于响应警报的操作。 根据自身需求，可使用 Webhook 让警报启动外部操作，并将其与 ITSM 工具集成。

### <a name="dashboards"></a>仪表板
可以使用 [Azure 仪表板](../azure-portal/azure-portal-dashboards.md)，将不同类型的数据合并到 [Azure 门户](https://portal.azure.cn)的单个窗格中， 然后将仪表板与其他 Azure 用户共享。 

例如，可以创建一个组合了以下元素的仪表板：
- 用于显示指标图的磁贴
- 包含活动日志的表
- 由 Application Insights 提供的使用情况图表
- Log Analytics 中的日志搜索的输出

还可以将 Log Analytics 数据导出到 [Power BI](/power-bi/)。 可以在其中利用其他可视化效果。 也可将数据提供给组织内外的其他人。

### <a name="metrics-explorer"></a>指标资源管理器
[指标](../monitoring-and-diagnostics/monitoring-overview-metrics.md)是由 Azure 资源生成的数值，用于了解资源的操作情况和性能。 可以通过指标资源管理器向 Log Analytics 发送指标，以便使用来自其他源的数据进行分析。


## <a name="core-monitoring"></a>核心监视
核心监视提供对 Azure 资源的基本必要的监视。 这些服务有一个最低配置要求，并收集高级监视服务所使用的核心遥测数据。    

### <a name="azure-monitor"></a>Azure Monitor
使用 [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md) 可收集[指标](../monitoring-and-diagnostics/monitoring-overview-metrics.md)、[活动日志](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)和[诊断日志](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)，为 Azure 服务启用核心监视。 例如，可以通过活动日志了解新资源的创建或修改时间。 

可通过指标获取不同资源（甚至包括虚拟机中的操作系统）的性能统计信息。 可以使用 Azure 门户中的某个资源管理器查看此数据，还可以基于这些指标创建警报。 Azure Monitor 提供最快的指标管道（5 分钟乃至 1 分钟），因此应将其用于时间关键型警报和通知。 
### <a name="azure-advisor"></a>Azure 顾问
[Azure 顾问](../advisor/advisor-overview.md)可持续监视资源配置和使用情况遥测。 然后，它会根据最佳做法提供个性化的建议。 采纳这些建议有助于改善支持应用程序的资源的性能、安全性和可用性。

### <a name="service-health"></a>服务运行状况
应用程序的运行状况取决于其所依赖的 Azure 服务。 [Azure 服务运行状况](../service-health/service-health-overview.md)可以标识 Azure 服务存在的可能影响应用程序的任何问题。 服务运行状况还有助于对计划性维护进行计划。

### <a name="activity-log"></a>活动日志
[活动日志](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)提供有关 Azure 资源操作的数据。 此信息包括：
- 对资源进行的配置更改。
- 服务运行状况事件。
- 关于如何更好地利用资源的建议。
- 与自动缩放操作相关的信息。 

在 Azure 门户中，可以在特定资源的页面上查看该资源的日志。 也可在活动日志资源管理器中查看多个资源提供的日志。 
## <a name="deep-monitoring-services"></a>深度监视服务
下列 Azure 服务提供丰富的功能，用于在更深的层次收集和分析监视数据。 这些服务基于核心监视功能构建，并可利用 Azure 中的常用功能。 它们可以对收集的数据进行深入的分析，并提供有关应用程序和基础结构的独特见解。 它们在面向不同受众的方案上下文中呈现数据。

## <a name="deep-infrastructure-monitoring"></a>深度基础结构监视
### <a name="network-monitoring"></a>网络监视
有几种工具可协同工作监视网络（无论在 Azure 中还是在本地）的各个方面。  

[网络观察程序](../network-watcher/network-watcher-monitoring-overview.md)为 Azure 中的不同网络方案提供基于方案的监视和诊断。 它将数据存储在 Azure 指标和诊断中，供将来进行分析。 它可以与以下解决方案配合使用，监视网络的各个方面。

[网络性能监视器 (NPM)](https://blogs.msdn.microsoft.com/azuregov/2017/09/05/network-performance-monitor-general-availability/) 是一种基于云的网络监视解决方案，用于监视公有云、数据中心和本地环境之间的连接。

[ExpressRoute 监视器](https://azure.microsoft.com/en-in/blog/monitoring-of-azure-expressroute-in-preview/)是一种 NPM 功能，用于通过 Azure ExpressRoute 线路监视端到端连接和性能。

## <a name="example-scenarios"></a>示例方案
以下是高级示例，介绍如何在 Azure 中针对不同的方案使用相应的监视工具。

### <a name="monitoring-a-web-application"></a>监视 Web 应用程序
可设想一个通过 Azure 应用服务、Azure 存储 和 SQL 数据库在 Azure 中部署的 Web 应用程序。 首先，可在 Azure 门户中找到这些资源的相应页面，然后访问它们的[指标](../monitoring-and-diagnostics/monitoring-overview-metrics.md)和[活动日志](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)。 请查找关键信息，例如向应用程序发出的请求数和平均响应时间。 另请标识任何配置更改。

然后，请转到门户中的“监视”，同时查看不同资源的指标和日志。 确定这些指标的标准参数以后，请[创建警报规则](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md)。 当遇到异常情况时（例如，平均响应时间超出阈值），这些规则会主动通知你。 为快速一览应用程序的每日性能，请创建 Azure 仪表板，以便显示代表关键 KPI 的指标图。




### <a name="monitoring-virtual-machines"></a>监视虚拟机
Azure 中同时运行有 Windows 和 Linux 虚拟机。 可以使用 Azure Monitor 来查看[活动日志](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)和[主机级别指标](../monitoring-and-diagnostics/monitoring-overview-metrics.md)。 可以向虚拟机添加 [Azure 诊断扩展](../virtual-machines/linux/tutorial-monitoring.md#install-diagnostics-extension)，以便从来宾操作系统收集指标。 然后创建[警报规则](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md)，以便在基本指标（如处理器利用率和内存）超过阈值时，主动发送通知。





## <a name="next-steps"></a>后续步骤
详细了解以下内容

* [Azure Monitor](/monitoring-and-diagnostics/)，以便开始使用核心监视指标和警报。
