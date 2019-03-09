---
title: Azure Service Fabric 诊断常见情况 | Azure
description: 了解如何使用 Azure Service Fabric 对常见情况进行故障排除
services: service-fabric
documentationcenter: .net
author: rockboyfor
manager: digimobile
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
origin.date: 02/25/2019
ms.date: 03/04/2019
ms.author: v-yeche
ms.openlocfilehash: 67b1b2e4cf26aa5250415e720fed6ae95e51adb4
ms.sourcegitcommit: ea33f8dbf7f9e6ac90d328dcd8fb796241f23ff7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/01/2019
ms.locfileid: "57204282"
---
# <a name="diagnose-common-scenarios-with-service-fabric"></a>使用 Service Fabric 诊断常见情况

本文阐述了用户在使用 Service Fabric 进行监视和诊断时遇到的常见情况。 所介绍的方案涵盖了 Service Fabric 的所有 3 层：应用程序、群集和基础结构。 每个解决方案均使用 Application Insights 和 Log Analytics（Azure 监视工具）来完成每种情况。 每个解决方案中的步骤都向用户介绍了如何在 Service Fabric 环境中使用 Application Insights 和 Log Analytics。

## <a name="prerequisites-and-recommendations"></a>先决条件和建议

本文中的解决方案将使用以下工具。 建议对这些工具进行设置和配置：

<!--Not Available on * [Application Insights with Service Fabric](service-fabric-tutorial-monitoring-aspnet.md)-->
* [在群集上启用 Azure 诊断](service-fabric-diagnostics-event-aggregation-wad.md)
* [设置 Log Analytics 工作区](service-fabric-diagnostics-oms-setup.md)
* [用于跟踪性能计数器的 Log Analytics 代理](service-fabric-diagnostics-oms-agent.md)

<!--Not Available on ## How can I see unhandled exceptions in my application?-->
<!--Not Available on  ## How do I view which HTTP calls are used in my services?-->
<!--Not Available on ## How do I create an alert when a node goes down-->

## <a name="how-can-i-be-alerted-of-application-upgrade-rollbacks"></a>怎样才能收到应用程序升级回滚警报？

1. 在与之前相同的“日志搜索”窗口中，针对升级回滚输入以下查询。 这些事件 ID 位于[应用程序事件参考](service-fabric-diagnostics-event-generation-operational.md#application-events)下方

    ```kusto
    ServiceFabricOperationalEvent
    | where EventId == 29623 or EventId == 29624
    ```

2. 单击顶部的“新建警报规则”，现在只要发生基于此查询的事件，你就会收到警报。

## <a name="how-do-i-see-container-metrics"></a>如何查看容器指标？

在包含所有图表的相同视图中，你将看到有关容器性能的一些磁贴。 你需要 Log Analytics 代理和[容器监视解决方案](service-fabric-diagnostics-oms-containers.md)来填充这些磁贴。

![Log Analytics 容器指标](media/service-fabric-diagnostics-common-scenarios/containermetrics.png)

>[!NOTE]
>若要从容器**内部**检测遥测数据，需要为容器添加 [Application Insights nuget 包](https://github.com/Microsoft/ApplicationInsights-servicefabric#microsoftapplicationinsightsservicefabric--for-service-fabric-lift-and-shift-scenarios)。

## <a name="how-can-i-monitor-performance-counters"></a>如何监视性能计数器？

1. 向群集添加 Log Analytics 代理后，需要添加要跟踪的特定性能计数器。导航到门户中的 Log Analytics 工作区页面（工作区选项卡位于解决方案页面的左侧菜单中）。

    ![Log Analytics 工作区选项卡](media/service-fabric-diagnostics-common-scenarios/workspacetab.png)

2. 进入工作区页面后，单击同一左侧菜单中的“高级设置”。

    ![Log Analytics 高级设置](media/service-fabric-diagnostics-common-scenarios/advancedsettingsoms.png)

3. 单击“数据”>“Windows 性能计数器”（对于 Linux 计算机，则为“数据”>“Linux 性能计数器”），开始通过 Log Analytics 代理从节点收集特定计数器。 以下是要添加的计数器的格式示例

    * `.NET CLR Memory(<ProcessNameHere>)\\# Total committed Bytes`
    * `Processor(_Total)\\% Processor Time`

    在快速入门中，VotingData 和 VotingWeb 是所用进程名称，因此，将按以下格式跟踪这些计数器

    * `.NET CLR Memory(VotingData)\\# Total committed Bytes`
    * `.NET CLR Memory(VotingWeb)\\# Total committed Bytes`

    ![Log Analytics 性能计数器](media/service-fabric-diagnostics-common-scenarios/omsperfcounters.png)

4. 这将允许你查看基础结构处理工作负荷的方式，并根据资源利用率设置相关警报。 例如，如果处理器总利用率高于 90% 或低于 5%，则可能需要设置警报。 此时将使用名为“处理器时间百分比”的计数器。 可通过为以下查询创建警报规则来执行此操作：

    ```kusto
    Perf | where CounterName == "% Processor Time" and InstanceName == "_Total" | where CounterValue >= 90 or CounterValue <= 5.
    ```

## <a name="how-do-i-track-performance-of-my-reliable-services-and-actors"></a>如何跟踪 Reliable Services 和 Actors 的性能？

若要跟踪应用程序中 Reliable Services 或 Actors 的性能，还应收集“Service Fabric 执行组件”、“执行组件方法”、“服务”和“服务方法”计数器。 下面是要收集的可靠服务和执行组件性能计数器

>[!NOTE]
>当前无法通过 Log Analytics 代理收集 Service Fabric 性能计数器，但可以通过[其他诊断解决方案](service-fabric-diagnostics-partners.md)进行收集

* `Service Fabric Service(*)\\Average milliseconds per request`
* `Service Fabric Service Method(*)\\Invocations/Sec`
* `Service Fabric Actor(*)\\Average milliseconds per request`
* `Service Fabric Actor Method(*)\\Invocations/Sec`

在 Reliable [Services](service-fabric-reliable-serviceremoting-diagnostics.md) 和 [Actors](service-fabric-reliable-actors-diagnostics.md) 上查看这些链接可获取性能计数器的完整列表

## <a name="next-steps"></a>后续步骤

* [在 AI 中设置警报](../azure-monitor/app/alerts.md)以获取有关性能或使用情况的通知
* [Application Insights 中的智能检测](../azure-monitor/app/proactive-diagnostics.md)针对发送给 AI 的遥测进行主动分析，向你警告潜在的性能问题
* 详细了解有助于进行检测和诊断的 Log Analytics [警报](../log-analytics/log-analytics-alerts.md)。
* 对于本地群集，Log Analytics 提供可用于向 Log Analytics 发送数据的网关（HTTP 正向代理）。 有关更多信息，请参阅[使用 Log Analytics 网关将无法访问 Internet 的计算机连接到 Log Analytics](../azure-monitor/platform/gateway.md)
* 掌握 Log Analytics 中提供的[日志搜索和查询](../log-analytics/log-analytics-log-searches.md)功能
* 有关 Log Analytics 及其功能的更详细概述，请参阅[什么是 Log Analytics？](../operations-management-suite/operations-management-suite-overview.md)

<!--Update_Description: new articles on service fabric diagnostics common scenarios -->
<!--ms.date: 03/04/2019-->