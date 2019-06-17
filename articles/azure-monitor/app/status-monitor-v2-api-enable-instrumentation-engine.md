---
title: Azure 状态监视器 v2 API 参考：启用检测引擎 | Azure Docs
description: 状态监视器 v2 API 参考 Enable-InstrumentationEngine。 无需重新部署网站即可监视网站性能。 使用托管在本地、VM 或 Azure 上的 ASP.NET Web 应用。
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
ms.openlocfilehash: a8e3be279957e3dba64f6a035eb0ab78cce2ec7c
ms.sourcegitcommit: f818003595bd7a6aa66b0d3e1e0e92e79b059868
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2019
ms.locfileid: "66732443"
---
# <a name="status-monitor-v2-api-enable-instrumentationengine-v021-alpha"></a>状态监视器 v2 API：Enable-InstrumentationEngine (v0.2.1-alpha)

本文档介绍作为 [Az.ApplicationMonitor PowerShell 模块](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/)成员提供的 cmdlet。

> [!IMPORTANT]
> 状态监视器 v2 目前以公共预览版提供。
> 此预览版在提供时没有附带服务级别协议，不建议将其用于生产工作负荷。 某些功能可能不受支持或者受限。
> 有关详细信息，请参阅[世纪互联 Azure 预览版补充使用条款](https://www.azure.cn/support/legal/preview-supplemental-terms/)

## <a name="description"></a>说明

此 cmdlet 将通过设置一些注册表项启用检测引擎。
重启 IIS 以使这些更改生效。

检测引擎可以补充 .NET SDK 收集的数据。
将收集描述托管进程执行的事件和消息。 包括但不限于依赖性结果代码、HTTP 谓词和 SQL 命令文本。 

在以下情况下启用检测引擎：
- 已使用 Enable cmdlet 启用了监视，但未启用检测引擎。
- 已使用 .NET SDK 手动检测应用程序，并希望收集其他遥测数据。

> [!IMPORTANT] 
> 此 cmdlet 需要具有管理员权限的 PowerShell 会话。

> [!NOTE] 
> 此 cmdlet 将要求你查看并接受我们的许可证和隐私声明。

> [!NOTE] 
> 检测引擎会增加额外的开销，默认情况下处于关闭状态。

## <a name="examples"></a>示例

```powershell
PS C:\> Enable-InstrumentationEngine
```

## <a name="parameters"></a>parameters 

### <a name="-acceptlicense"></a>-AcceptLicense
**可选。** 使用此开关接受无外设安装中的许可证和隐私声明。

### <a name="-verbose"></a>-Verbose
**通用参数。** 使用此开关输出详细的日志。

## <a name="output"></a>输出


#### <a name="example-output-from-successfully-enabling-the-instrumentation-engine"></a>成功启用检测引擎的示例输出

```
Configuring IIS Environment for instrumentation engine...
Configuring registry for instrumentation engine...
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
 - [获取配置](status-monitor-v2-api-get-config.md)以确认你的设置已正确记录。
 - [获取状态](status-monitor-v2-api-get-status.md)以检查监视。




