---
title: Azure 资源类型支持的移动操作
description: 列出可移到新资源组或订阅的 Azure 资源类型。
author: rockboyfor
ms.service: azure-resource-manager
ms.topic: reference
origin.date: 07/09/2019
ms.date: 08/26/2019
ms.author: v-yeche
ms.openlocfilehash: accf73e1b6e6e5d9b7906f2e739756ff2908a90c
ms.sourcegitcommit: 599d651afb83026938d1cfe828e9679a9a0fb69f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/23/2019
ms.locfileid: "69993617"
---
# <a name="move-operation-support-for-resources"></a>支持移动操作的资源
本文列出某个 Azure 资源类型是否支持移动操作。 它还提供了有关移动资源时要考虑的特殊条件的信息。

跳转到资源提供程序命名空间：
> [!div class="op_single_selector"]
> - [Microsoft.AlertsManagement](#microsoftalertsmanagement)
> - [Microsoft.AnalysisServices](#microsoftanalysisservices)
> - [Microsoft.ApiManagement](#microsoftapimanagement)
> - [Microsoft.AppService](#microsoftappservice)
> - [Microsoft.Authorization](#microsoftauthorization)
> - [Microsoft.Automation](#microsoftautomation)
> - [Microsoft.AzureActiveDirectory](#microsoftazureactivedirectory)
> - [Microsoft.AzureStack](#microsoftazurestack)
> - [Microsoft.Backup](#microsoftbackup)
> - [Microsoft.Batch](#microsoftbatch)
> - [Microsoft.Cache](#microsoftcache)
> - [Microsoft.Cdn](#microsoftcdn)
> - [Microsoft.ClassicCompute](#microsoftclassiccompute)
> - [Microsoft.ClassicNetwork](#microsoftclassicnetwork)
> - [Microsoft.ClassicStorage](#microsoftclassicstorage)
> - [Microsoft.CognitiveServices](#microsoftcognitiveservices)
> - [Microsoft.Compute](#microsoftcompute)
> - [Microsoft.ContainerRegistry](#microsoftcontainerregistry)
> - [Microsoft.ContainerService](#microsoftcontainerservice)
> - [Microsoft.DataFactory](#microsoftdatafactory)
> - [Microsoft.DataMigration](#microsoftdatamigration)
> - [Microsoft.DBforMariaDB](#microsoftdbformariadb)
> - [Microsoft.DBforMySQL](#microsoftdbformysql)
> - [Microsoft.DBforPostgreSQL](#microsoftdbforpostgresql)
> - [Microsoft.Devices](#microsoftdevices)
> - [Microsoft.DocumentDB](#microsoftdocumentdb)
> - [Microsoft.EventHub](#microsofteventhub)
> - [Microsoft.HDInsight](#microsofthdinsight)
> - [Microsoft.ImportExport](#microsoftimportexport)
> - [microsoft.insights](#microsoftinsights)
> - [Microsoft.KeyVault](#microsoftkeyvault)
> - [Microsoft.Kusto](#microsoftkusto)
> - [Microsoft.Logic](#microsoftlogic)
> - [Microsoft.ManagedIdentity](#microsoftmanagedidentity)
> - [Microsoft.Media](#microsoftmedia)
> - [Microsoft.Network](#microsoftnetwork)
> - [Microsoft.NotificationHubs](#microsoftnotificationhubs)
> - [Microsoft.OperationalInsights](#microsoftoperationalinsights)
> - [Microsoft.OperationsManagement](#microsoftoperationsmanagement)
> - [Microsoft.Portal](#microsoftportal)
> - [Microsoft.PortalSdk](#microsoftportalsdk)
> - [Microsoft.PowerBI](#microsoftpowerbi)
> - [Microsoft.PowerBIDedicated](#microsoftpowerbidedicated)
> - [Microsoft.RecoveryServices](#microsoftrecoveryservices)
> - [Microsoft.Relay](#microsoftrelay)
> - [Microsoft.Scheduler](#microsoftscheduler)
> - [Microsoft.Security](#microsoftsecurity)
> - [Microsoft.ServiceBus](#microsoftservicebus)
> - [Microsoft.ServiceFabric](#microsoftservicefabric)
> - [Microsoft.SiteRecovery](#microsoftsiterecovery)
> - [Microsoft.Sql](#microsoftsql)
> - [Microsoft.Storage](#microsoftstorage)
> - [Microsoft.StreamAnalytics](#microsoftstreamanalytics)
> - [Microsoft.TimeSeriesInsights](#microsofttimeseriesinsights)
> - [Microsoft.Web](#microsoftweb)

<!--Not Available on ## Microsoft.AAD-->
<!--Not Available on ## microsoft.aadiam-->

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement
| 资源类型 | 资源组 | 订阅 |
| ------------- | ----------- | ---------- |
| actionrules | 是 | 是 |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| servers | 是 | 是 |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement
| 资源类型 | 资源组 | 订阅 |
| ------------- | ---------------| -----------  |
| 服务 | 是 | 是 |

<!--Not Available on ## Microsoft.AppConfiguration-->

## <a name="microsoftappservice"></a>Microsoft.AppService
| 资源类型 | 资源组 | 订阅 |
| ------------- | ----------- | ---------- |
| apiapps | 否 | 否 |
| appidentities | 否 | 否 |
| gateways | 否 | 否 |

> [!IMPORTANT]
> 请参阅[应用服务移动指南](./move-limitations/app-service-move-limitations.md)。

## <a name="microsoftauthorization"></a>Microsoft.Authorization
| 资源类型 | 资源组 | 订阅 |
| ------------- | ---------------| -----------  |
| policyassignments | 否 | 否 |

## <a name="microsoftautomation"></a>Microsoft.Automation
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| automationaccounts | 是 | 是 |
| automationaccounts/configurations | 是 | 是 |
| automationaccounts/runbooks | 是 | 是 |

> [!IMPORTANT]
> Runbook 必须与自动化帐户存在于同一资源组中。

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory
| 资源类型 | 资源组 | 订阅 |
| ------------- | ----------- | ---------- |
| b2cdirectories | 是 | 是 |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| registrations | 是 | 是 |

## <a name="microsoftbackup"></a>Microsoft.Backup
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| backupvault | 否 | 否 |

## <a name="microsoftbatch"></a>Microsoft.Batch
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| batchaccounts | 是 | 是 |

<!-- Not Available on ## ## Microsoft.BatchAI-->
<!-- Not Available on ## Microsoft.BingMaps-->
<!-- Not Available on ## Microsoft.BizTalkServices-->
<!-- Not Available on ## Microsoft.Blockchain-->
<!-- Not Available on ## Microsoft.Blueprint-->
<!-- Not Available on ## Microsoft.BotService-->

## <a name="microsoftcache"></a>Microsoft.Cache
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| redis | 是 | 是 |

## <a name="microsoftcdn"></a>Microsoft.Cdn
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| 配置文件 | 是 | 是 |
| profiles/endpoints | 是 | 是 |

<!-- Not Available on ## Microsoft.CertificateRegistration-->

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| domainnames | 是 | 否 |
| virtualmachines | 是 | 否 |

> [!IMPORTANT]
> 请参阅[经典部署移动指南](./move-limitations/classic-model-move-limitations.md)。 可以使用特定于该方案的操作跨订阅移动经典部署资源。

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| networksecuritygroups | 否 | 否 |
| reservedips | 否 | 否 |
| virtualnetworks | 否 | 否 |

> [!IMPORTANT]
> 请参阅[经典部署移动指南](./move-limitations/classic-model-move-limitations.md)。 可以使用特定于该方案的操作跨订阅移动经典部署资源。

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| storageaccounts | 是 | 否 |

> [!IMPORTANT]
> 请参阅[经典部署移动指南](./move-limitations/classic-model-move-limitations.md)。 可以使用特定于该方案的操作跨订阅移动经典部署资源。

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| accounts | 是 | 是 |

## <a name="microsoftcompute"></a>Microsoft.Compute
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| availabilitysets | 是 | 是 |
| disks | 是 | 是 |
| galleries | 否 | 否 |
| galleries/images | 否 | 否 |
| galleries/images/versions | 否 | 否 |
| hostgroups | 否 | 否 |
| hostgroups/hosts | 否 | 否 |
| images | 是 | 是 |
| proximityplacementgroups | 否 | 否 |
| restorepointcollections | 否 | 否 |
| sharedvmimages | 否 | 否 |
| sharedvmimages/versions | 否 | 否 |
| snapshots | 是 | 是 |
| virtualmachines | 是 | 是 |
| virtualmachines/extensions | 是 | 是 |
| virtualmachinescalesets | 是 | 是 |

> [!IMPORTANT]
> 请参阅[虚拟机移动指南](./move-limitations/virtual-machines-move-limitations.md)。
<!-- Not Available on ## Microsoft.Container-->
<!-- Not Available on ## Microsoft.ContainerInstance-->

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| registries | 是 | 是 |
| registries/buildtasks | 是 | 是 |
| registries/replications | 是 | 是 |
| registries/tasks | 是 | 是 |
| registries/webhooks | 是 | 是 |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| containerservices | 否 | 否 |
| managedclusters | 否 | 否 |
| openshiftmanagedclusters | 否 | 否 |

<!-- Not Available on ## Microsoft.ContentModerator-->
<!-- Not Available on ## Microsoft.CortanaAnalytics-->
<!-- Not Available on ## Microsoft.CostManagement-->
<!-- Not Available on ## Microsoft.CustomerInsights-->
<!-- Not Available on ## Microsoft.DataBox-->
<!-- Not Available on ## Microsoft.DataBoxEdge-->
<!-- Not Available on ## Microsoft.Databricks-->
<!-- Not Available on ## Microsoft.DataCatalog-->
<!-- Not Available on## Microsoft.DataConnect-->
<!-- Not Available on## Microsoft.DataExchange-->

<a name="microsoftdatafactory"></a>
## <a name="microsoftdatafactory"></a>Microsoft.DataFactory
| 资源类型 | 资源组 | 订阅 |
| ------------- | ----------- | ---------- |
| datafactories | 是 | 是 |
| factories | 是 | 是 |
<!-- Not Available on ## Microsoft.DataLake-->
<!-- Not Available on ## Microsoft.DataLakeAnalytics-->
<!-- Not Available on ## Microsoft.DataLakeStore-->

<a name="microsoftdatamigration"></a>
## <a name="microsoftdatamigration"></a>Microsoft.DataMigration
| 资源类型 | 资源组 | 订阅 |
| ------------- | ----------- | ---------- |
| services | 否 | 否 |
| services/projects | 否 | 否 |
| slots | 否 | 否 |

<a name="microsoftdbformariadb"></a>
## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB
| 资源类型 | 资源组 | 订阅 |
| ------------- | ----------- | ---------- |
| servers | 是 | 是 |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL
| 资源类型 | 资源组 | 订阅 |
| ------------- | ----------- | ---------- |
| servers | 是 | 是 |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL
| 资源类型 | 资源组 | 订阅 |
| ------------- | ----------- | ---------- |
| servergroups | 否 | 否 |
| servers | 是 | 是 |
| serversv2 | 是 | 是 |

<!--Not Available on ## Microsoft.DeploymentManager-->

## <a name="microsoftdevices"></a>Microsoft.Devices
| 资源类型 | 资源组 | 订阅 |
| ------------- | ----------- | ---------- |
| elasticpools | 否 | 否 |
| elasticpools/iothubtenants | 否 | 否 |
| iothubs | 是 | 是 |
| provisioningservices | 是 | 是 |

<!-- Not Available on ## Microsoft.DevSpaces-->
<!-- Not Available on ## Microsoft.DevTestLab-->
<!-- Not Available on ## microsoft.dns-->

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| databaseaccounts | 是 | 是 |

<!-- Not Available on ## Microsoft.DomainRegistration-->
<!-- Not Available on ## Microsoft.EventGrid-->

## <a name="microsofteventhub"></a>Microsoft.EventHub
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| clusters | 是 | 是 |
| namespaces | 是 | 是 |

<!-- Not Available on ## Microsoft.Genomics-->
<!-- Not Available on ## Microsoft.HanaOnAzure-->

## <a name="microsofthdinsight"></a>Microsoft.HDInsight
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| clusters | 是 | 是 |

> [!IMPORTANT]
> 可以将 HDInsight 群集移到新订阅或资源组。 但是，无法在订阅之间移动链接到 HDInsight 群集的网络资源（例如虚拟网络、NIC 或负载均衡器）。 此外，无法将连接到群集的虚拟机的 NIC 移到新的资源组。
>
> 将 HDInsight 群集移至新订阅时，请先移动其他资源（例如存储帐户）。 然后移动 HDInsight 群集本身。

<!-- Not Available on ## Microsoft.HealthcareApis-->
<!-- Not Available on ## Microsoft.HybridCompute-->
<!-- Not Available on ## Microsoft.HybridData-->
## <a name="microsoftimportexport"></a>Microsoft.ImportExport
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| jobs | 是 | 是 |

## <a name="microsoftinsights"></a>microsoft.insights
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| accounts | 否 | 否 |
| actiongroups | 是 | 是 |
| activitylogalerts | 否 | 否 |
| alertrules | 是 | 是 |
| autoscalesettings | 是 | 是 |
| components | 是 | 是 |
| guestdiagnosticsettings | 否 | 否 |
| metricalerts | 否 | 否 |
| notificationgroups | 否 | 否 |
| notificationrules | 否 | 否 |
| scheduledqueryrules | 是 | 是 |
| webtests | 是 | 是 |
| workbooks | 是 | 是 |

> [!IMPORTANT]
> 确保移到新订阅时，不会超出[订阅配额](../azure-subscription-service-limits.md#azure-monitor-limits)。

<!-- Not Available on ## Microsoft.IoTCentral-->
<!-- Not Available on ## Microsoft.IoTSpaces-->

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| vaults | 是 | 是 |

<!--MOONCAKE: HSM IS NOT SUPPORT ON MOONCAKE-->

> [!IMPORTANT]
> 用于磁盘加密的 Key Vault 不能移到同一订阅中的资源组，也不能跨订阅移动。

## <a name="microsoftkusto"></a>Microsoft.Kusto
| 资源类型 | 资源组 | 订阅 |
| ------------- | ----------- | ---------- |
| clusters | 是 | 是 |

<!-- Not Available on ## Microsoft.LabServices-->
<!-- Not Available on ## Microsoft.LocationBasedServices-->
<!-- Not Available on ## Microsoft.LocationServices-->

## <a name="microsoftlogic"></a>Microsoft.Logic
| 资源类型 | 资源组 | 订阅 |
| ------------- | ----------- | ---------- |
| hostingenvironments | 否 | 否 |
| integrationaccounts | 是 | 是 |
| integrationserviceenvironments | 否 | 否 |
| isolatedenvironments | 否 | 否 |
| workflows | 是 | 是 |

<!-- Not Available on ## Microsoft.MachineLearning-->
<!-- Not Available on ## Microsoft.MachineLearningCompute-->
<!-- Not Available on ## Microsoft.MachineLearningExperimentation-->
<!-- Not Available on ## Microsoft.MachineLearningModelManagement-->
<!-- Not Available on ## Microsoft.MachineLearningOperationalization-->
<!-- Not Available on ## Microsoft.MachineLearningServices-->

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| userassignedidentities | 否 | 否 |

<!-- Not Available on ## Microsoft.Maps-->
<!-- Not Available on ## Microsoft.MarketplaceApps-->

## <a name="microsoftmedia"></a>Microsoft.Media
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| mediaservices | 是 | 是 |
| mediaservices/liveevents | 是 | 是 |
| mediaservices/streamingendpoints | 是 | 是 |

<!-- Not Available on ## Microsoft.Migrate-->
<!-- Not Available on ## Microsoft.NetApp-->

## <a name="microsoftnetwork"></a>Microsoft.Network
| 资源类型 | 资源组 | 订阅 |
| ------------- | ----------- | ---------- |
| applicationgateways | 否 | 否 |
| applicationgatewaywebapplicationfirewallpolicies | 否 | 否 |
| applicationsecuritygroups | 是 | 是 |
| azurefirewalls | 是 | 是 |
| bastionhosts | 否 | 否 |
| connections | 是 | 是 |
| ddoscustompolicies | 是 | 是 |
| ddosprotectionplans | 否 | 否 |
| dnszones | 是 | 是 |
| expressroutecircuits | 否 | 否 |
| expressroutecrossconnections | 否 | 否 |
| expressroutegateways | 否 | 否 |
| expressrouteports | 否 | 否 |
| frontdoors | 否 | 否 |
| frontdoorwebapplicationfirewallpolicies | 否 | 否 |
| loadbalancers | 是 - 基本 SKU<br />否 - 标准 SKU | 是 - 基本 SKU<br />否 - 标准 SKU |
| localnetworkgateways | 是 | 是 |
| natgateways | 是 | 是 |
| networkintentpolicies | 是 | 是 |
| networkinterfaces | 是 | 是 |
| networkprofiles | 否 | 否 |
| networksecuritygroups | 是 | 是 |
| networkwatchers | 是 | 是 |
| networkwatchers/connectionmonitors | 是 | 是 |
| networkwatchers/lenses | 是 | 是 |
| networkwatchers/pingmeshes | 是 | 是 |
| p2svpngateways | 否 | 否 |
| privatednszones | 是 | 是 |
| privatednszones/virtualnetworklinks | 是 | 是 |
| privateendpoints | 否 | 否 |
| privatelinkservices | 否 | 否 |
| publicipaddresses | 是 - 基本 SKU<br />否 - 标准 SKU | 是 - 基本 SKU<br />否 - 标准 SKU |
| publicipprefixes | 是 | 是 |
| routefilters | 否 | 否 |
| routetables | 是 | 是 |
| securegateways | 是 | 是 |
| serviceendpointpolicies | 是 | 是 |
| trafficmanagerprofiles | 是 | 是 |
| virtualhubs | 否 | 否 |
| virtualnetworkgateways | 是 | 是 |
| virtualnetworks | 是 | 是 |
| virtualnetworktaps | 否 | 否 |
| virtualwans | 否 | 否 |
| vpngateways | 否 | 否 |
| vpnsites | 否 | 否 |
| webapplicationfirewallpolicies | 是 | 是 |

> [!IMPORTANT]
> 请参阅[虚拟网络移动指南](./move-limitations/virtual-network-move-limitations.md)。

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| namespaces | 是 | 是 |
| namespaces/notificationhubs | 是 | 是 |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| workspaces | 是 | 是 |

> [!IMPORTANT]
> 确保移到新订阅时，不会超出[订阅配额](../azure-subscription-service-limits.md#azure-monitor-limits)。

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| managementconfigurations | 是 | 是 |
| solutions | 是 | 是 |
| 视图 | 是 | 是 |

<!--Not Available on ## Microsoft.Peering-->
## <a name="microsoftportal"></a>Microsoft.Portal
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| dashboards | 是 | 是 |

<a name="microsoftportalsdk"><a/>
<!--Not Available on ## Microsoft.PortalSdk-->

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| workspacecollections | 是 | 是 |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| capacities | 是 | 是 |

<!--Not Available on ## Microsoft.ProjectOxford-->
## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| vaults | 是 | 是 |

<!--MOONCAKE: Not Availabl on backup/backup-azure-move-recovery-services-vault-->
## <a name="microsoftrelay"></a>Microsoft.Relay
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| namespaces | 是 | 是 |

<!--Not Available on ## Microsoft.SaaS-->

## <a name="microsoftscheduler"></a>Microsoft.Scheduler
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| flows | 是 | 是 |
| jobcollections | 是 | 是 |

<!--Not Available on  ## Microsoft.Search-->


## <a name="microsoftsecurity"></a>Microsoft.Security
| 资源类型 | 资源组 | 订阅 |
| ------------- | ----------- | ---------- |
| iotsecuritysolutions | 是 | 是 |

<!-- Not Available on ## Microsoft.ServerManagement-->

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| namespaces | 是 | 是 |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric
| 资源类型 | 资源组 | 订阅 |
| ------------- | ----------- | ---------- |
| applications | 否 | 否 |
| clusters | 是 | 是 |
| containergroups | 否 | 否 |
| containergroupsets | 否 | 否 |
| edgeclusters | 否 | 否 |
| networks | 否 | 否 |
| secretstores | 否 | 否 |
| volumes | 否 | 否 |

<!-- Not Available on ## Microsoft.ServiceFabricMesh-->
<!-- Not Available on ## Microsoft.SignalRService-->

## <a name="microsoftsiterecovery"></a>Microsoft.SiteRecovery
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| siterecoveryvault | 否 | 否 |

<!--MOONCAKE: Not Available on backup/backup-azure-move-recovery-services-vault-->

<!--Not Available on ## Microsoft.Solutions-->

## <a name="microsoftsql"></a>Microsoft.Sql
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| instancepools | 否 | 否 |
| managedinstances | 否 | 否 |
| managedinstances/databases | 否 | 否 |
| servers | 是 | 是 |
| servers/databases | 是 | 是 |
| servers/elasticpools | 是 | 是 |
| virtualclusters | 是 | 是 |

> [!IMPORTANT]
> 数据库和服务器必须位于同一个资源组中。 移动 SQL 服务器时，也会移动其所有数据库。 此行为适用于 Azure SQL 数据库和 Azure SQL 数据仓库数据库。

<!-- Not Available on ## Microsoft.SqlVirtualMachine-->
<!-- Not Available on ## Microsoft.SqlVM-->

## <a name="microsoftstorage"></a>Microsoft.Storage
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| storageaccounts | 是 | 是 |

<!-- Not Available on ## Microsoft.StorageSync-->
<!-- Not Available on ## Microsoft.StorageSyncDev-->
<!-- Not Available on ## Microsoft.StorageSyncInt-->
<!-- Not Available on ## Microsoft.StorSimple-->
## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| streamingjobs | 是 | 是 |

> [!IMPORTANT]
> 当流分析作业处于运行状态时，则无法进行移动。

<!-- Not Available on ## Microsoft.StreamAnalyticsExplorer-->
<!-- Not Available on ## Microsoft.TerraformOSS-->
<a name="microsofttimeseriesinsights"></a>
## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights
| 资源类型 | 资源组 | 订阅 |
| ------------- | ----------- | ---------- |
| environments | 是 | 是 |
| environments/eventsources | 是 | 是 |
| environments/referencedatasets | 是 | 是 |
<!-- Not Available on ## Microsoft.Token-->
<!-- Not Available on ## Microsoft.VirtualMachineImages-->
<!-- Not Available on ## microsoft.visualstudio-->
<!-- Not Available on ## Microsoft.VMwareCloudSimple-->
## <a name="microsoftweb"></a>Microsoft.Web
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| certificates | 否 | 是 |
| connectiongateways | 是 | 是 |
| connections | 是 | 是 |
| customapis | 是 | 是 |
| hostingenvironments | 否 | 否 |
| serverfarms | 是 | 是 |
| sites | 是 | 是 |
| sites/premieraddons | 是 | 是 |
| sites/slots | 是 | 是 |

> [!IMPORTANT]
> 请参阅[应用服务移动指南](./move-limitations/app-service-move-limitations.md)。

<!-- Not Available on ## Microsoft.WindowsIoT-->
## <a name="third-party-services"></a>第三方服务

第三方服务目前不支持移动操作。

## <a name="next-steps"></a>后续步骤
有关用于移动资源的命令，请参阅[将资源移到新资源组或订阅](resource-group-move-resources.md)。

<!--Not Avaialble on [move-support-resources.csv](https://github.com/tfitzmac/resource-capabilities/blob/master/move-support-resources.csv)-->
<!-- Update_Description: Update meta properties, wording update -->
