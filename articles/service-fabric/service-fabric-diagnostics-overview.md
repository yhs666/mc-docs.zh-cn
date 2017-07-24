---
title: "Azure Service Fabric 监视和诊断概述 | Azure"
description: "了解 Azure Service Fabric 群集、应用程序和服务的监视与诊断。"
services: service-fabric
documentationcenter: .net
author: rockboyfor
manager: digimobile
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
origin.date: 05/26/2017
ms.date: 07/17/2017
ms.author: v-yeche
ms.openlocfilehash: 6f94ed28d0be01cb437c76b966c13956062be339
ms.sourcegitcommit: f2f4389152bed7e17371546ddbe1e52c21c0686a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/14/2017
---
# <a name="monitoring-and-diagnostics-for-azure-service-fabric"></a>Azure Service Fabric 的监视和诊断

监视和诊断对于在生产环境中开发、测试和部署应用程序与服务至关重要。 如果规划和实施的监视与诊断有助于确保应用程序和服务在本地开发环境或在生产环境中按预期工作，则 Service Fabric 解决方案可以发挥最大的作用。

监视和诊断的主要目标是：
* 检测和诊断硬件和基础结构问题
* 检测软件和应用问题，减少服务停机时间
* 了解资源消耗情况，帮助推动运营决策
* 优化应用程序、服务和基础结构性能
* 生成业务见解并确定改进区域

监视和诊断的整体工作流程分为三个步骤：

1. **事件生成**：包括基础结构（群集）级别和应用程序/服务级别的事件（日志、跟踪、自定义事件）
2. **事件聚合**：需要先收集和聚合生成的事件才能显示这些事件
3. **分析**：需可视化事件并能够以某种格式访问事件，以便按需进行分析和显示

有许多产品能够解决上述三个方面的问题，你可以针对每个方面任意选择不同的技术。 必须确保将各个部分组合起来协同工作，这样才能为群集提供端到端的监视解决方案。

## <a name="event-generation"></a>事件生成

监视和诊断工作流的第一个步骤是创建和生成事件与日志。 可在两个级别生成这些事件、日志和跟踪：基础结构层（群集中的任何组件、计算机或 Service Fabric 操作）或应用程序层（添加到应用中的任何检测或部署到群集的服务）。 其中每个级别的事件可自定义，不过，Service Fabric 默认提供一些检测。

请阅读有关[基础结构级事件](service-fabric-diagnostics-event-generation-infra.md)和[应用程序级事件](service-fabric-diagnostics-event-generation-app.md)的详细信息，了解提供的内容以及如何添加更多的检测。

在确定要使用的日志记录提供程序后，需确保正确聚合和存储日志。

## <a name="event-aggregation"></a>事件聚合

若要收集应用程序和群集生成的日志与事件，我们通常建议使用 [Azure 诊断](service-fabric-diagnostics-event-aggregation-wad.md)（更像是基于代理的日志收集）或 [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md)（进程内日志收集）。

如果日志源和目标的集不经常更改，并且源及其目标之间存在直接的映射，那么，使用 Azure 诊断扩展为 Service Fabric 服务收集应用程序日志是一种很好的做法。 采用这种做法的原因是，Azure 诊断的配置是在 Resource Manager 层发生的，因此，对配置进行重大更改需要更新/重新部署群集。 此外，这种做法可以充分确保将日志存储在某个位置更长的时间，供各种分析平台访问。 但这也意味着，最终的管道效率比使用 EventFlow 等选项更低。

使用 [EventFlow](https://github.com/Azure/diagnostics-eventflow) 可让服务将其日志直接发送到分析与可视化平台和/或存储。 其他库（ILogger、Serilog 等）可用于同一用途，但 EventFlow 的优势在于它是专门为进程内日志收集所设计的，支持 Service Fabric 服务。 这往往会带来许多潜在优势：

* 易于配置及部署
    * 诊断数据收集配置只是服务配置的一部分。 将其与应用程序的其余部分始终保持“同步”很简单。
    * 可轻松基于每个应用程序或每个服务进行配置。
    * 通过 EventFlow 配置数据目标只需添加相应的 NuGet 包并更改 *eventFlowConfig.json* 文件
* 灵活性
    * 只要客户端库支持目标数据存储系统，应用程序就可以将数据发送至任何所需位置。 可以根据需要添加新的目标。
    * 可以实现复杂的捕获、筛选和数据聚合规则。
* 访问内部应用程序数据与上下文
    * 应用程序/服务进程内运行的诊断子系统可以轻松地随着上下文信息而扩展跟踪

需要注意的一点是，这两个选项并不互斥。尽管使用其中的一个选项即可完成类似的作业，但同时设置这两个选项可能会有所帮助。 在大多数情况下，将代理与进程内收集相结合可使监视工作流更加可靠。 可为基础结构级日志选择 Azure 诊断扩展（代理），同时，为应用程序级日志使用 EventFlow（进程内收集）。 确定最合适的选项后，可以开始考虑如何显示和分析数据。

## <a name="event-analysis"></a>事件分析

可以借助市场中有许多优异平台来分析和可视化监视与诊断数据。 我们推荐的两个平台是 [OMS](service-fabric-diagnostics-event-analysis-oms.md) 和 [Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md)，因为它们能够与 Service Fabric 更好地集成；但同时应该考察[弹性堆栈](https://www.elastic.co/products)（尤其是考虑在脱机环境中运行群集时）、[Splunk](https://www.splunk.com/) 或其他任何偏爱的平台。

选择任何平台时的要点应该包括用户界面和查询选项的习惯性、是否能够可视化数据和轻松创建易读的仪表板，以及平台提供的用于增强监视的其他工具（例如自动警报）。

除了所选的平台以外，将 Service Fabric 群集设置为 Azure 资源后，还可以访问 Azure 的现成计算机监视功能，这在执行特定的性能和指标监视时可能非常有用。

### <a name="azure-monitor"></a>Azure Monitor

可以使用 [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview.md) 来监视构建 Service Fabric 群集时所在的许多 Azure 资源。
<!-- Not Available [virtual machine scale set](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesets) -->
<!-- Not Available [virtual machines](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesetsvirtualmachines) -->
![Azure 门户中收集的指标信息视图](media/service-fabric-diagnostics-overview/azure-monitoring-metrics.png)

若要自定义图表，请遵照 [Metrics in Azure](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md)（Azure 中的指标）中的说明。 还可以根据 [Create alerts in Azure Monitor for Azure services](../monitoring-and-diagnostics/insights-alerts-portal.md)（在 Azure Monitor 中为 Azure 服务创建警报）中所述，基于这些指标创建警报。 可以根据 [Configure a web hook on an Azure metric alert](../monitoring-and-diagnostics/insights-webhooks-alerts.md)（针对 Azure 指标警报配置 Webhook）中所述，使用 Webhook 将警报发送到通知服务。 Azure Monitor 仅支持一个订阅。 

## <a name="next-steps"></a>后续步骤

### <a name="watchdogs"></a>监视器

监视器是一个独立的服务，可以监视各个服务的运行状况和负载，并报告运行状况模型层次结构中任何组件的运行状况。 这有助于防止出现基于单个服务的视图所检测不到的错误。 监视器也是一个不错的代码托管位置，它可以在无需用户交互的情况下执行补救措施（例如，根据特定的时间间隔清理存储中的日志文件）。 可在[此处](https://github.com/Azure-Samples/service-fabric-watchdog-service)获取监视软件服务实现示例。

开始了解如何在[基础结构级别](service-fabric-diagnostics-event-generation-infra.md)和[应用程序级别](service-fabric-diagnostics-event-generation-app.md)生成事件与日志。