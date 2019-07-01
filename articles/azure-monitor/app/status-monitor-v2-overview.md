---
title: Azure 状态监视器 v2 概述 | Azure Docs
description: 状态监视器 v2 的概述。 无需重新部署网站即可监视网站性能。 使用托管在本地、VM 或 Azure 上的 ASP.NET Web 应用。
services: application-insights
documentationcenter: .net
author: lingliw
manager: digimobile
ms.assetid: 769a5ea4-a8c6-4c18-b46c-657e864e24de
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 6/4/2019
ms.author: v-lingwu
ms.openlocfilehash: 80e9ab5cf353df4689bf6a8f63fa1c440eab9679
ms.sourcegitcommit: 5fc46672ae90b6598130069f10efeeb634e9a5af
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/19/2019
ms.locfileid: "67236535"
---
# <a name="status-monitor-v2"></a>状态监视器 v2

状态监视器 v2 是发布到 [PowerShellGallery](https://www.powershellgallery.com/packages/Az.ApplicationMonitor) 的 PowerShell 模块，是[状态监视器](https://docs.microsoft.com/azure/azure-monitor/app/monitor-performance-live-website-now)的替代品。 此模块提供了使用 IIS 托管的 .NET Web 应用程序的无代码检测。
遥测数据将发送到 Azure 门户，你可以在其中[监视](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview)应用程序。

> [!IMPORTANT]
> 状态监视器 v2 目前以公共预览版提供。
> 此预览版在提供时没有附带服务级别协议，不建议将其用于生产工作负荷。 某些功能可能不受支持或者受限。
> 有关详细信息，请参阅[世纪互联 Azure 预览版补充使用条款](https://www.azure.cn/support/legal/preview-supplemental-terms/)

## <a name="powershell-gallery"></a>PowerShell 库

https://www.powershellgallery.com/packages/Az.ApplicationMonitor


## <a name="instructions"></a>说明
- 查看我们的[入门说明](status-monitor-v2-get-started.md)，以立即开始使用简洁的代码示例。
- 查看我们的[详细说明](status-monitor-v2-detailed-instructions.md)，以深入了解如何开始使用。

## <a name="powershell-api-reference"></a>PowerShell API 参考
- [Disable-ApplicationInsightsMonitoring](status-monitor-v2-api-disable-monitoring.md)
- [Enable-ApplicationInsightsMonitoring](status-monitor-v2-api-enable-monitoring.md)
- [Enable-InstrumentationEngine](status-monitor-v2-api-enable-instrumentation-engine.md)
- [Get-ApplicationInsightsMonitoringConfig](status-monitor-v2-api-get-config.md)
- [Get-ApplicationInsightsMonitoringStatus](status-monitor-v2-api-get-status.md)
- [Set-ApplicationInsightsMonitoringConfig](status-monitor-v2-api-set-config.md)

## <a name="troubleshooting"></a>故障排除
- [故障排除](status-monitor-v2-troubleshoot.md)
- [已知问题](status-monitor-v2-troubleshoot.md#known-issues)


## <a name="faq"></a>常见问题

- 状态监视器 v2 是否支持代理安装？

  **是**。 有多个选项可以下载状态监视器 v2。 如果计算机可以访问 Internet，则可以使用 `-Proxy` 参数登录到 PowerShell 库。 还可以手动下载此模块并将其安装到计算机上，或者直接使用此模块。 上述每个选项都在我们的[详细说明](status-monitor-v2-detailed-instructions.md)中进行了说明。
  
- 如何验证启用是否成功？

   我们没有 cmdlet 来验证启用是否成功。 我们建议使用[实时指标](https://docs.microsoft.com/azure/azure-monitor/app/live-stream)来快速观察你的应用程序是否正在向我们发送遥测数据。

   还可以使用 [Analytics](../log-query/get-started-portal.md) 列出当前正在发送遥测数据的所有云角色。
   ```Kusto
   union * | summarize count() by cloud_RoleName, cloud_RoleInstance
   ```

## <a name="next-steps"></a>后续步骤

查看遥测：

* [浏览指标](../../azure-monitor/app/metrics-explorer.md)，以便监视性能和使用情况
* [搜索事件和日志](../../azure-monitor/app/diagnostic-search.md)以诊断问题
* [分析](../../azure-monitor/app/analytics.md)，以便进行更高级的查询

添加更多遥测：

* [创建 Web 测试](monitor-web-app-availability.md)，以确保站点保持活动状态。
* [添加 Web 客户端遥测](../../azure-monitor/app/javascript.md)，以查看网页代码中的异常并将其插入跟踪调用。
* [将 Application Insights SDK 添加到代码](../../azure-monitor/app/asp-net.md)，以便插入跟踪和日志调用





