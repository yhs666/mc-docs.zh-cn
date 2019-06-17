---
title: Azure 状态监视器 v2 API 参考：禁用监视 | Azure Docs
description: 状态监视器 v2 API 参考 Disable-ApplicationInsightsMonitoring。 无需重新部署网站即可监视网站性能。 使用托管在本地、VM 或 Azure 上的 ASP.NET Web 应用。
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
ms.openlocfilehash: 545c58150275ffb6ac3b7579b7b1c27f0f9cefc8
ms.sourcegitcommit: f818003595bd7a6aa66b0d3e1e0e92e79b059868
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2019
ms.locfileid: "66732451"
---
# <a name="status-monitor-v2-api-disable-applicationinsightsmonitoring-v021-alpha"></a>状态监视器 v2 API：Disable-ApplicationInsightsMonitoring (v0.2.1-alpha)

本文档介绍作为 [Az.ApplicationMonitor PowerShell 模块](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/)成员提供的 cmdlet。

> [!IMPORTANT]
> 状态监视器 v2 目前以公共预览版提供。
> 此预览版在提供时没有附带服务级别协议，不建议将其用于生产工作负荷。 某些功能可能不受支持或者受限。
> 有关详细信息，请参阅[世纪互联 Azure 预览版补充使用条款](https://www.azure.cn/support/legal/preview-supplemental-terms/)

## <a name="description"></a>说明

在目标计算机上禁用监视。
此 cmdlet 将删除对 IIS applicationHost.config 的编辑并删除注册表项。

> [!IMPORTANT] 
> 此 cmdlet 需要具有管理员权限的 PowerShell 会话。

## <a name="examples"></a>示例

```powershell
PS C:\> Disable-ApplicationInsightsMonitoring
```

## <a name="parameters"></a>parameters 

### <a name="-verbose"></a>-Verbose
**通用参数。** 使用此开关输出详细的日志。

## <a name="output"></a>输出


#### <a name="example-output-from-successfully-disabling-monitoring"></a>成功禁用监视的示例输出

```
Initiating Disable Process
Applying transformation to 'C:\Windows\System32\inetsrv\config\applicationHost.config'
'C:\Windows\System32\inetsrv\config\applicationHost.config' backed up to 'C:\Windows\System32\inetsrv\config\applicationHost.config.backup-2019-03-26_08-59-00z'
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
```


## <a name="next-steps"></a>后续步骤

 使用状态监视器 v2 执行更多操作：
 - 使用我们的指南对状态监视器 v2 进行[故障排除](status-monitor-v2-troubleshoot.md)。




