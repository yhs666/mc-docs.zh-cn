---
title: 支持移动操作的 Azure 资源类型 | Azure
description: 列出可移到新资源组或订阅的 Azure 资源类型。
services: azure-resource-manager
documentationcenter: ''
author: rockboyfor
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
origin.date: 01/22/2018
ms.date: 02/18/2019
ms.author: v-yeche
ms.openlocfilehash: f5b7ad2f5b85e8acdacb1badef64a2f0915005e6
ms.sourcegitcommit: cdcb4c34aaae9b9d981dec534007121b860f0774
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/15/2019
ms.locfileid: "56306174"
---
<!--pending for verify -->
# <a name="move-operation-support-for-resources"></a>支持移动操作的资源

本文列出某个 Azure 资源类型是否支持移动操作。 尽管某个资源类型支持移动操作，但某些条件可能会阻止该资源的移动。 有关影响移动操作的条件的详细信息，请参阅[将资源移到新的资源组或订阅](resource-group-move-resources.md)。

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="find-resource-provider-and-resource-type"></a>查找资源提供程序和资源类型

若要确定是否可以移动某个资源，必须查找其资源提供程序和资源类型。

对于 PowerShell，请使用：

```azurepowershell
Get-AzResource -ResourceGroupName demogroup | Select Name, ResourceType | Format-table
```

对于 Azure CLI，请使用：

```azurecli
az resource list -g demogroup --query '[].{name:name, resourceType:type}' --output table
```

资源类型以 `<resource-provider>/<resource-type-name>` 格式返回。 因此，值 `Microsoft.Portal/dashboards` 的资源提供程序为 **Microsoft.Portal**，资源类型名称为 **dashboards**。

<!--Change Microsoft.OperationalInsights/workspaces to Microsoft.Portal/dashboards-->

找到资源提供程序和资源类型后，使用本文中的表格来确定该资源类型是否支持移动操作。

<!--Not Available on ## Microsoft.AAD-->

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| servers | 是 | 是 |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement
| 资源类型 | 资源组 | 订阅 |
| ------------- | --------------- | ----------- |
| 服务 | 是 | 是 |

## <a name="microsoftauthorization"></a>Microsoft.Authorization
| 资源类型 | 资源组 | 订阅 |
| ------------- | --------------- | ----------- |
| policyassignments | 否 | 否 |

## <a name="microsoftautomation"></a>Microsoft.Automation
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| automationaccounts | 是 | 是 |
| automationaccounts/configurations | 是 | 是 |
| automationaccounts/runbooks | 是 | 是 |

<!--Not Available on ## Microsoft.AzureActiveDirectory-->

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

<!-- Not Available on ## Microsoft.BingMaps-->
<!-- Not Available on ## Microsoft.BizTalkServices-->
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

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| networksecuritygroups | 否 | 否 |
| reservedips | 否 | 否 |
| virtualnetworks | 否 | 否 |

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| storageaccounts | 是 | 否 |

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
| images | 是 | 是 |
| restorepointcollections | 否 | 否 |
| sharedvmimages | 否 | 否 |
| sharedvmimages/versions | 否 | 否 |
| snapshots | 是 | 是 |
| virtualmachines | 是 | 是 |
| virtualmachines/extensions | 是 | 是 |
| virtualmachinescalesets | 是 | 是 |

<!-- Not Available on ## Microsoft.Container-->
<!-- Not Available on ## Microsoft.ContainerInstance-->

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| registries | 是 | 是 |
| registries/buildtasks | 是 | 是 |
| registries/replications | 否 | 否 |
| registries/tasks | 是 | 是 |
| registries/webhooks | 是 | 是 |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| containerservices | 否 | 否 |
| managedclusters | 否 | 否 |
| openshiftmanagedclusters | 否 | 否 |

<!-- Not Available on ## Microsoft.ContentModerator-->
<!-- Not Available on ## Microsoft.CostManagement-->
<!-- Not Available on ## Microsoft.CustomerInsights-->
<!-- Not Available on ## Microsoft.DataBox-->
<!-- Not Available on ## Microsoft.DataBoxEdge-->
<!-- Not Available on ## Microsoft.Databricks-->
<!-- Not Available on ## Microsoft.DataCatalog-->
<!-- Not Available on ## Microsoft.DataFactory-->
<!-- Not Available on ## Microsoft.DataLake-->
<!-- Not Available on ## Microsoft.DataLakeAnalytics-->
<!-- Not Available on ## Microsoft.DataLakeStore-->
<!-- Not Available on ## Microsoft.DataMigration-->
<!-- Not Available on ## Microsoft.DBforMariaDB-->

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| servers | 是 | 是 |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| servergroups | 否 | 否 |
| servers | 是 | 是 |

<!--Not Available on ## Microsoft.DeploymentManager-->

## <a name="microsoftdevices"></a>Microsoft.Devices
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| iothubs | 是 | 是 |
| provisioningservices | 是 | 是 |

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

<!-- Not Available on ## Microsoft.HanaOnAzure-->
<!-- Not Available on ## Microsoft.HardwareSecurityModules-->

## <a name="microsofthdinsight"></a>Microsoft.HDInsight
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| clusters | 是 | 是 |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| jobs | 是 | 是 |

## <a name="microsoftinsights"></a>microsoft.insights
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| actiongroups | 是 | 是 |
| activitylogalerts | 否 | 否 |
| alertrules | 是 | 是 |
| autoscalesettings | 是 | 是 |
| components | 是 | 是 |
| metricalerts | 否 | 否 |
| scheduledqueryrules | 是 | 是 |
| webtests | 是 | 是 |
| workbooks | 是 | 是 |

<!-- Not Available on ## Microsoft.IoTCentral-->

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| vaults | 是 | 是 |

<!-- Not Available on## Microsoft.Kusto-->
<!-- Not Available on ## Microsoft.LabServices-->
<!-- Not Available on ## Microsoft.LocationBasedServices-->
<!-- Not Available on ## Microsoft.LocationServices-->

## <a name="microsoftlogic"></a>Microsoft.Logic
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| integrationaccounts | 是 | 是 |
| workflows | 是 | 是 |

<!-- Not Available on ## Microsoft.MachineLearning-->
<!-- Not Available on ## Microsoft.MachineLearningCompute-->
<!-- Not Available on ## Microsoft.MachineLearningExperimentation-->
<!-- Not Available on ## Microsoft.MachineLearningModelManagement-->

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| userassignedidentities | 是 | 是 |

<!-- Not Available on ## Microsoft.Maps-->
<!-- Not Available on ## Microsoft.MarketplaceApps-->

## <a name="microsoftmedia"></a>Microsoft.Media
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| mediaservices | 是 | 是 |
| mediaservices/liveevents | 是 | 是 |
| mediaservices/streamingendpoints | 是 | 是 |

<!-- Not Available on ## Microsoft.Migrate-->

## <a name="microsoftnetwork"></a>Microsoft.Network
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| applicationgateways | 否 | 否 |
| applicationsecuritygroups | 是 | 是 |
| azurefirewalls | 否 | 否 |
| connections | 是 | 是 |
| ddosprotectionplans | 否 | 否 |
| dnszones | 是 | 是 |
| expressroutecircuits | 否 | 否 |
| expressroutecrossconnections | 否 | 否 |
| expressroutegateways | 否 | 否 |
| expressrouteports | 否 | 否 |
| frontdoors | 是 | 是 |
| frontdoorwebapplicationfirewallpolicies | 是 | 是 |
| interfaceendpoints | 否 | 否 |
| loadbalancers | 是 | 是 |
| localnetworkgateways | 是 | 是 |
| networkintentpolicies | 是 | 是 |
| networkinterfaces | 是 | 是 |
| networkprofiles | 否 | 否 |
| networksecuritygroups | 是 | 是 |
| networkwatchers | 是 | 是 |
| networkwatchers/connectionmonitors | 是 | 是 |
| networkwatchers/lenses | 是 | 是 |
| networkwatchers/pingmeshes | 是 | 是 |
| publicipaddresses | 是 | 是 |
| publicipprefixes | 是 | 是 |
| routefilters | 否 | 否 |
| routetables | 是 | 是 |
| serviceendpointpolicies | 是 | 是 |
| trafficmanagerprofiles | 是 | 是 |
| virtualhubs | 是 | 是 |
| virtualnetworkgateways | 是 | 是 |
| virtualnetworks | 是 | 是 |
| virtualnetworktaps | 否 | 否 |
| virtualwans | 是 | 是 |
| vpngateways | 是 | 是 |
| vpnsites | 是 | 是 |
| webapplicationfirewallpolicies | 是 | 是 |

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| namespaces | 是 | 是 |
| namespaces/notificationhubs | 是 | 是 |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| workspaces | 是 | 是 |

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| managementconfigurations | 是 | 是 |
| solutions | 是 | 是 |
| 视图 | 是 | 是 |

## <a name="microsoftportal"></a>Microsoft.Portal
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| dashboards | 是 | 是 |

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| workspacecollections | 是 | 是 |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| capacities | 是 | 是 |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| vaults | 是 | 是 |

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

<!-- Not Available on ## Microsoft.Search-->

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| namespaces | 是 | 是 |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| clusters | 是 | 是 |

<!-- Not Available on ## Microsoft.ServiceFabricMesh-->
<!-- Not Available on ## Microsoft.SignalRService-->

## <a name="microsoftsiterecovery"></a>Microsoft.SiteRecovery
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| siterecoveryvault | 否 | 否 |

<!--Not Available on ## Microsoft.Solutions-->

## <a name="microsoftsql"></a>Microsoft.Sql
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| managedinstances | 是 | 是 |
| managedinstances/databases | 是 | 是 |
| servers | 是 | 是 |
| servers/databases | 是 | 是 |
| servers/elasticpools | 是 | 是 |
| virtualclusters | 是 | 是 |

## <a name="microsoftstorage"></a>Microsoft.Storage
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| storageaccounts | 是 | 是 |

<!-- Not Available on ## Microsoft.StorageSync-->
<!-- Not Available on ## Microsoft.StorSimple-->
## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| streamingjobs | 是 | 是 |

<!-- Not Available on # Microsoft.TimeSeriesInsights-->
<!-- Not Available on ## microsoft.visualstudio-->
## <a name="microsoftweb"></a>Microsoft.Web
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| certificates | 否 | 是 |
| classicmobileservices | 否 | 否 |
| connectiongateways | 是 | 是 |
| connections | 是 | 是 |
| customapis | 是 | 是 |
| hostingenvironments | 否 | 否 |
| serverfarms | 是 | 是 |
| sites | 是 | 是 |
| sites/premieraddons | 是 | 是 |
| sites/slots | 是 | 是 |

<!-- Not Available on ## Microsoft.WindowsIoT-->
<!-- Not Available on ## Third-party services-->

## <a name="next-steps"></a>后续步骤

* 有关用于移动资源的命令，请参阅[将资源移到新资源组或订阅](resource-group-move-resources.md)。

<!-- Update_Description: Update meta properties, wording update -->
