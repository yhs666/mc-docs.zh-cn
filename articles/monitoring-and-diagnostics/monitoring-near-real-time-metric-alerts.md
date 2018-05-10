---
title: Azure Monitor 中的准实时指标警报
description: 了解如何使用准实时指标警报以小到 1 分钟的频率监视 Azure 资源指标。
author: snehithm
manager: kmadnani1
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ''
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 03/26/2018
ms.author: v-yiso
ms.date: 05/14/2018
ms.openlocfilehash: b2293705171b20c37479765960735f75e0abbc25
ms.sourcegitcommit: 0b63440e7722942ee1cdabf5245ca78759012500
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/07/2018
---
# <a name="newer-metric-alerts-for-azure-services-in-the-azure-portal"></a>Azure 门户中 Azure 服务的新型指标警报
Azure Monitor 现在支持一种新型指标警报类型。 新型警报与[经典指标警报](insights-alerts-portal.md)在以下方面有所不同：

- **延迟降低**：新型指标警报的运行频率可达每分钟一次。 旧式指标警报每 5 分钟方可运行 1 次。 由于引入日志需要时间，日志警报的延迟仍然超过 1 分钟。 
- **支持多维指标**：支持对维度指标发出警报，从而可仅监视关注的指标段。 
- **更好地控制指标条件**：可以定义更丰富的警报规则。 新型警报支持监视指标的最大值、最小值、平均值和总值。 
- **综合监视多个指标**：可以使用单个规则监视多个指标（目前最多为两个指标）。 如果两个指标在指定时间段内违反其各自的阈值，则会触发警报。 
- **更好的通知系统**：所有新型警报均使用[操作组](monitoring-action-groups.md)，这些组是命名的通知和操作组，可以在多个警报中重复使用。 经典指标警报和旧版 Log Analytics 警报不使用操作组。 
- **日志中的指标**（受限公共预览版）：进入 Log Analytics 的日志数据现在可以提取并转换为 Azure Monitor 指标，然后就像其他指标一样，基于其发出警报。 

若要了解如何在 Azure 门户中创建新型指标警报，请参阅[在 Azure 门户中创建警报规则](monitor-alerts-unified-usage.md#create-an-alert-rule-with-the-azure-portal)。 创建后，可以按照[在 Azure 门户中管理警报](monitor-alerts-unified-usage.md#managing-your-alerts-in-azure-portal)中所述的步骤管理警报。


## <a name="portal-powershell-cli-rest-support"></a>门户、PowerShell、CLI、REST 支持
目前，仅可在 Azure 门户 或 REST API 中创建新型指标报警。 即将推出使用 PowerShell 和 Azure 命令行接口 (Azure CLI 2.0) 配置新型警报的支持功能。

## <a name="metrics-and-dimensions-supported"></a>指标和维度支持
新型指标警报支持针对使用维度的指标发出警报。 可以使用维度将指标筛选到适当级别。 所有受支持的指标以及适用的维度都可以从 [Azure Monitor - 指标资源管理器（预览）](monitoring-metric-charts.md)中进行浏览和可视化。

下面是新型警报支持的 Azure Monitor 指标源的完整列表：

|资源类型  |支持的维度  | 可用指标|
|---------|---------|----------------|
|Microsoft.ApiManagement/service     | 是        | [API 管理](monitoring-supported-metrics.md#microsoftapimanagementservice)|
|Microsoft.Automation/automationAccounts     |     是   | [自动化帐户](monitoring-supported-metrics.md#microsoftautomationautomationaccounts)|
|Microsoft.Batch/batchAccounts | 不适用| [Batch 帐户](monitoring-supported-metrics.md#microsoftbatchbatchaccounts)|
|Microsoft.Cache/Redis     |    不适用     |[Redis 缓存](monitoring-supported-metrics.md#microsoftcacheredis)|
|Microsoft.Compute/virtualMachines     |    不适用     | [虚拟机](monitoring-supported-metrics.md#microsoftcomputevirtualmachines)|
|Microsoft.Compute/virtualMachineScaleSets     |   不适用      |[虚拟机规模集](monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesets)|
|Microsoft.EventHub/namespaces     |  是      |[事件中心](monitoring-supported-metrics.md#microsofteventhubnamespaces)|
|Microsoft.Logic/workflows     |     不适用    |[逻辑应用](monitoring-supported-metrics.md#microsoftlogicworkflows) |
|Microsoft.Network/applicationGateways     |    不适用     | [应用程序网关](monitoring-supported-metrics.md#microsoftnetworkapplicationgateways) |
|Microsoft.Network/publicipaddresses     |  不适用       |[公共 IP 地址](monitoring-supported-metrics.md#microsoftnetworkpublicipaddresses)|
|Microsoft.Search/searchServices     |   不适用      |[搜索服务](monitoring-supported-metrics.md#microsoftsearchsearchservices)|
|Microsoft.ServiceBus/namespaces     |  是       |[服务总线](monitoring-supported-metrics.md#microsoftservicebusnamespaces)|
|Microsoft.Storage/storageAccounts     |    是     | [存储帐户](monitoring-supported-metrics.md#microsoftstoragestorageaccounts)|
|Microsoft.Storage/storageAccounts/services     |     是    | [Blob 服务](monitoring-supported-metrics.md#microsoftstoragestorageaccountsblobservices)、[文件服务](monitoring-supported-metrics.md#microsoftstoragestorageaccountsfileservices)、[队列服务](monitoring-supported-metrics.md#microsoftstoragestorageaccountsqueueservices)和[表服务](monitoring-supported-metrics.md#microsoftstoragestorageaccountstableservices)|
|Microsoft.StreamAnalytics/streamingjobs     |  不适用       | [流分析](monitoring-supported-metrics.md#microsoftstreamanalyticsstreamingjobs)|
|Microsoft.CognitiveServices/accounts     |    不适用     | [认知服务](monitoring-supported-metrics.md#microsoftcognitiveservicesaccounts)|
|Microsoft.OperationalInsights/workspaces（预览版） | 是|[Log Analytics 工作区](#log-analytics-logs-as-metrics-for-alerting)|
## <a name="payload-schema"></a>负载架构

使用适当配置的[操作组](monitoring-action-groups.md)时，POST 操作对于所有准新型指标警报包含以下 JSON 有效负载和架构：

```json
{"schemaId":"AzureMonitorMetricAlert","data":
    {
    "version": "2.0",
    "status": "Activated",
    "context": {
    "timestamp": "2018-02-28T10:44:10.1714014Z",
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/microsoft.insights/metricAlerts/StorageCheck",
    "name": "StorageCheck",
    "description": "",
    "conditionType": "SingleResourceMultipleMetricCriteria",
    "condition": {
      "windowSize": "PT5M",
      "allOf": [
        {
          "metricName": "Transactions",
          "dimensions": [
            {
              "name": "AccountResourceId",
              "value": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500"
            },
            {
              "name": "GeoType",
              "value": "Primary"
            }
          ],
          "operator": "GreaterThan",
          "threshold": "0",
          "timeAggregation": "PT5M",
          "metricValue": 1.0
        },
      ]
    },
    "subscriptionId": "00000000-0000-0000-0000-000000000000",
    "resourceGroupName": "Contoso",
    "resourceName": "diag500",
    "resourceType": "Microsoft.Storage/storageAccounts",
    "resourceId": "/subscriptions/1e3ff1c0-771a-4119-a03b-be82a51e232d/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500",
    "portalLink": "https://portal.azure.com/#resource//subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500"
  },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
    }
}
```

## <a name="next-steps"></a>后续步骤

* 详细了解新式[警报体验](monitoring-overview-unified-alerts.md)。
* 了解 [Azure 中的日志警报](monitor-alerts-unified-log.md)。
* 了解 [Azure 中的警报](monitoring-overview-alerts.md)。