---
title: "Azure Monitor PowerShell 快速入门示例。 | Microsoft Docs"
description: "使用 PowerShell 访问 Azure Monitor 功能，例如自动缩放、警报、webhook 和搜索活动日志。"
author: kamathashwin
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: c0761814-7148-4ab5-8c27-a2c9fa4cfef5
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 08/09/2017
ms.author: v-yiso
ms.date: 09/04/2017
ms.openlocfilehash: bc8ec44ae51c4fe483e84ccfa2415ee24ed5a052
ms.sourcegitcommit: 0f2694b659ec117cee0110f6e8554d96ee3acae8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/25/2017
---
# <a name="azure-monitor-powershell-quick-start-samples"></a>Azure Monitor PowerShell 快速启动示例
本文说明可帮助访问 Azure Monitor 功能的示例 PowerShell 命令。 Azure Monitor 允许基于配置的遥测数据值自动缩放云服务、虚拟机和 Web 应用，以及发送警报通知或调用 Web URL。

> [!NOTE]
> “Azure Insights”在 2016 年 9 月 25 日后称为 Azure Monitor。 但是，命名空间及以下命令仍包含“insights”。
> 
> 

## <a name="set-up-powershell"></a>设置 PowerShell
如果尚未安装，请在计算机上安装要运行的 PowerShell。 有关详细信息，请参阅[如何安装和配置 PowerShell](../powershell-install-configure.md)。

## <a name="examples-in-this-article"></a>本文中的示例
本文中的示例演示如何使用 Azure Monitor cmdlet。 还可以在 [Azure Monitor (Insights) Cmdlet](https://msdn.microsoft.com/library/azure/mt282452#40v=azure.200#41.aspx) 上查看 Azure Monitor PowerShell cmdlet 的完整列表。

## <a name="sign-in-and-use-subscriptions"></a>登录并使用订阅
首先，登录 Azure 订阅。

```PowerShell
Login-AzureRmAccount
```

这要求进行登录。 执行此操作后，会显示帐户、TenantId 和默认的订阅 ID。 所有 Azure cmdlet 都可用于默认订阅的上下文。 若要查看有权访问的订阅的列表，请使用以下命令。

```PowerShell
Get-AzureRmSubscription
```

要将工作上下文更改为其他订阅，请使用以下命令。

```PowerShell
Set-AzureRmContext -SubscriptionId <subscriptionid>
```


## <a name="retrieve-activity-log-for-a-subscription"></a>检索订阅的活动日志
使用 `Get-AzureRmLog` cmdlet。  下面是一些常见示例。

从此时间/日期中获取要显示的日志条目︰

```PowerShell
Get-AzureRmLog -StartTime 2016-03-01T10:30
```

在一个时间/日期范围中获取日志条目︰

```PowerShell
Get-AzureRmLog -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

从特定资源组中获取日志条目︰

```PowerShell
Get-AzureRmLog -ResourceGroup 'myrg1'
```

在一个时间/日期范围中从特定资源提供程序获取日志条目︰

```PowerShell
Get-AzureRmLog -ResourceProvider 'Microsoft.Web' -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

获取特定调用方的所有日志项︰

```PowerShell
Get-AzureRmLog -Caller 'myname@company.com'
```

以下命令从活动日志中检索最后 1000 个事件：

```PowerShell
Get-AzureRmLog -MaxEvents 1000
```

`Get-AzureRmLog` 支持许多其他参数。 有关详细信息，请参阅 `Get-AzureRmLog` 参考文档。

> [!NOTE]
> `Get-AzureRmLog` 仅提供 15 天的历史记录。 使用 **-MaxEvents** 参数可查询 15 天之外的最后 N 个事件。 若要访问超过 15 天的事件，请使用 REST API 或 SDK（使用 SDK 的 C# 示例）。 如果不包括 **StartTime**，则默认值为 **EndTime** 减去一小时。 如果不包括 **EndTime**，则默认值为当前时间。 所有时间均是 UTC 时间。
> 
> 

