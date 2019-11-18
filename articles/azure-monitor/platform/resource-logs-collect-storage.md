---
title: 将 Azure 资源日志存档到存储帐户 | Microsoft Docs
description: 了解如何存档 Azure 资源日志，将其长期保留在存储帐户中。
author: lingliw
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/20/2019
ms.author: v-lingwu
ms.subservice: logs
ms.openlocfilehash: f9b1e54853e61f2a357320ebc23d5b46fb2549bd
ms.sourcegitcommit: b09d4b056ac695ba379119eb9e458a945b0a61d9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/28/2019
ms.locfileid: "72971150"
---
# <a name="archive-azure-resource-logs-to-storage-account"></a>将 Azure 资源日志存档到存储帐户
Azure 中的[资源日志](resource-logs-overview.md)提供有关 Azure 资源内部操作的丰富、频繁的数据。 本文介绍如何将资源日志收集到到 Azure 存储帐户，以便保留要存档的数据。

## <a name="prerequisites"></a>先决条件
需[创建 Azure 存储帐户](../../storage/common/storage-quickstart-create-account.md)（如果还没有）。 只要配置设置的用户同时拥有两个订阅的相应 RBAC 访问权限，存储帐户就不必位于发送日志的资源所在的订阅中。

不应使用其中存储了其他非监视数据的现有存储帐户，以便更好地控制监视数据所需的访问权限。 不过，如果还要将[活动日志](activity-logs-overview.md)存档到存储帐户，则可以选择使用该存储帐户在一个中心位置保留所有监视数据。

## <a name="create-a-diagnostic-setting"></a>创建诊断设置
默认不会收集资源日志。 需要通过创建 Azure 资源的诊断设置，在 Azure 存储帐户和其他目标中收集资源日志。 有关详细信息，请参阅[创建诊断设置以收集 Azure 中的日志和指标](diagnostic-settings.md)。


## <a name="data-retention"></a>数据保留
保留策略定义存储在存储帐户中的每个日志类别和指标数据的保留天数。 可将保留策略设置为 0 到 365 之间的任意天数。 保留策略为零表示系统会无限期存储该日志类别的事件。

保留策略按天应用，因此在一天结束时 (UTC)，会删除当天已超过保留策略期限的日志。 例如，假设保留策略的期限为一天，则在今天开始时，会删除前天的日志。 删除过程从午夜 (UTC) 开始，但请注意，可能最多需要 24 小时才能将日志从存储帐户中删除。 


## <a name="schema-of-resource-logs-in-storage-account"></a>存储帐户中的资源日志的架构

创建诊断设置以后，一旦在已启用的日志类别之一中出现事件，就会在存储帐户中创建存储容器。 容器中的 blob 使用以下命名约定：

```
insights-logs-{log category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/RESOURCEGROUPS/{resource group name}/PROVIDERS/{resource provider name}/{resource type}/{resource name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

例如，网络安全组的 blob 的名称可能如下所示：

```
insights-logs-networksecuritygrouprulecounter/resourceId=/SUBSCRIPTIONS/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUP/TESTNSG/y=2016/m=08/d=22/h=18/m=00/PT1H.json
```

每个 PT1H.json blob 都包含一个 JSON blob，其中的事件为在 blob URL 中指定的小时（例如 h=12）内发生的。 在当前的小时内发生的事件将附加到 PT1H.json 文件。 分钟值始终为 00 (m=00)，因为资源日志事件按小时细分成单个 blob。

在 PT1H.json 文件中，每个事件都按以下格式存储。 这将使用通用的顶级架构，但每个 Azure 服务并不相同，详见[资源日志架构](resource-logs-overview.md#resource-logs-schema)。

``` JSON
{"time": "2016-07-01T00:00:37.2040000Z","systemId": "46cdbb41-cb9c-4f3d-a5b4-1d458d827ff1","category": "NetworkSecurityGroupRuleCounter","resourceId": "/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/TESTNSG","operationName": "NetworkSecurityGroupCounters","properties": {"vnetResourceGuid": "{12345678-9012-3456-7890-123456789012}","subnetPrefix": "10.3.0.0/24","macAddress": "000123456789","ruleName": "/subscriptions/ s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg/securityRules/default-allow-rdp","direction": "In","type": "allow","matchedConnections": 1988}}
```

> [!NOTE]
> 平台日志使用 [JSON 行](http://jsonlines.org/)写入到 blob 存储，其中每个事件都是一行，换行符表示新事件。 此格式已在 2018 年 11 月实现。 在此日期之前，日志以记录的 json 数组形式写入到 blob 存储，详见[为存档到存储帐户的 Azure Monitor 平台日志的格式更改做准备](resource-logs-blob-format.md)。

## <a name="next-steps"></a>后续步骤

* [下载 blob 进行分析](../../storage/blobs/storage-quickstart-blobs-dotnet.md)。
* [详细阅读资源日志](../../azure-monitor/platform/resource-logs-overview.md)。

