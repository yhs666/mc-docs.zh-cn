---
title: Azure 状态监视器 v2 API 参考：设置配置 | Azure Docs
description: 状态监视器 v2 API 参考 Set-ApplicationInsightsMonitoringConfig。 无需重新部署网站即可监视网站性能。 使用托管在本地、VM 或 Azure 上的 ASP.NET Web 应用。
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
ms.openlocfilehash: c9b730be984bc5bb7a2e6ce756fbcb9471e0b106
ms.sourcegitcommit: f818003595bd7a6aa66b0d3e1e0e92e79b059868
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2019
ms.locfileid: "66732438"
---
# <a name="status-monitor-v2-api-set-applicationinsightsmonitoringconfig-v021-alpha"></a>状态监视器 v2 API：Set-ApplicationInsightsMonitoringConfig (v0.2.1-alpha)

本文档介绍作为 [Az.ApplicationMonitor PowerShell 模块](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/)成员提供的 cmdlet。

> [!IMPORTANT]
> 状态监视器 v2 目前以公共预览版提供。
> 此预览版在提供时没有附带服务级别协议，不建议将其用于生产工作负荷。 某些功能可能不受支持或者受限。
> 有关详细信息，请参阅[世纪互联 Azure 预览版补充使用条款](https://www.azure.cn/support/legal/preview-supplemental-terms/)

## <a name="description"></a>说明

在不重复完全重新安装的情况下设置配置文件。 重启 IIS 以使更改生效。

> [!IMPORTANT] 
> 此 cmdlet 需要具有管理员权限的 PowerShell 会话。


## <a name="examples"></a>示例

### <a name="example-with-single-instrumentation-key"></a>使用单个检测密钥的示例
在此示例中，将为当前计算机上的所有应用程序分配一个检测密钥。

```powershell
PS C:\> Enable-ApplicationInsightsMonitoring -InstrumentationKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

### <a name="example-with-instrumentation-key-map"></a>使用检测密钥映射的示例
在此示例中， 
- `MachineFilter` 将使用 `'.*'` 通配符匹配当前计算机。
- `AppFilter='WebAppExclude'` 提供 `null` InstrumentationKey。 不会检测此应用。
- `AppFilter='WebAppOne'` 将为此特定应用分配一个唯一的检测密钥。
- `AppFilter='WebAppTwo'` 还将为此特定应用分配一个唯一的检测密钥。
- 最后，`AppFilter` 还使用 `'.*'` 通配符来匹配与之前规则不匹配的所有其他 Web 应用，并分配默认检测密钥。
- 添加空格仅为了可读性。

```powershell
PS C:\> Enable-ApplicationInsightsMonitoring -InstrumentationKeyMap 
    @(@{MachineFilter='.*';AppFilter='WebAppExclude'},
      @{MachineFilter='.*';AppFilter='WebAppOne';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx1'},
      @{MachineFilter='.*';AppFilter='WebAppTwo';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx2'},
      @{MachineFilter='.*';AppFilter='.*';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxdefault'})

```


## <a name="parameters"></a>parameters 

### <a name="-instrumentationkey"></a>-InstrumentationKey
**必需。** 使用此参数提供单个 iKey 以供目标计算机上的所有应用程序使用。

### <a name="-instrumentationkeymap"></a>-InstrumentationKeyMap
**必需。** 使用此参数可以提供多个 ikey，以及哪些应用要使用哪个 ikey 的映射。 通过设置 MachineFilter，可以为多台计算机创建单个安装脚本。 

> [!IMPORTANT] 
> 应用程序将按照提供规则的顺序与规则相匹配。 因此，你应该首先指定最特定的规则，最后指定最通用的规则。

#### <a name="schema"></a>架构
`@(@{MachineFilter='.*';AppFilter='.*';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'})`

- **MachineFilter** 是计算机或 VM 名称所需的 c# 正则表达式。
    - “.*”将匹配所有项
    - “ComputerName”将仅匹配具有该确切名称的计算机。
- **AppFilter** 是计算机或 VM 名称所需的 c# 正则表达式。
    - “.*”将匹配所有项
    - “ApplicationName”将仅匹配具有该确切名称的 IIS 应用程序。
- 需要 **InstrumentationKey** 才能监视与上述两个筛选器匹配的应用程序。
    - 如果要定义排除监视的规则，请将此值保留为 null


### <a name="-verbose"></a>-Verbose
**通用参数。** 使用此开关输出详细的日志。


## <a name="output"></a>输出

默认情况下没有输出。

#### <a name="example-verbose-output-from-setting-the-config-file-via--instrumentationkey"></a>通过 -InstrumentationKey 设置配置文件的示例详细输出

```
VERBOSE: Operation: InstallWithIkey
VERBOSE: InstrumentationKeyMap parsed:
Filters:
0)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx AppFilter: .* MachineFilter: .*
VERBOSE: set config file
VERBOSE: Config File Path:
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\applicationInsights.ikey.config
```

#### <a name="example-verbose-output-from-setting-the-config-file-via--instrumentationkeymap"></a>通过 -InstrumentationKeyMap 设置配置文件的示例详细输出

```
VERBOSE: Operation: InstallWithIkeyMap
VERBOSE: InstrumentationKeyMap parsed:
Filters:
0)InstrumentationKey:  AppFilter: WebAppExclude MachineFilter: .*
1)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx2 AppFilter: WebAppTwo MachineFilter: .*
2)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxdefault AppFilter: .* MachineFilter: .*
VERBOSE: set config file
VERBOSE: Config File Path:
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\applicationInsights.ikey.config
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




