---
title: Azure Status Monitor v2 故障排除和已知问题 | Azure Docs
description: 状态监视器 v2 的已知问题和故障排除示例。 无需重新部署网站即可监视网站性能。 使用托管在本地、VM 或 Azure 上的 ASP.NET Web 应用。
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
ms.openlocfilehash: 62053c252ab1ee095ac5ecd5f01f18818402bdb9
ms.sourcegitcommit: f818003595bd7a6aa66b0d3e1e0e92e79b059868
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2019
ms.locfileid: "66732433"
---
# <a name="troubleshooting-status-monitor-v2"></a>对状态监视器 v2 进行故障排除

启用监视时，可能会遇到阻止数据收集的问题。 本文档列出了所有已知的问题和故障排除示例。
如果你遇到未在此处列出的问题，可以从[此处](https://github.com/Microsoft/ApplicationInsights-Home/issues)联系我们。


> [!IMPORTANT]
> 状态监视器 v2 目前为公共预览版。
> 此预览版在提供时没有附带服务级别协议，不建议将其用于生产工作负荷。 某些功能可能不受支持或者受限。
> 有关详细信息，请参阅[世纪互联 Azure 预览版补充使用条款](https://www.azure.cn/support/legal/preview-supplemental-terms/)

## <a name="known-issues"></a>已知问题

### <a name="conflicting-dlls-in-an-applications-bin-directory"></a>应用程序的 bin 目录中的 Dll 冲突

如果 bin 目录中存在这些 DLL 中的任何一个，则监视可能会失败。

- Microsoft.ApplicationInsights.dll
- Microsoft.AspNet.TelemetryCorrelation.dll
- System.Diagnostics.DiagnosticSource.dll

这些 DLL 有一些包括在 Visual Studio 的默认应用程序模板中，即使你的应用程序不使用它们。
可以使用以下故障排除工具查看症状行为：

- PerfView：
    ```
    ThreadID="7,500" 
    ProcessorNumber="0" 
    msg="Found 'System.Diagnostics.DiagnosticSource, Version=4.0.2.1, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51' assembly, skipping attaching redfield binaries" 
    ExtVer="2.8.13.5972" 
    SubscriptionId="" 
    AppName="" 
    FormattedMessage="Found 'System.Diagnostics.DiagnosticSource, Version=4.0.2.1, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51' assembly, skipping attaching redfield binaries" 
    ```

- iisreset + 应用负载（无遥测）。 使用 Sysinternals（Handle.exe 和 ListDLLs.exe）进行调查
    ```
    .\handle64.exe -p w3wp | findstr /I "InstrumentationEngine AI. ApplicationInsights"
    E54: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll

    .\Listdlls64.exe w3wp | findstr /I "InstrumentationEngine AI ApplicationInsights"
    0x0000000009be0000  0x127000  C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\MicrosoftInstrumentationEngine_x64.dll
    0x0000000009b90000  0x4f000   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\Microsoft.ApplicationInsights.ExtensionsHost_x64.dll
    0x0000000004d20000  0xb2000   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\Microsoft.ApplicationInsights.Extensions.Base_x64.dll
    ```

### <a name="conflict-with-iis-shared-configuration"></a>与 IIS 共享配置冲突

如果你有 Web 服务器群集，则可能会使用[共享配置](https://docs.microsoft.com/iis/web-hosting/configuring-servers-in-the-windows-web-platform/shared-configuration_211)。 我们无法自动将我们的 HttpModule 注入到此共享配置中。每个 Web 服务器必须首先运行“启用”命令来将我们的 DLL 安装到其 GAC 中。

在运行“启用”命令后， 
1. 浏览到你的“共享配置”目录，并找到你的 `applicationHost.config` 文件。
2. 将以下行添加到 modules 部分中的配置中：
    ```
    <modules>
        <!-- Registered global managed http module handler. The 'Microsoft.AppInsights.IIS.ManagedHttpModuleHelper.dll' must be installed in the GAC before this config is applied. -->
        <add name="ManagedHttpModuleHelper" type="Microsoft.AppInsights.IIS.ManagedHttpModuleHelper.ManagedHttpModuleHelper, Microsoft.AppInsights.IIS.ManagedHttpModuleHelper, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35" preCondition="managedHandler,runtimeVersionv4.0" />
    </modules>
    ```
    


## <a name="troubleshooting"></a>故障排除
    
### <a name="troubleshooting-powershell"></a>PowerShell 故障排除

#### <a name="how-to-inspect-what-modules-are-available"></a>如何检查有哪些模块可用？
可以使用以下命令审核已安装的模块：`Get-Module -ListAvailable`

#### <a name="how-to-import-a-module-into-the-current-session"></a>如何将模块导入到当前会话中？
如果模块尚未加载到 PowerShell 会话中，可以使用以下命令手动加载：`Import-Module <path to psd1>`


### <a name="troubleshooting-the-status-monitor-v2-module"></a>对状态监视器 v2 模块进行故障排除

#### <a name="how-to-review-what-commands-are-available-in-the-status-monitor-v2-module"></a>如何查看状态监视器 v2 模块中有哪些命令可用？
- 运行 `Get-Command -Module Az.ApplicationMonitor` 命令来获取可用的命令：

    ```
    CommandType     Name                                               Version    Source
    -----------     ----                                               -------    ------
    Cmdlet          Disable-ApplicationInsightsMonitoring              0.2.1      Az.ApplicationMonitor
    Cmdlet          Disable-InstrumentationEngine                      0.2.1      Az.ApplicationMonitor
    Cmdlet          Enable-ApplicationInsightsMonitoring               0.2.1      Az.ApplicationMonitor
    Cmdlet          Enable-InstrumentationEngine                       0.2.1      Az.ApplicationMonitor
    Cmdlet          Get-ApplicationInsightsMonitoringConfig            0.2.1      Az.ApplicationMonitor
    Cmdlet          Get-ApplicationInsightsMonitoringStatus            0.2.1      Az.ApplicationMonitor
    Cmdlet          Set-ApplicationInsightsMonitoringConfig            0.2.1      Az.ApplicationMonitor
    ```

#### <a name="what-is-the-current-version-of-the-status-monitor-v2-module"></a>状态监视器 v2 模块的当前版本是多少？
- 运行 `Get-ApplicationInsightsMonitoringStatus` 命令来获取有关此模块的信息的输出：
    - PowerShell 模块版本
    - Application Insights SDK 版本
    - PowerShell 模块的文件路径
    
有关此 cmdlet 的详细用法说明，请查看 [API 参考](status-monitor-v2-api-get-status.md)。



### <a name="troubleshooting-running-processes"></a>对正在运行的进程进行故障排除

可以检查已检测计算机上的进程以查看是否已加载所有 DLL。
如果监视正常工作，则至少应加载 12 个 DLL。

- Cmd：`Get-ApplicationInsightsMonitoringStatus -InspectProcess`

有关此 cmdlet 的详细用法说明，请查看 [API 参考](status-monitor-v2-api-get-status.md)。


### <a name="collect-etw-logs-with-perfview"></a>使用 PerfView 收集 ETW 日志

#### <a name="setup"></a>设置

1. 从 https://github.com/Microsoft/perfview/releases 下载 PerfView.exe 和 PerfView64.exe
2. 启动 PerfView64.exe
3. 展开“高级选项”
4. 取消选中：
    - Zip
    - 合并
    - .NET 符号集合
5. 设置其他提供程序：`61f6ca3b-4b5f-5602-fa60-759a2a2d1fbd,323adc25-e39b-5c87-8658-2c1af1a92dc5,925fa42b-9ef6-5fa7-10b8-56449d7a2040,f7d60e07-e910-5aca-bdd2-9de45b46c560,7c739bb9-7861-412e-ba50-bf30d95eae36,61f6ca3b-4b5f-5602-fa60-759a2a2d1fbd,323adc25-e39b-5c87-8658-2c1af1a92dc5,252e28f4-43f9-5771-197a-e8c7e750a984`


#### <a name="collecting-logs"></a>收集日志

1. 在具有管理权限的 cmd 控制台中，执行 `iisreset /stop` 以关闭 IIS 和所有 Web 应用。
2. 在 PerfView 中，单击“开始收集”
3. 在具有管理权限的 cmd 控制台中，执行 `iisreset /start` 以启动 IIS。
4. 尝试浏览到你的应用。
5. 在你的应用完成加载后，返回到 PerfView 并单击“停止收集”



## <a name="next-steps"></a>后续步骤

- 查看 [API 参考](status-monitor-v2-overview.md#powershell-api-reference)来查找你可能遗漏的参数。
- 如果你遇到未在此处列出的问题，可以从[此处](https://github.com/Microsoft/ApplicationInsights-Home/issues)联系我们。




