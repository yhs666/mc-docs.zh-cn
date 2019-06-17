---
title: Azure 状态监视器 v2 API 参考：获取配置 | Azure Docs
description: 状态监视器 v2 API 参考 Get-ApplicationInsightsMonitoringConfig。 无需重新部署网站即可监视网站性能。 使用托管在本地、VM 或 Azure 上的 ASP.NET Web 应用。
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
ms.openlocfilehash: fa4b8dc42b1d1d456268fb7c2a3549b01b539083
ms.sourcegitcommit: f818003595bd7a6aa66b0d3e1e0e92e79b059868
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2019
ms.locfileid: "66732440"
---
# <a name="status-monitor-v2-api-get-applicationinsightsmonitoringconfig-v021-alpha"></a>状态监视器 v2 API：Get-ApplicationInsightsMonitoringConfig (v0.2.1-alpha)

本文档介绍作为 [Az.ApplicationMonitor PowerShell 模块](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/)成员提供的 cmdlet。

> [!IMPORTANT]
> 状态监视器 v2 目前以公共预览版提供。
> 此预览版在提供时没有附带服务级别协议，不建议将其用于生产工作负荷。 某些功能可能不受支持或者受限。
> 有关详细信息，请参阅[世纪互联 Azure 预览版补充使用条款](https://www.azure.cn/support/legal/preview-supplemental-terms/)

## <a name="description"></a>说明

获取配置文件并将值打印到控制台。

> [!IMPORTANT] 
> 此 cmdlet 需要具有管理员权限的 PowerShell 会话。

## <a name="examples"></a>示例

```powershell
PS C:\> Get-ApplicationInsightsMonitoringConfig
```

## <a name="parameters"></a>parameters 

（不需要参数）

## <a name="output"></a>输出


#### <a name="example-output-from-reading-the-config-file"></a>读取配置文件的示例输出

```
RedfieldConfiguration:
Filters:
0)InstrumentationKey:  AppFilter: WebAppExclude MachineFilter: .*
1)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx2 AppFilter: WebAppTwo MachineFilter: .*
2)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxdefault AppFilter: .* MachineFilter: .*
```

## <a name="next-steps"></a>后续步骤

  查看遥测：
 - [浏览指标](../../azure-monitor/app/metrics-explorer.md)，以便监视性能和使用情况
- [搜索事件和日志](../../azure-monitor/app/diagnostic-search.md)，以便诊断问题
- [分析](../../azure-monitor/app/analytics.md)，以便进行更高级的查询
- [创建仪表板](../../azure-monitor/app/app-insights-dashboards.md)
 
 添加更多遥测：
 - [创建 Web 测试](monitor-web-app-availability.md)，以确保站点保持活动状态。
- [添加 Web 客户端遥测](../../azure-monitor/app/javascript.md)，以查看网页代码中的异常并将其插入跟踪调用。
- [将 Application Insights SDK 添加到代码](../../azure-monitor/app/asp-net.md)，以便插入跟踪和日志调用
 
 使用状态监视器 v2 执行更多操作：
 - 使用我们的指南对状态监视器 v2 进行[故障排除](status-monitor-v2-troubleshoot.md)。
 - 使用 [Set config](status-monitor-v2-api-set-config.md) cmdlet 更改此配置。




