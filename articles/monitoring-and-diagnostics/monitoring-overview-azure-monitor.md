---
title: "Azure Monitor 概述 | Azure"
description: "Azure Monitor 收集在警报、webhook、自动缩放和自动化中使用的统计信息。 本文还列出了其他 Microsoft 监视选项。"
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 05/12/2017
ms.author: v-yiso
ms.date: 
ms.openlocfilehash: 238b0a28416490f7982cd1cfc4ee30b2f467ef7e
ms.sourcegitcommit: d5d647d33dba99fabd3a6232d9de0dacb0b57e8f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/14/2017
---
# Azure Monitor 概述
<a id="overview-of-azure-monitor" class="xliff"></a>
本文概述了 Azure 中的 Azure Monitor 服务。 它讨论了 Azure Monitor 可以执行的工作并指出了可以在哪里找到有关如何使用 Azure Monitor 的其他信息。  

## 为何要监视应用程序或系统
<a id="why-monitor-your-application-or-system" class="xliff"></a>
云应用程序很复杂，包含很多移动部件。 监视可以为用户提供数据，确保应用程序始终处于健康运行状态。 监视还有助于避免潜在问题，或者解决过去的问题。 此外，还可以利用监视数据深入了解应用程序的情况。 这些解析可帮助提升应用程序性能或维护性，或者将某些原本需要手动介入的操作自动化。


## Azure Monitor 和 Microsoft 的其他监视产品
<a id="azure-monitor-and-microsofts-other-monitoring-products" class="xliff"></a>
Azure Monitor 针对 Azure 中的大多数服务提供基本级别的基础结构指标和日志。 尚未将其数据放置到 Azure Monitor 中的 Azure 服务在将来会将数据放置到其中。 

Microsoft 还提供了其他产品和服务，用以为还具有本地安装的开发人员、DevOps 或 IT 运营人员提供其他监视功能。 若要了解这些不同的产品和服务以及它们如何协作，请参阅 [Microsoft Azure 中的监视功能](monitoring-overview.md)。

## 监视源 - 计算
<a id="monitoring-sources---compute" class="xliff"></a>

![非计算资源的监视和诊断模型](./media/monitoring-overview-azure-monitor/Monitoring_Azure_Resources-compute_v6.png)

计算服务包括 
- 云服务 
- 虚拟机 
- 虚拟机规模集 
- Service Fabric

### 应用程序 - 诊断日志、应用程序日志和指标
<a id="application---diagnostics-logs-application-logs-and-metrics" class="xliff"></a>
在计算模型中，应用程序可以基于来宾 OS 运行。 应用程序会发出自己的日志和指标集。 Azure Monitor 依赖于 Azure 诊断扩展（Windows 或 Linux）来收集大多数应用程序级指标和日志。 类型包括

* 性能计数器
* 应用程序日志
* Windows 事件日志
* .NET 事件源
* IIS Logs
* 基于清单的 ETW
* 故障转储
* 客户错误日志

如果没有诊断扩展，则只有少数指标（例如 CPU 使用率）可用。 

### 宿主和来宾 VM 指标
<a id="host-and-guest-vm-metrics" class="xliff"></a>
前面列出的计算资源具有它们与之交互的专用宿主 VM 和来宾 OS。 宿主 VM 和来宾 OS 是 Hyper-V 虚拟机监控程序模型中的根 VM 和来宾 VM 的等效项。 可以收集关于这两者的指标。 还可以收集关于来宾 OS 的诊断日志。   

### 活动日志
<a id="activity-log" class="xliff"></a>
可以搜索活动日志（以前称为操作日志或审核日志）中是否存在通过 Azure 基础结构查看的资源的相关信息。 日志包含多种信息，例如创建或销毁资源的时间。  有关详细信息，请参阅[活动日志概述](./monitoring-overview-activity-logs.md)。 

## 监视源 - 所有其他项
<a id="monitoring-sources---everything-else" class="xliff"></a>

![计算资源的监视和诊断模型](./media/monitoring-overview-azure-monitor/Monitoring_Azure_Resources-non-compute_v6.png)


### 资源 - 指标和诊断日志
<a id="resource---metrics-and-diagnostics-logs" class="xliff"></a>
可收集的指标和诊断日志因资源类型而异。 例如，Web 应用提供有关磁盘 IO 和 CPU 百分比的统计信息。 对于服务总线队列来说，这些统计信息不存在，该队列提供的是队列大小和消息吞吐量之类的指标。

### 宿主和来宾 VM 指标
<a id="host-and-guest-vm-metrics" class="xliff"></a>
因为在资源与特定宿主 VM 或来宾 VM 之间不一定存在 1:1 映射关系，因此，这些指标不可用。

### 活动日志
<a id="activity-log" class="xliff"></a>
活动日志与针对计算资源的活动日志相同。  

## 用于监视数据
<a id="uses-for-monitoring-data" class="xliff"></a>
在收集数据后，可以使用该数据在 Azure Monitor 中执行以下操作

### 路由
<a id="route" class="xliff"></a>
可以将监视数据实时流式传输到其他位置。

示例包括：

- 发送到 Application Insights，以便使用其更加丰富的可视化和分析工具。
- 发送到事件中心，以便将其路由到第三方工具。 

### 存储和存档
<a id="store-and-archive" class="xliff"></a>
某些监视数据已存储并且在设定的时间段内在 Azure Monitor 中可用。 
- 指标存储 30 天。 
- 活动日志条目存储 90 天。 
- 诊断日志根本不存储。 

若要将数据存储比上面列出的时间段更长的时间，可以使用 Azure 存储。 监视数据根据设置的保留策略保留在存储帐户中。 不需要为数据在 Azure 存储中占用的空间付款。 

可以通过多种方式使用该数据：

- 写入以后，即可让 Azure 内外的其他工具读取和处理该数据。
- 可以将该数据下载到本地进行本地存档，也可以更改云中的保留策略，让数据保留更长的时间。  
- 可以无限期地将数据保留在 Azure 存储中以进行存档。 

### 查询
<a id="query" class="xliff"></a>
可以使用 Azure 监视器 REST API、跨平台命令行接口 (CLI) 命令、PowerShell cmdlet 或 .NET SDK 访问系统或 Azure 存储中的数据

示例包括：

* 获取所编写的自定义监视应用程序的数据
* 创建自定义查询，将该数据发送到第三方应用程序。

### 可视化
<a id="visualize" class="xliff"></a>
以图形和图表形式将监视数据可视化可以帮助你更快地查明趋势，其速度远非单纯查看数据可比。  

可视化方法包括：

* 使用 Azure 门户
* 将数据路由到 Azure Application Insights
* 将数据路由到 Microsoft PowerBI
* 将数据路由到第三方可视化工具，可以使用实时传送视频流，也可以让工具从 Azure 存储中的存档读取。


### 自动化
<a id="automate" class="xliff"></a>
可以使用监视数据触发警报或甚至整个过程。 示例包括：

* 使用数据根据应用程序负载自动缩放计算实例数。
* 当某个指标超出预定阈值时发送电子邮件。
* 调用 Web URL (webhook)，在 Azure 外部系统中执行操作。
* 在 Azure 自动化中启动 Runbook，执行各种任务

## 访问 Azure Monitor 的方法
<a id="methods-of-accessing-azure-monitor" class="xliff"></a>
一般情况下，可以使用下述方法之一操作数据的跟踪、路由和检索。 并非所有方法都适用于所有操作或数据类型。

* [Azure 门户](https://portal.azure.cn)
* [PowerShell](./insights-powershell-samples.md)  
* [跨平台的命令行接口 (CLI)](./insights-cli-samples.md)
* [REST API](https://docs.microsoft.com/rest/api/monitor/)
* [.NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Management.Monitor)

## 后续步骤
<a id="next-steps" class="xliff"></a>
详细了解以下内容
- 如果要尝试诊断云服务、虚拟机、虚拟机规模集或 Service Fabric 应用程序中的问题，请设置 [Azure 诊断扩展](../azure-diagnostics.md)。
- [Azure 存储故障诊断](../storage/storage-e2e-troubleshooting.md) 在使用存储 Blob、表或队列的情况下。
