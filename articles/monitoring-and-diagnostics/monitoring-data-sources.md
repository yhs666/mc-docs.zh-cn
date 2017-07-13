---
title: "使用 Azure 中的监视数据 | Azure"
description: "了解 Azure 上目前提供的所有监视数据源。"
author: johnkemnetz
manager: rboucher
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2017
ms.author: v-yiso
ms.openlocfilehash: 5aef88914a898fb9cdee06c13a188ba04e3dc734
ms.sourcegitcommit: 6728c686935e3cdfaa93a7a364b959ab2ebad361
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
# 使用 Azure 中的监视数据
<a id="consume-monitoring-data-from-azure" class="xliff"></a>

在 Azure 平台上，我们正在使用 Azure Monitor 管道将监视数据集中在一起放置于单个位置，但实际上目前还并非所有监视数据都在该管道中提供。 本文汇总了可以从 Azure 服务以编程方式访问监视数据的各种方法。

## 数据使用选项
<a id="options-for-data-consumption" class="xliff"></a>

| 数据类型 | 类别 | 支持的服务 | 访问方法 |
| --- | --- | --- | --- |
| 计算来宾 OS 指标（例如 性能计数器） | 度量值 | [Windows](../virtual-machines/virtual-machines-dotnet-diagnostics.md) 和 Linux 虚拟机 (v2)、[云服务](../cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md)、[Service Fabric](../service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md) | <ul><li>**存储表或 blob：**[Windows 或 Linux Azure 诊断](../cloud-services/cloud-services-dotnet-diagnostics-storage.md)</li><li>**事件中心：**[Windows Azure 诊断](../event-hubs/event-hubs-streaming-azure-diags-data.md)</li></ul> |
| 自定义指标或应用程序指标 | 度量值 | 使用 Application Insights 检测到的任何应用程序 | <ul><li>**REST API：**[Application Insights REST API](https://dev.applicationinsights.io/reference)</li></ul> |
| 存储度量值 | 度量值 | Azure 存储 | <ul><li>**存储表：**[存储分析](https://docs.microsoft.com/rest/api/storageservices/storage-analytics)</li></ul> |
| 活动日志 | 事件 | 所有 Azure 服务 | <ul><li>**REST API：**[Azure Monitor 事件 API](https://docs.microsoft.com/rest/api/monitor/events)</li><li>**存储 blob 或事件中心：**[日志配置文件](monitoring-overview-activity-logs.md#export-the-activity-log-with-log-profiles)</li></ul> |
| 计算来宾 OS 日志（例如 IIS、ETW、syslog） | 事件 | [Windows](../virtual-machines/virtual-machines-dotnet-diagnostics.md) 和 Linux 虚拟机 (v2)、[云服务](../cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md)、[Service Fabric](../service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md) | <ul><li>**存储表或 blob：**[Windows 或 Linux Azure 诊断](../cloud-services/cloud-services-dotnet-diagnostics-storage.md)</li><li>**事件中心：**[Windows Azure 诊断](../event-hubs/event-hubs-streaming-azure-diags-data.md)</li></ul> |
| 应用服务日志 | 事件 | 应用程序服务 | <ul><li>**文件、表或 blob 存储：**[Web 应用诊断](../app-service-web/web-sites-enable-diagnostic-log.md)</li></ul> |
| 存储日志 | 事件 | Azure 存储 | <ul><li>**存储表：**[存储分析](https://docs.microsoft.com/rest/api/storageservices/storage-analytics)</li></ul> |
| 安全中心警报 | 事件 | Azure 安全中心 | <ul><li>**REST API：**[安全警报](https://msdn.microsoft.com/library/mt704050.aspx)</li></ul> |
| 安全中心资源状态 | 状态 | [所有支持的资源](https://msdn.microsoft.com/library/mt704041.aspx#Anchor_1) | <ul><li>**REST API：**[安全状态](https://msdn.microsoft.com/library/mt704041.aspx)</li></ul> |
| 资源运行状况 | 状态 | 支持的服务 | <ul><li>**REST API：**[资源运行状况 REST API](https://azure.microsoft.com/blog/reduce-troubleshooting-time-with-azure-resource-health/)</li></ul> |
| Azure Monitor 活动日志警报 | 通知 | 所有 Azure 服务 | <ul><li>**Webhook：**Azure 活动日志警报</li></ul> |
****


## 后续步骤
<a id="next-steps" class="xliff"></a>

- 详细了解 [Azure 活动日志](./monitoring-overview-activity-logs.md)
