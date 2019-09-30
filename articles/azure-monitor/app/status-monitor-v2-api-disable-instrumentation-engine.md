---
title: Azure 状态监视器 v2 API 参考：禁用检测引擎 | Microsoft Docs
description: 状态监视器 v2 API 参考。 Disable-InstrumentationEngine。 无需重新部署网站即可监视网站性能。 使用托管在本地、VM 或 Azure 上的 ASP.NET Web 应用。
services: application-insights
documentationcenter: .net
author: lingliw
manager: digimobile
ms.assetid: 769a5ea4-a8c6-4c18-b46c-657e864e24de
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
origin.date: 04/23/2019
ms.date: 6/4/2019
ms.author: v-lingwu
ms.openlocfilehash: e3ac97dd97ce3302e73d311d58ada51e6e2d4452
ms.sourcegitcommit: 2f2ced6cfaca64989ad6114a6b5bc76700870c1a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71329784"
---
# <a name="status-monitor-v2-api-disable-instrumentationengine"></a>状态监视器 v2 API：Disable-InstrumentationEngine

本文介绍属于 [Az.ApplicationMonitor PowerShell 模块](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/)的 cmdlet。

## <a name="description"></a>说明
通过删除一些注册表项来禁用检测引擎。
重启 IIS 以使这些更改生效。

> [!IMPORTANT] 
> 此 cmdlet 需要具有管理员权限的 PowerShell 会话。

## <a name="examples"></a>示例

```powershell
PS C:\> Disable-InstrumentationEngine
```

## <a name="parameters"></a>parameters 

### <a name="-verbose"></a>-Verbose
**通用参数。** 使用此开关可输出详细日志。

## <a name="output"></a>输出


#### <a name="example-output-from-successfully-disabling-the-instrumentation-engine"></a>成功禁用检测引擎的示例输出

```
Configuring IIS Environment for instrumentation engine...
Registry: removing 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\IISADMIN[Environment]'
Registry: removing 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W3SVC[Environment]'
Registry: removing 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WAS[Environment]'
Configuring registry for instrumentation engine...
```


## <a name="next-steps"></a>后续步骤

 使用状态监视器 v2 执行更多操作：
 - 使用我们的指南对状态监视器 v2 进行[故障排除](status-monitor-v2-troubleshoot.md)。
