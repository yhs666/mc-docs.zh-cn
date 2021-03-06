---
title: 使用 Linux Azure 诊断聚合 Azure Service Fabric 事件 | Azure
description: 了解如何使用 LAD 聚合和收集事件，以便对 Azure Service Fabric 群集进行监视和诊断。
services: service-fabric
documentationcenter: .net
author: rockboyfor
manager: digimobile
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
origin.date: 02/25/2019
ms.date: 03/04/2019
ms.author: v-yeche
ms.openlocfilehash: 20a573f8fd0d2956b2bc316e7202ce1ec9cda70e
ms.sourcegitcommit: ea33f8dbf7f9e6ac90d328dcd8fb796241f23ff7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/01/2019
ms.locfileid: "57204013"
---
# <a name="event-aggregation-and-collection-using-linux-azure-diagnostics"></a>使用 Linux Azure 诊断聚合和收集事件
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-event-aggregation-wad.md)
> * [Linux](service-fabric-diagnostics-event-aggregation-lad.md)
>
>

当你运行 Azure Service Fabric 群集时，最好是从一个中心位置的所有节点中收集日志。 将日志放在中心位置可帮助分析和排查群集中的问题，或该群集中运行的应用程序与服务的问题。

若要上传和收集日志，一种方式是使用可将日志上传到 Azure 存储并能选择将日志发送到事件中心的 Linux Azure 诊断 (LAD) 扩展。 也可以使用外部进程读取存储中的事件，并将它们放在分析平台产品（例如 [Log Analytics](../log-analytics/log-analytics-service-fabric.md) 或其他日志分析解决方案）中。

<!-- Not Available on Azure Application Insights -->

## <a name="log-and-event-sources"></a>日志和事件源

### <a name="service-fabric-platform-events"></a>Service Fabric 平台事件
Service Fabric 通过 [LTTng](http://lttng.org) 发出几个现成可用的日志，包括操作事件或运行时事件。 这些日志存储在群集的 Resource Manager 模板指定的位置。 若要获取或设置存储帐户详细信息，请搜索 AzureTableWinFabETWQueryable 标记，然后查找 StoreConnectionString。

### <a name="application-events"></a>应用程序事件
 检测软件时，事件按指定从应用程序和服务的代码中发出。 可以使用任何能够写入基于文本的日志的日志记录解决方案，例如 LTTng。 有关详细信息，请参阅有关跟踪应用程序的 LTTng 文档。

[在本地计算机开发安装过程中监视和诊断服务](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)。

## <a name="deploy-the-diagnostics-extension"></a>部署诊断扩展
收集日志的第一个步骤是将诊断扩展部署在 Service Fabric 群集中的每个 VM 上。 诊断扩展会收集每个 VM 上的日志，并将它们上传到指定的存储帐户。 

要在创建群集期间将诊断扩展部署到群集中的 VM，请将“**诊断**”设置为“**打开**”。 创建群集后，无法使用门户更改此设置，因此，必须在资源管理器模板中进行相应更改。

这将配置 LAD 代理来监视指定的日志文件。 每当在文件中追加新行时，该代理都会创建一个 syslog 条目并将其发送到指定的存储（表）。

## <a name="next-steps"></a>后续步骤

1. 若要更详细了解在排查问题时应检查哪些事件，请参阅 [LTTng 文档](http://lttng.org/docs)和[使用 LAD](/virtual-machines/extensions/diagnostics-linux)。
2. [设置 Log Analytics 代理](service-fabric-diagnostics-event-analysis-oms.md)来帮助收集指标、监视群集上部署的容器和直观显示日志

<!--Update_Description: update meta properties -->