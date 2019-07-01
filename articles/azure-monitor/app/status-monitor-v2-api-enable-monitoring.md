---
title: Azure 状态监视器 v2 API 参考：启用监视 | Azure Docs
description: 状态监视器 v2 API 参考 Enable-ApplicationInsightsMonitoring。 无需重新部署网站即可监视网站性能。 使用托管在本地、VM 或 Azure 上的 ASP.NET Web 应用。
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
ms.openlocfilehash: 92c8a7da43d8ef9b094f09b39ff2b7697f1d732d
ms.sourcegitcommit: 5fc46672ae90b6598130069f10efeeb634e9a5af
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/19/2019
ms.locfileid: "67236511"
---
# <a name="status-monitor-v2-api-enable-applicationinsightsmonitoring-v021-alpha"></a>状态监视器 v2 API：Enable-ApplicationInsightsMonitoring (v0.2.1-alpha)

本文档介绍作为 [Az.ApplicationMonitor PowerShell 模块](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/)成员提供的 cmdlet。

> [!IMPORTANT]
> 状态监视器 v2 目前以公共预览版提供。
> 此预览版在提供时没有附带服务级别协议，不建议将其用于生产工作负荷。 某些功能可能不受支持或者受限。
> 有关详细信息，请参阅[世纪互联 Azure 预览版补充使用条款](https://www.azure.cn/support/legal/preview-supplemental-terms/)

## <a name="description"></a>说明

对目标计算机上的 IIS 应用程序启用无代码附加监视。
此 cmdlet 将修改 IIS applicationHost.config 并设置一些注册表项。
此 cmdlet 还会创建一个 applicationinsights.ikey.config，用于定义哪个应用程序使用哪个检测密钥。
IIS 在启动时会加载 RedfieldModule，从而在应用程序启动时将 Application Insights SDK 注入到这些应用程序中。
重启 IIS 以使更改生效。

启用监视后，我们建议使用[实时指标](live-stream.md)来快速观察你的应用程序是否正在向我们发送遥测数据。


> [!NOTE] 
> 若要开始，必须有一个检测密钥。 有关详细信息，请参阅[创建新资源](create-new-resource.md#copy-the-instrumentation-key)。


> [!IMPORTANT] 
> 此 cmdlet 要求使用管理员权限和提升的执行策略建立 PowerShell 会话。 有关详细信息，请参阅[此文](status-monitor-v2-detailed-instructions.md#run-powershell-as-administrator-with-an-elevated-execution-policy)。

> [!NOTE] 
> 此 cmdlet 要求查看并接受我们的许可条款和隐私声明。


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


### <a name="-enableinstrumentationengine"></a>-EnableInstrumentationEngine
**可选。** 使用此开关可让检测引擎收集执行托管进程期间发生的事件和相关消息。 包括但不限于依赖性结果代码、HTTP 谓词和 SQL 命令文本。 检测引擎会增加额外的开销，默认情况下处于关闭状态。

### <a name="-acceptlicense"></a>-AcceptLicense
**可选。** 使用此开关可在无外设安装中接受许可条款和隐私声明。

### <a name="-verbose"></a>-Verbose
**通用参数。** 使用此开关可输出详细日志。

### <a name="-whatif"></a>-WhatIf 
**通用参数。** 使用此开关可以测试和验证输入参数，而无需真正启用监视。

## <a name="output"></a>输出


#### <a name="example-output-from-a-successful-enablement"></a>成功启用后的示例输出

```
Initiating Disable Process
Applying transformation to 'C:\Windows\System32\inetsrv\config\applicationHost.config'
'C:\Windows\System32\inetsrv\config\applicationHost.config' backed up to 'C:\Windows\System32\inetsrv\config\applicationHost.config.backup-2019-03-26_08-59-52z'
in :1,237
No element in the source document matches '/configuration/location[@path='']/system.webServer/modules/add[@name='ManagedHttpModuleHelper']'
Not executing RemoveAll (transform line 1, 546)
Transformation to 'C:\Windows\System32\inetsrv\config\applicationHost.config' was successfully applied. Operation: 'disable'
GAC Module will not be removed, since this operation might cause IIS instabilities
Configuring IIS Environment for codeless attach...
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\IISADMIN[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W3SVC[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WAS[Environment]
Configuring IIS Environment for instrumentation engine...
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\IISADMIN[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W3SVC[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WAS[Environment]
Configuring registry for instrumentation engine...
Successfully disabled Application Insights Status Monitor
Installing GAC module 'C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\0.2.0\content\Runtime\Microsoft.AppInsights.IIS.ManagedHttpModuleHelper.dll'
Applying transformation to 'C:\Windows\System32\inetsrv\config\applicationHost.config'
Found GAC module Microsoft.AppInsights.IIS.ManagedHttpModuleHelper.ManagedHttpModuleHelper, Microsoft.AppInsights.IIS.ManagedHttpModuleHelper, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35
'C:\Windows\System32\inetsrv\config\applicationHost.config' backed up to 'C:\Windows\System32\inetsrv\config\applicationHost.config.backup-2019-03-26_08-59-52z_1'
Transformation to 'C:\Windows\System32\inetsrv\config\applicationHost.config' was successfully applied. Operation: 'enable'
Configuring IIS Environment for codeless attach...
Configuring IIS Environment for instrumentation engine...
Configuring registry for instrumentation engine...
Updating app pool permissions...
Successfully enabled Application Insights Status Monitor
```

## <a name="next-steps"></a>后续步骤

  查看遥测：
 - [浏览指标](../../azure-monitor/app/metrics-explorer.md)，以便监视性能和使用情况
- [搜索事件和日志](../../azure-monitor/app/diagnostic-search.md)以诊断问题
- [分析](../../azure-monitor/app/analytics.md)，以便进行更高级的查询
 
 添加更多遥测：
 - [创建 Web 测试](monitor-web-app-availability.md)，以确保站点保持活动状态。
- [添加 Web 客户端遥测](../../azure-monitor/app/javascript.md)，以查看网页代码中的异常并将其插入跟踪调用。
- [将 Application Insights SDK 添加到代码](../../azure-monitor/app/asp-net.md)，以便插入跟踪和日志调用
 
 使用状态监视器 v2 执行更多操作：
 - 使用我们的指南对状态监视器 v2 进行[故障排除](status-monitor-v2-troubleshoot.md)。
 - [获取配置](status-monitor-v2-api-get-config.md)以确认是否正确记录了你的设置。
 - [获取状态](status-monitor-v2-api-get-status.md)以检查监视。




