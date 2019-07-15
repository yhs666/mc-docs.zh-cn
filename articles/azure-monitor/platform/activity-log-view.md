---
title: 查看 Azure Monitor 中的 Azure 活动日志事件
description: 在 Azure Monitor 中查看 Azure 活动日志并使用 PowerShell、CLI 和 REST API 进行检索。
author: lingliw
manager: digimobile
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/10/2019
ms.author: v-lingwu
ms.subservice: logs
ms.openlocfilehash: cedf61bc9f959c8a2365dc0e00ae67abead2dc22
ms.sourcegitcommit: fd927ef42e8e7c5829d7c73dc9864e26f2a11aaa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/04/2019
ms.locfileid: "67562896"
---
# <a name="view-and-retrieve-azure-activity-log-events"></a>查看和检索 Azure 活动日志事件

Azure 活动日志可以方便用户深入了解 Azure 中发生的订阅级别事件[](activity-logs-overview.md)。 本文详细介绍了如何使用不同的方法来查看和检索活动日志事件。

## <a name="azure-portal"></a>Azure 门户
在 Azure 门户的“监视器”菜单中查看所有资源的活动日志。  在该资源的菜单的“活动日志”选项中查看特定资源的活动日志。 

![查看活动日志](./media/activity-logs-overview/view-activity-log.png)

可以按以下字段筛选活动日志事件：

* **时间跨度**：事件的开始时间和结束时间。
* **类别**：[活动日志中的类别](activity-logs-overview.md#categories-in-the-activity-log)中所述的事件类别。
* **订阅**：一个或多个 Azure 订阅名称。
* **资源组**：所选订阅中的一个或多个资源组。
* **资源(名称)** - 特定资源的名称。
* **资源类型**：资源的类型，例如 _Microsoft.Compute/virtualmachines_。
* **操作名称** - Azure 资源管理器操作的名称，例如 _Microsoft.SQL/servers/Write_。
* **严重性**：事件的严重级别。 可用值为“信息性”、“警告”、“错误”、“严重”。    
* **事件发起者**：执行了操作的用户。
* **开放搜索**：开放的文本搜索框，可在所有事件的所有字段中搜索该字符串。

### <a name="view-change-history"></a>查看更改历史记录

查看活动日志时，可以查看在该事件时间范围内发生了哪些更改。 可以通过“更改历史记录”查看此信息。  从活动日志中选择一个需要深入了解的事件。 选择“更改历史记录(预览)”选项卡，查看与该事件关联的任何更改。 

![事件的更改历史记录列表](media/activity-logs-overview/change-history-event.png)

如果有任何与该事件关联的更改，则会看到一个列表，其中包含可以选择的更改。 此时会打开“更改历史记录(预览)”页。  在此页上，可以看到对资源的更改。 从以下示例可以看出，我们不仅能够看到 VM 更改了大小，而且能够看到更改前 VM 的大小，以及更改后的大小。

![显示了差异的更改历史记录页](media/activity-logs-overview/change-history-event-details.png)


## <a name="log-analytics-workspace"></a>Log Analytics 工作区
单击“活动日志”页顶部的“日志”，打开订阅的 [Activity Log Analytics 监视解决方案](activity-log-collect.md)。   这样就可以查看活动日志的分析，并使用 AzureActivity 表运行[日志查询](../log-query/log-query-overview.md)。  如果活动日志未连接到 Log Analytics 工作区，系统会提示你执行该配置。



## <a name="powershell"></a>PowerShell
使用 [Get-AzLog](https://docs.microsoft.com/powershell/module/az.monitor/get-azlog) cmdlet 从 PowerShell 检索活动日志。 下面是一些常见示例。

> [!NOTE]
> `Get-AzLog` 仅提供 15 天的历史记录。 使用 **-MaxEvents** 参数查询 15 天之外的最后 N 个事件。 若要访问超过 15 天的事件，请使用 REST API 或 SDK。 如果不包括 **StartTime**，则默认值为 **EndTime** 减去一小时。 如果不包括 **EndTime**，则默认值为当前时间。 所有时间均是 UTC 时间。


获取在特定日期时间之后创建的日志条目：

```powershell
Get-AzLog -StartTime 2016-03-01T10:30
```

在一个日期时间范围中获取日志条目：

```powershell
Get-AzLog -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

从特定资源组中获取日志条目︰

```powershell
Get-AzLog -ResourceGroup 'myrg1'
```

在一个日期时间范围中从特定资源提供程序获取日志条目：

```powershell
Get-AzLog -ResourceProvider 'Microsoft.Web' -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

获取特定调用方的日志条目：

```powershell
Get-AzLog -Caller 'myname@company.com'
```

获取最后 1000 个事件：

```powershell
Get-AzLog -MaxEvents 1000
```


## <a name="cli"></a>CLI
使用 [az monitor activity-log](cli-samples.md#view-activity-log-for-a-subscription) 从 CLI 检索活动日志。 下面是一些常见示例。


查看所有可用选项。

```azurecli
az monitor activity-log list -h
```

从特定资源组中获取日志条目︰

```azurecli
az monitor activity-log list --resource-group <group name>
```

获取特定调用方的日志条目：

```azurecli
az monitor activity-log list --caller myname@company.com
```

在日期范围内，按调用方获取某个资源类型的日志：

```azurecli
az monitor activity-log list --resource-provider Microsoft.Web \
    --caller myname@company.com \
    --start-time 2016-03-08T00:00:00Z \
    --end-time 2016-03-16T00:00:00Z
```

## <a name="rest-api"></a>REST API
使用 [Azure Monitor REST API](https://docs.microsoft.com/rest/api/monitor/) 从 REST 客户端检索活动日志。 下面是一些常见示例。

使用筛选器获取活动日志：

``` HTTP
GET https://management.azure.com/subscriptions/089bd33f-d4ec-47fe-8ba5-0753aa5c5b33/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01&$filter=eventTimestamp ge '2018-01-21T20:00:00Z' and eventTimestamp le '2018-01-23T20:00:00Z' and resourceGroupName eq 'MSSupportGroup'
```

使用筛选器和 select 获取活动日志：

```HTTP
GET https://management.azure.com/subscriptions/089bd33f-d4ec-47fe-8ba5-0753aa5c5b33/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01&$filter=eventTimestamp ge '2015-01-21T20:00:00Z' and eventTimestamp le '2015-01-23T20:00:00Z' and resourceGroupName eq 'MSSupportGroup'&$select=eventName,id,resourceGroupName,resourceProviderName,operationName,status,eventTimestamp,correlationId,submissionTimestamp,level
```

使用 select 获取活动日志：

```HTTP
GET https://management.azure.com/subscriptions/089bd33f-d4ec-47fe-8ba5-0753aa5c5b33/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01&$select=eventName,id,resourceGroupName,resourceProviderName,operationName,status,eventTimestamp,correlationId,submissionTimestamp,level
```

不使用筛选器或 select 获取活动日志：

```HTTP
GET https://management.azure.com/subscriptions/089bd33f-d4ec-47fe-8ba5-0753aa5c5b33/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01
```


## <a name="next-steps"></a>后续步骤

* [阅读活动日志概述](activity-logs-overview.md)
* [将活动日志存档到存储或将其流式传输到事件中心](activity-log-export.md)


