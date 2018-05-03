---
title: Azure Service Fabric 监视和诊断概述 | Azure
description: 了解 Azure Service Fabric 群集、应用程序和服务的监视与诊断。
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
origin.date: 04/03/2018
ms.date: 04/30/2018
ms.author: v-yeche
ms.openlocfilehash: 9fbf93ef90d6fa2d86a83af06481e67e571d1940
ms.sourcegitcommit: 0fedd16f5bb03a02811d6bbe58caa203155fd90e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2018
---
# <a name="monitoring-and-diagnostics-for-azure-service-fabric"></a>Azure Service Fabric 的监视和诊断

本文概述 Azure Service Fabric 的监视和诊断。 在任何云环境中开发、测试和部署工作负荷时，监视和诊断至关重要。 使用监视可以跟踪应用程序的使用方式、资源利用率，以及群集的总体运行状况。 可以使用此信息来诊断和更正任何问题，避免将来发生此类问题。 

监视和诊断的主要目标是：
* 检测和诊断基础结构问题
* 检测应用程序问题
* 了解资源消耗
* 跟踪应用程序、服务和基础结构的性能

在 Service Fabric 群集中，监视和诊断数据来自三个级别：应用程序、平台（群集）和基础结构。 
* [应用程序级别](service-fabric-diagnostics-event-generation-app.md)包括有关应用程序性能的数据，以及添加的其他任何自定义日志数据。 从应用程序本身的角度讲，应用程序监视数据最终应该传入 Application Insights (AI)。 可以通过 AI SDK、EventFlow 或所选的其他管道传输这些数据。
* [平台级别](service-fabric-diagnostics-event-generation-infra.md)包括针对群集执行的操作所生成的事件，这些事件与群集及其上运行的应用程序的管理相关。 应该使用 Service Fabric 分析解决方案将平台监视数据发送到 OMS Log Analytics，以帮助可视化传入事件。 通常会通过 Windows 或 Linux Azure 诊断扩展将此数据发送到存储帐户，供 OMS Log Analytics 访问。 
* 基础结构级别侧重于[性能监视](service-fabric-diagnostics-event-generation-perf.md)，它分析关键指标和性能计数器以确定资源利用率和负载。 可以使用代理实现性能监视 - 我们建议使用 OMS (Microsoft Monitoring) 代理，使性能数据最终传入平台事件所在的同一位置，从而在关联整个群集中发生的更改时获得更好的诊断体验。 

![诊断概述图表](media/service-fabric-diagnostics-overview/diagnostics-overview.png)

## <a name="monitoring-scenarios"></a>监视方案

本部分介绍有关监视 Service Fabric 群集的重要方案 - 应用程序、群集、性能和运行状况监视。 其中将会讨论每种方案的意图和总体方法。 可以在[最佳做法 - 监视和诊断](https://docs.microsoft.com/azure/architecture/best-practices/monitoring)中找到有关这些方案以及适用于 Azure 资源的其他一般性监视建议的更多详细信息。 

这些方案也松散映射到前面所述的三个级别的监视和诊断数据，也就是说，要在群集中适当处理每种方案，应该获取来自相应级别的监视和诊断数据。 但运行状况监视方案例外，因为它跨越整个群集以及群集中运行的任何组件。

## <a name="application-monitoring"></a>应用程序监视
应用程序监视跟踪应用程序的功能和组件的使用方式。 监视应用程序可以确保捕获影响用户的问题。 监视应用程序可能对以下情况有帮助：
* 确定应用程序负载和用户流量 - 是否需要缩放服务来满足用户需求，或解决应用程序中的潜在瓶颈？
* 识别服务通信和群集远程控制的问题
* 判断用户正在使用应用程序做些什么 - 收集应用程序中的遥测数据可能有助于将来的功能开发，以及更好地诊断应用错误
* 监视运行的容器中发生的情况
<!-- Not Avaiable on Application Insight(AI) -->

## <a name="platform-cluster-monitoring"></a>平台（群集）监视
若要确保平台和所有工作负荷按预期运行，监视 Service Fabric 群集至关重要。 Service Fabric 的目标之一是确保应用程序能够灵活应对硬件故障。 为了实现此目的，可以通过平台的系统服务功能检测基础结构问题，并快速将工作负荷故障转移到群集中的其他节点。 但在此特殊情况下，系统服务本身出现问题该怎么办？ 或者，在尝试移动工作负荷时，如果违反服务位置的规则该怎么办？ 监视群集可以随时了解群集中发生的活动，帮助有效诊断和解决问题。 需要分析的一些重要问题包括：
* 定位好应用程序并均衡群集中的负载后，Service Fabric 的行为方式是否符合预期？ 
* 对于确认有问题的群集，是否采取了用户措施，这些措施是否发挥了预期的作用？ 缩放群集时，这一点尤为重要。
* Service Fabric 是否能够正确处理群集中的数据和服务间的通信？

Service Fabric 通过操作和数据与消息传递通道现成地提供整套事件。 在 Windows 中，这些事件以单个 ETW 提供程序的形式提供，其中包含一组相关的 `logLevelKeywordFilters`，用于在不同的通道之间进行选择。 在 Linux 中，所有平台事件通过 LTTng 传入一个表中，可在其中按需筛选事件。 

这些通道包含组织有序的结构化事件用于更好地了解群集的状态。 创建群集时默认会启用“诊断”并创建一个 Azure 存储表，来自这些通道的事件将发送到其中，方便将来进行查询。 可以在[平台级别事件和日志生成](service-fabric-diagnostics-event-generation-infra.md)中详细了解如何监视群集。

若要收集群集生成的日志和事件，我们通常建议使用 [Azure 诊断扩展](service-fabric-diagnostics-event-aggregation-wad.md)。 此扩展与 Service Fabric 分析完美集成，后者提供用于监视 Service Fabric 群集的自定义仪表板，并可查询群集的事件和设置警报。
<!-- Not Available on [Event analysis with OMS](service-fabric-diagnostics-event-analysis-oms.md) -->
<!-- Not Available on  OMS Log Analytics Service Fabric specific solution -->

## <a name="performance-monitoring"></a>性能监视
监视底层基础结构是了解群集状态和资源利用率的重要组成部分。 测量系统性能的过程取决于多种因素，其中的每种因素通常是通过关键性能指标 (KPI) 测量的。 可将 Service Fabric 相关的 KPI 作为性能计数器映射到可以从群集中的节点收集的指标。
这些 KPI 有助于：
* 了解资源利用率和负载 - 实现缩放群集或优化服务进程的目的。
* 预测基础结构问题 - 许多问题在发生之前，都会出现性能骤然发生变化（下降）的前兆，因此，我们可以使用网络 I/O 和 CPU 利用率等 KPI 来预测和诊断基础结构问题。

可以在[性能指标](service-fabric-diagnostics-event-generation-perf.md)中找到应在基础结构级别收集的性能计数器列表。 

Service Fabric 为 Reliable Services 和 Reliable Actors 编程模型提供了一组性能计数器。 如果使用其中的任一模型，这些性能计数器可以提供 KPI 来帮助确保执行组件正常启动和停止，或者以足够快的速度处理可靠服务请求。 有关详细信息，请参阅 [Reliable Services 远程处理的监视](service-fabric-reliable-serviceremoting-diagnostics.md#performance-counters)和 [Reliable Actors 的性能监视](service-fabric-reliable-actors-diagnostics.md#performance-counters)。 除此之外，如果已在应用程序中进行相应的配置，Application Insights 还会提供它所要收集的一组性能指标。

<!-- Not Available on [OMS agent](service-fabric-diagnostics-oms-agent.md)-->

## <a name="health-monitoring"></a>运行状况监视
Service Fabric 平台包含运行状况模型，针对群集中的实体状态提供可扩展的运行状况报告。 每个节点、应用程序、服务、分区、副本或实例都具有持续可更新的运行状况。 运行状况可能是“正常”、“警告”或“错误”。 可以根据群集中的问题，针对针对每个实体发出的运行状况报告更改运行状况。 可以随时按如下所示在 Service Fabric Explorer (SFX) 中检查实体的运行状况，或者通过平台的运行状况 API 查询。 还可以通过添加自己的运行状况报告或使用运行状况 API 来自定义运行状况报告和修改实体的运行状况。 可以在 [Service Fabric 运行状况监视简介](service-fabric-health-introduction.md)中找到有关运行状况模型的详细信息。

![SFX 运行状况仪表板](media/service-fabric-diagnostics-overview/sfx-healthstatus.png)


## <a name="other-logging-solutions"></a>其他日志记录解决方案

许多事件会通过 etw 提供程序写出，并且可随其他日志记录解决方案一起扩展。 此外，还应考虑 [Elastic Stack](https://www.elastic.co/products)（尤其是考虑在脱机环境中运行群集时）、[Splunk](https://www.splunk.com/)、[Dynatrace](https://www.dynatrace.com/) 或其他任何偏好的平台。 
<!-- Not Available on [OMS](service-fabric-diagnostics-event-analysis-oms.md)-->
<!-- Not Available on [Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md)-->

选择任何平台时的要点应该包括用户界面和查询选项的习惯性、是否能够可视化数据和轻松创建易读的仪表板，以及平台提供的用于增强监视的其他工具（例如自动警报）。

## <a name="next-steps"></a>后续步骤

* 若要开始检测应用程序，请参阅[应用程序级别事件和日志生成](service-fabric-diagnostics-event-generation-app.md)。
* 在[平台级别事件和日志生成](service-fabric-diagnostics-event-generation-infra.md)中详细了解如何监视平台以及 Service Fabric 提供的事件。
<!-- Not Avaiable on * Go through the steps to set up AI for your application with [Monitor and diagnose an ASP.NET Core application on Service Fabric](service-fabric-tutorial-monitoring-aspnet.md) -->
<!-- Not Avaiable on * Learn how to set up OMS Log Analytics for monitoring containers - [Monitoring and Diagnostics for Windows Containers in Azure Service Fabric](service-fabric-tutorial-monitoring-wincontainers.md) -->
* 了解适用于 Azure 资源的一般性监视建议 - [最佳做法 - 监视和诊断](https://docs.microsoft.com/azure/architecture/best-practices/monitoring)。
<!--Update_Description: update meta properties, wording update, update link  -->