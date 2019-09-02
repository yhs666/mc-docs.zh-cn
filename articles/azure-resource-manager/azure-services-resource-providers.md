---
title: Azure 服务的 Azure 资源管理器资源提供程序
description: 列出 Azure 资源管理器的所有资源提供程序命名空间，并显示该命名空间的 Azure 服务。
author: rockboyfor
ms.service: azure-resource-manager
ms.topic: reference
origin.date: 06/25/2019
ms.date: 07/22/2019
ms.author: v-yeche
ms.openlocfilehash: afd68b34bd5f6e63cd20013499da4fd780a8a397
ms.sourcegitcommit: 5fea6210f7456215f75a9b093393390d47c3c78d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/19/2019
ms.locfileid: "68337303"
---
<!--Verify sucessfully-->
# <a name="resource-providers-for-azure-services"></a>Azure 服务的资源提供程序

本文介绍资源提供程序命名空间如何映射到 Azure 服务。

## <a name="match-resource-provider-to-service"></a>将资源提供程序匹配到服务

| 资源提供程序命名空间 | Azure 服务 |
| --------------------------- | ------------- |
| Microsoft.Advisor | [Azure 顾问](../advisor/index.md) |
| Microsoft.AlertsManagement | [Azure Monitor](../azure-monitor/index.yml) |
| Microsoft.AnalysisServices | [Azure Analysis Services](/analysis-services/) |
| Microsoft.ApiManagement | [API 管理](../api-management/index.yml) |
| Microsoft.Authorization | [Azure 资源管理器](index.yml) |
| Microsoft.Automation | [自动化](../automation/index.yml) |
| Microsoft.AzureActiveDirectory | [Azure Active Directory B2C](../active-directory-b2c/index.yml) |
| Microsoft.AzureStack | core |
| Microsoft.Batch | [批处理](../batch/index.yml) |
| Microsoft.Cache | [用于 Redis 的 Azure 缓存](/azure-cache-for-redis/) |
| Microsoft.Cdn | [内容分发网络](/cdn/) |
| Microsoft.ClassicCompute | 经典部署模型虚拟机 |
| Microsoft.ClassicInfrastructureMigrate | 经典部署模型迁移 |
| Microsoft.ClassicNetwork | 经典部署模型虚拟网络 |
| Microsoft.ClassicStorage | 经典部署模型存储 |
| Microsoft.ClassicSubscription | 经典部署模型 |
| Microsoft.CognitiveServices | [认知服务](/cognitive-services/) |
| Microsoft.Compute | [虚拟机](/virtual-machines/) |
| Microsoft.ContainerRegistry | [容器注册表](/container-registry/) |
| Microsoft.ContainerService | [Azure Kubernetes 服务 (AKS)](/aks/) |
| Microsoft.DataFactory | [数据工厂](/data-factory/) |
| Microsoft.DBforMariaDB | [Azure Database for MariaDB](/mariadb/) |
| Microsoft.DBforMySQL | [Azure Database for MySQL](/mysql/) |
| Microsoft.DBforPostgreSQL | [Azure Database for PostgreSQL](/postgresql/) |
| Microsoft.Devices | [IoT 中心](/iot-hub/)<br />[IoT 中心设备预配服务](/iot-dps/) |
| Microsoft.DocumentDB | [Azure Cosmos DB](../cosmos-db/index.yml) |
| Microsoft.EventHub | [事件中心](../event-hubs/index.yml) |
| Microsoft.Features | [Azure 资源管理器](index.yml) |
| Microsoft.GuestConfiguration | [Azure Policy](../governance/policy/index.yml) |
| Microsoft.HDInsight | [HDInsight](../hdinsight/index.yml) |
| Microsoft.ImportExport | [Azure 导入/导出](../storage/common/storage-import-export-service.md) |
| microsoft.insights | [Azure Monitor](../azure-monitor/index.yml) |
| Microsoft.KeyVault | [密钥保管库](../key-vault/index.yml) |
| Microsoft.Kusto | [Azure 数据资源管理器](../data-explorer/index.yml) |
| Microsoft.LogAnalytics | [Azure Monitor](../azure-monitor/index.yml) |
| Microsoft.Logic | [逻辑应用](../logic-apps/index.yml) |
| Microsoft.ManagedIdentity | Azure 资源的托管标识 |
| Microsoft.Management | [管理组](/governance/management-groups/) |
| Microsoft.Media | [媒体服务](../media-services/index.yml) |
| Microsoft.Network | [虚拟网络](../virtual-network/index.yml)<br />[负载均衡器](../load-balancer/index.yml)<br />[应用程序网关](../application-gateway/index.yml)<br />[Azure DNS](../dns/index.yml)<br />[ExpressRoute](../expressroute/index.yml)<br />[VPN 网关](../vpn-gateway/index.yml)<br />[流量管理器](../traffic-manager/index.yml)<br />[网络观察程序](../network-watcher/index.yml) |
| Microsoft.NotificationHubs | [通知中心](../notification-hubs/index.yml) |
| Microsoft.OperationalInsights | [Azure Monitor](../azure-monitor/index.yml) |
| Microsoft.OperationsManagement | [Azure Monitor](../azure-monitor/index.yml) |
| Microsoft.PolicyInsights | [Azure Policy](../governance/policy/index.yml) |
| Microsoft.Portal | [Azure 门户](/azure-portal/) |
| Microsoft.PowerBI | [Power BI](https://docs.microsoft.com/zh-cn/power-bi/power-bi-overview) |
| Microsoft.PowerBIDedicated | [Power BI Embedded](/power-bi-embedded/) |
| Microsoft.RecoveryServices | [站点恢复](../site-recovery/index.yml) |
| Microsoft.Relay | [Azure 中继](../service-bus-relay/relay-what-is-it.md) |
| Microsoft.ResourceHealth | core |
| Microsoft.Resources | [Azure Resource Manager](index.yml) |
| Microsoft.Scheduler | [计划程序](/scheduler/) |
| Microsoft.ServiceBus | [服务总线](/service-bus/) |
| Microsoft.ServiceFabric | [Service Fabric](../service-fabric/index.yml) |
| Microsoft.SiteRecovery | [站点恢复](../site-recovery/index.yml) |
| Microsoft.Sql | [Azure SQL 数据库](../sql-database/index.yml) |
| Microsoft.Storage | [存储](../storage/index.yml) |
| Microsoft.StreamAnalytics | [流分析](../stream-analytics/index.yml) |
| Microsoft.TimeSeriesInsights | [时序见解](../time-series-insights/index.yml) |
| Microsoft.Web | [应用服务](../app-service/index.yml)<br />[函数](../azure-functions/index.yml) |


## <a name="next-steps"></a>后续步骤

有关资源提供程序的详细信息，请参阅 [Azure 资源提供程序和类型](resource-manager-supported-services.md)