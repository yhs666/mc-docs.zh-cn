---
title: "监视 Azure 应用程序和资源"
description: "适用于 Azure 服务和应用程序的完整监视策略所涉及的各种 Microsoft 服务及功能的概述。"
author: robb
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 1b962c74-8d36-4778-b816-a893f738f92d
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 01/30/2018
ms.author: v-yiso
ms.date: 03/19/2018
ms.openlocfilehash: 40148c489d0356ffda3bfd4674fa21497bd3ad17
ms.sourcegitcommit: ad7accbbd1bc7ce0aeb2b58ce9013b7cafa4668b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/12/2018
---
# <a name="monitoring-azure-applications-and-resources"></a>监视 Azure 应用程序和资源

监视是一种数据收集和分析操作，用于确定商业应用程序的性能、运行状况及应用程序和其所依赖的资源的可用性。 通过实施有效的监视策略，可了解应用程序中不同组件的具体操作情况，通过主动发出有关关键问题的通知，可帮助在问题发生前解决问题，从而延长运行时间。

Azure 包含多个服务，它们在监视空间中各自执行特定角色和任务，并共同提供有关收集、分析和操作来自应用程序的遥测数据和支持它们的基础 Azure 资源的综合解决方案。  这些服务还监视关键的本地资源，以提供混合监视环境。   为应用程序开发完整监视策略的第一步是了解可用的工具和数据。 

下图显示的是这些不同组件的概念视图，它们共同提供对 Azure 资源的监视。  以下各部分对各组件进行了介绍，并附有相关详细技术信息的链接。

![监视概述](media/monitoring-overview/overview.png)

## <a name="basic-monitoring"></a>基本监视
基本监视提供对 Azure 资源的最基础必要的监视。  这些服务仅需最低配置，并收集高级监视服务所需的核心遥测数据。    

### <a name="azure-monitor"></a>Azure Monitor
使用 [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md)，可收集[指标](../monitoring-and-diagnostics/monitoring-overview-metrics.md)、[活动日志](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)和[诊断日志](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)，为 Azure 服务实现基本监视。  例如，通过活动日志可了解新资源的创建或修改时间。  可通过指标获取有关不同资源甚至是虚拟机内部操作系统的性能统计信息。  可使用 Azure 门户中的资源管理器之一来查看此数据、将数据发送至 Log Analytics 进行趋势和详细信息分析，或创建警报规则以主动发送有关关键问题的通知。

### <a name="service-health"></a>服务运行状况
应用程序的运行状况取决于其所依赖的 Azure 服务。  [Azure 服务运行状况](../service-health/service-health-overview.md)使用 Azure 服务标识任何可能影响应用程序的问题，还可帮助规划维护计划。


## <a name="premium-monitoring-services"></a>高级监视服务
下列 Azure 服务提供丰富的功能，用于收集和分析监视数据。  这些服务依赖于基本监视并使用 Azure 中常见的功能，通过收集的数据进行强大的分析，进而提供有关应用程序和基础结构的独特见解。  它们在面向不同受众的特定方案上下文中呈现数据。

### <a name="network-watcher"></a>网络观察程序
[网络观察程序](../network-watcher/network-watcher-monitoring-overview.md)为 Azure 中的不同网络方案提供基于方案的监视和诊断。  它将数据存储在 Azure 指标和诊断中供将来分析时使用，并与以下监视解决方案协同工作，实现监视网络的各个方面：
* [网络性能监视器 (NPM)](https://blogs.msdn.microsoft.com/azuregov/2017/09/05/network-performance-monitor-general-availability/) - 一种基于云的网络监视解决方案，用于监视公有云、数据中心和本地环境之间的连接
* [ExpressRoute 监视器](https://azure.microsoft.com/en-in/blog/monitoring-of-azure-expressroute-in-preview/) - 一种 NPM 功能，用于通过 ExpressRoute 线路监视端到端连接和性能。
* 流量分析 - 一种基于云的解决方案，可查看云网络上的用户和应用程序活动。
## <a name="shared-functionality"></a>共享功能
下列 Azure 工具为高级监视服务提供关键功能。  由于它们为多个服务所共享，因此可跨多个服务使用这些常见功能和配置。

### <a name="alerts"></a>警报
[Azure 警报](../monitoring-and-diagnostics/monitoring-overview-alerts.md)会主动发送有关关键情况的通知，并可能采取纠正措施。  警报规则可使用来自多个源（包括指标和日志）的数据。 它们使用[操作组](../monitoring-and-diagnostics/monitoring-action-groups.md)，其中包含唯一的一组收件人，以及用于响应警报的操作。  根据自身需求，可使用 Webhook 让警报启动外部操作，并将其与 ITSM 工具集成。

### <a name="dashboards"></a>仪表板
使用 [Azure 仪表板](../azure-portal/azure-portal-dashboards.md)，可将不同类型的数据合并到 Azure 门户中的单个窗格中，并与其他 Azure 用户共享。  例如，可以创建一个包含多个磁贴的仪表板，这些磁贴分别用于显示指标图、活动日志的表格、Application Insights 的使用情况图表，以及 Log Analytics 中日志搜索的输出。

还可将 Log Analytics 数据导出至 [Power BI](/power-bi/) 来利用其他可视化效果，并将数据与组织内外的人员共享。

### <a name="metrics-explorer"></a>指标资源管理器
[指标](../monitoring-and-diagnostics/monitoring-overview-metrics.md)是由 Azure 资源生成的数值，用于了解资源的操作情况和性能。 可以向 Log Analytics 发送指标，以便使用来自其他源的数据进行分析。



### <a name="activity-logs"></a>活动日志
[活动日志](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)提供有关 Azure 资源操作的数据。  其中包括有关资源配置更改、服务运行状况事件、资源使用优化建议的信息，以及与自动缩放操作相关的信息。  可以在 Azure 门户中特定资源的页面上查看其日志，或在活动日志资源管理器中的多个资源处查看日志。  此外还可向 Log Analytics 发送活动日志，以便使用由管理解决方案、虚拟机上的代理和其他资源收集的数据对其进行分析。


## <a name="example-scenarios"></a>示例方案
以下是高级示例，介绍如何在 Azure 中为不同的方案应用相应的监视工具。

### <a name="monitoring-a-web-application"></a>监视 Web 应用程序
可设想一个使用应用服务、Azure 存储 和 SQL 数据库在 Azure 中部署的 Web 应用程序。  首先，可在 Azure 门户中找到这些资源的相应页面，然后访问它们的[指标](../monitoring-and-diagnostics/monitoring-overview-metrics.md)和[活动日志](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)。  这包括关键信息，如对应用程序的请求数、平均响应时间，以及有关标识配置更改的信息。

然后可转到门户中的“监视”，同时查看不同资源的指标和日志。  在为指标确定标准参数时，[创建警报规则](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md)，目的是在特定条件下主动发送通知，比如在平均响应时间超出阈值时，。  为快速一览应用程序的每日性能，可创建 Azure 仪表板，用于显示关键 KPI 的指标图。




### <a name="monitoring-virtual-machines"></a>监视虚拟机
Azure 中同时运行有 Windows 和 Linux 虚拟机。  使用 Azure Monitor 查看[活动日志](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)和[主机级别指标](../monitoring-and-diagnostics/monitoring-overview-metrics.md)，然后向虚拟机添加 [Azure 诊断扩展](../virtual-machines/linux/tutorial-monitoring.md#install-diagnostics-extension)，以便从来宾操作系统收集指标。  然后创建[警报规则](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md)，以便在基本指标（如处理器利用率和内存）超过阈值时，主动发送通知。





## <a name="next-steps"></a>后续步骤
详细了解以下内容

* [Azure Monitor 入门](monitoring-get-started.md)
* [Azure 诊断](../azure-diagnostics.md)：如果要尝试诊断云服务、虚拟机、虚拟机规模集或 Service Fabric 应用程序中的问题。
