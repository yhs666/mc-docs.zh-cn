---
title: Azure Monitor 中的指标警报支持的资源
description: Azure Monitor 中的指标警报的支持指标和日志参考
author: lingliw
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
origin.date: 06/29/2018
ms.date: 11/26/2018
ms.author: v-lingwu
ms.component: alerts
ms.openlocfilehash: b97cb55cdcf7ca58154b10733cf4191ab13849aa
ms.sourcegitcommit: 579d4e19c2069ba5c7d5cb7e9b233744cc90d1f5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "53219515"
---
# <a name="supported-resources-for-metric-alerts-in-azure-monitor"></a>Azure Monitor 中的指标警报支持的资源

Azure Monitor 现在支持[新型指标警报类型](monitoring-overview-alerts.md)，它比旧式[经典指标警报](monitoring-overview-alerts-classic.md)具有显著的优势。 指标可用于 [Azure 服务的大型列表](monitoring-supported-metrics.md)。 新型警报支持资源类型的一个（不断增长的）子集。 本文列出了该子集。

还可以在常用 Log Analytics 日志中使用新型指标警报，这些警报作为日志中指标的一部分提取为指标
 
> [!NOTE]
> 只有在选定期间内存在其数据时，特定的指标和/或维度才会显示。 在美国东部、美国中西部和西欧具有 Azure Log Analytics 工作区的客户可使用这些指标。 Log Analytics 中的指标目前处于公共预览状态，日后可能会作出更改。

## <a name="portal-powershell-cli-rest-support"></a>门户、PowerShell、CLI、REST 支持
目前，仅可在 Azure 门户、[REST API](https://docs.microsoft.com/rest/api/monitor/metricalerts/createorupdate) 或[资源管理器模板](monitoring-create-metric-alerts-with-templates.md)中创建新型指标警报。 对于使用 PowerShell 和 Azure CLI 2.0 及更高版本配置新型警报的支持即将推出。

## <a name="metrics-and-dimensions-supported"></a>指标和维度支持
新型指标警报支持针对使用维度的指标发出警报。 可以使用维度将指标筛选到适当级别。 所有受支持的指标以及适用的维度都可以从 [Azure Monitor - 指标资源管理器](monitoring-metric-charts.md)中进行浏览和可视化。

下面是新型警报支持的 Azure Monitor 指标源的完整列表：

|资源类型  |支持维度  | 可用指标|
|---------|---------|----------------|
|Microsoft.ApiManagement/service     | 是        | [API 管理](monitoring-supported-metrics.md#microsoftapimanagementservice)|
|Microsoft.Automation/automationAccounts     |     是   | [自动化帐户](monitoring-supported-metrics.md#microsoftautomationautomationaccounts)|
|Microsoft.Batch/batchAccounts | 不适用| [Batch 帐户](monitoring-supported-metrics.md#microsoftbatchbatchaccounts)|
|Microsoft.Cache/Redis     |    不适用     |[Redis 缓存](monitoring-supported-metrics.md#microsoftcacheredis)|
|Microsoft.CognitiveServices/accounts     |    不适用     | [认知服务](monitoring-supported-metrics.md#microsoftcognitiveservicesaccounts)|
|Microsoft.Compute/virtualMachines     |    不适用     | [虚拟机](monitoring-supported-metrics.md#microsoftcomputevirtualmachines)|
|Microsoft.Compute/virtualMachineScaleSets     |   不适用      |[虚拟机规模集](monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesets)|
|Microsoft.DBforMySQL/servers     |   不适用      |[适用于 MySQL 的 DB](monitoring-supported-metrics.md#microsoftdbformysqlservers)|
|Microsoft.DBforPostgreSQL/servers     |    不适用     | [适用于 PostgreSQL 的 DB](monitoring-supported-metrics.md#microsoftdbforpostgresqlservers)|
|Microsoft.EventHub/namespaces     |  是      |[事件中心](monitoring-supported-metrics.md#microsofteventhubnamespaces)|
|Microsoft.KeyVault/vaults| 否 | [保管库](monitoring-supported-metrics.md#microsoftkeyvaultvaults)|
|Microsoft.Logic/workflows     |     不适用    |[逻辑应用](monitoring-supported-metrics.md#microsoftlogicworkflows) |
|Microsoft.Network/applicationGateways     |    不适用     | [应用程序网关](monitoring-supported-metrics.md#microsoftnetworkapplicationgateways) |
|Microsoft.Network/expressRouteCircuits | 不适用 |  [Express Route 线路](monitoring-supported-metrics.md#microsoftnetworkexpressroutecircuits) |
|Microsoft.Network/loadBalancers（仅限标准 SKU）| 是| [负载均衡器](monitoring-supported-metrics.md#microsoftnetworkloadbalancers) |
|Microsoft.Network/publicipaddresses     |  不适用       |[公共 IP 地址](monitoring-supported-metrics.md#microsoftnetworkpublicipaddresses)|
|Microsoft.PowerBIDedicated/capacities | 不适用 | [容量](monitoring-supported-metrics.md#microsoftpowerbidedicatedcapacities)|
|Microsoft.Network/trafficManagerProfiles | 是 | [流量管理器配置文件](monitoring-supported-metrics.md#microsoftnetworktrafficmanagerprofiles) |
|Microsoft.Search/searchServices     |   不适用      |[搜索服务](monitoring-supported-metrics.md#microsoftsearchsearchservices)|
|Microsoft.ServiceBus/namespaces     |  是       |[服务总线](monitoring-supported-metrics.md#microsoftservicebusnamespaces)|
|Microsoft.Storage/storageAccounts     |    是     | [存储帐户](monitoring-supported-metrics.md#microsoftstoragestorageaccounts)|
|Microsoft.Storage/storageAccounts/services     |     是    | [Blob 服务](monitoring-supported-metrics.md#microsoftstoragestorageaccountsblobservices)、[文件服务](monitoring-supported-metrics.md#microsoftstoragestorageaccountsfileservices)、[队列服务](monitoring-supported-metrics.md#microsoftstoragestorageaccountsqueueservices)和[表服务](monitoring-supported-metrics.md#microsoftstoragestorageaccountstableservices)|
|Microsoft.StreamAnalytics/streamingjobs     |  不适用       | [流分析](monitoring-supported-metrics.md#microsoftstreamanalyticsstreamingjobs)|
| Microsoft.Web/serverfarms | 是 | [应用服务计划](monitoring-supported-metrics.md#microsoftwebserverfarms)  |
| Microsoft.Web/sites | 是 | [应用程序服务](monitoring-supported-metrics.md#microsoftwebsites-excluding-functions)和 [Functions](monitoring-supported-metrics.md#microsoftwebsites-functions)|
| Microsoft.Web/sites/slots | 是 | [应用服务槽](monitoring-supported-metrics.md#microsoftwebsitesslots)|
|Microsoft.OperationalInsights/workspaces| 是|[Log Analytics 工作区](monitoring-supported-metrics.md#microsoftoperationalinsightsworkspaces)|



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
    "portalLink": "https://portal.azure.cn/#resource//subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500"
  },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
    }
}
```

## <a name="next-steps"></a>后续步骤

* 了解 [Azure 中的警报](monitoring-overview-alerts.md)。