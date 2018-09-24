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
origin.date: 09/07/2018
ms.date: 09/24/2018
ms.author: v-yeche
ms.openlocfilehash: ccef712d1fb63aa73914250c8d2263f0fc07d87e
ms.sourcegitcommit: 1742417f2a77050adf80a27c2d67aff4c456549e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/21/2018
ms.locfileid: "46527292"
---
<!--pending for verify -->
# <a name="move-operation-support-for-resources"></a>支持移动操作的资源

本文列出某个 Azure 资源类型是否支持移动操作。 尽管某个资源类型支持移动操作，但某些条件可能会阻止该资源的移动。 有关影响移动操作的条件的详细信息，请参阅[将资源移到新的资源组或订阅](resource-group-move-resources.md)。

## <a name="find-resource-provider-and-resource-type"></a>查找资源提供程序和资源类型

若要确定是否可以移动某个资源，必须查找其资源提供程序和资源类型。

对于 PowerShell，请使用：

```PowerShell
Get-AzureRmResource -ResourceGroupName demogroup | Select Name, ResourceType | Format-table
```

对于 Azure CLI，请使用：

```azurecli
az resource list -g demogroup --query '[].{name:name, reourcetype:type}'
```

资源类型以 `<resource-provider>/<resource-type-name>` 格式返回。 因此，值 `Microsoft.Portal/dashboards` 的资源提供程序为 **Microsoft.Portal**，资源类型名称为 **dashboards**。
<!--Change Microsoft.OperationalInsights/workspaces to Microsoft.Portal/dashboards-->

找到资源提供程序和资源类型后，使用本文中的表格来确定该资源类型是否支持移动操作。

## <a name="microsoftaad"></a>Microsoft.AAD

| 资源类型 | 资源组 | 订阅 |
| ------------- | --------------- | ----------- |
| domainservices | 否 | 否 |

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

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
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

<!-- Not Available on ## Microsoft.BatchAI-->

<!-- Not Available on ## Microsoft.BingMaps-->
<!-- Not Available on ## Microsoft.BizTalkServices-->
<!-- Not Available on ## Microsoft.Blueprint-->

<!-- Not Available on ## Microsoft.BotService-->

## <a name="microsoftcache"></a>Microsoft.Cache
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| redis | 是 | 是 |

<!-- Not Available on ## Microsoft.Cdn-->
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
| registries/webhooks | 是 | 是 |

<!-- Not Available on ## Microsoft.ContainerService-->
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
| servers | 否 | 否 |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| servergroups | 否 | 否 |
| servers | 否 | 否 |

## <a name="microsoftdevices"></a>Microsoft.Devices
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| iothubs | 是 | 是 |
| provisioningservices | 是 | 是 |

<!-- Not Available on ## Microsoft.DevTestLab-->
## <a name="microsoftdns"></a>microsoft.dns
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| dnszones | 否 | 否 |
| dnszones/a | 否 | 否 |
| dnszones/aaaa | 否 | 否 |
| dnszones/cname | 否 | 否 |
| dnszones/mx | 否 | 否 |
| dnszones/ptr | 否 | 否 |
| dnszones/srv | 否 | 否 |
| dnszones/txt | 否 | 否 |
| trafficmanagerprofiles | 否 | 否 |

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

<!-- Not Available on ## Microsoft.ImportExport-->
<!-- Not Available on ## microsoft.insights-->

<!-- Not Available on ## Microsoft.IoTCentral-->
## <a name="microsoftkeyvault"></a>Microsoft.KeyVault
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| vaults | 是 | 是 |

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
<!-- Not Available on ## Microsoft.ManagedIdentity-->
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
| connections | 是 | 是 |
| ddosprotectionplans | 否 | 否 |
| dnszones | 是 | 是 |
| expressroutecircuits | 否 | 否 |
| expressrouteports | 否 | 否 |
| loadbalancers | 是 | 是 |
| localnetworkgateways | 是 | 是 |
| networkintentpolicies | 是 | 是 |
| networkinterfaces | 是 | 是 |
| networksecuritygroups | 是 | 是 |
| networkwatchers | 是 | 是 |
| networkwatchers/connectionmonitors | 是 | 是 |
| networkwatchers/lenses | 是 | 是 |
| networkwatchers/pingmeshes | 是 | 是 |
| publicipaddresses | 是 | 是 |
| publicipprefixes | 是 | 是 |
| routefilters | 否 | 否 |
| routetables | 是 | 是 |
| trafficmanagerprofiles | 是 | 是 |
| virtualnetworkgateways | 是 | 是 |
| virtualnetworks | 是 | 是 |

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| namespaces | 是 | 是 |
| namespaces/notificationhubs | 是 | 是 |

<!-- Not Available on ## Microsoft.OperationalInsights-->
<!-- Not Available on ## Microsoft.OperationsManagement-->
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

## <a name="microsoftsaas"></a>Microsoft.SaaS
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| applications | 是 | 否 |

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

## <a name="microsoftsolutions"></a>Microsoft.Solutions
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| appliancedefinitions | 否 | 否 |
| appliances | 否 | 否 |
| applicationdefinitions | 否 | 否 |
| applications | 否 | 否 |

## <a name="microsoftsql"></a>Microsoft.Sql
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| servers | 是 | 是 |
| servers/databases | 是 | 是 |
| servers/elasticpools | 是 | 是 |
| virtualclusters | 是 | 是 |

## <a name="microsoftstorage"></a>Microsoft.Storage
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| storageaccounts | 是 | 是 |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| storagesyncservices | 是 | 是 |

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

## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT
| 资源类型 | 资源组 | 订阅 |
| ------------- | -------------- | ------------ |
| deviceservices | 是 | 是 |

## <a name="third-party-services"></a>第三方服务

第三方服务目前不支持移动操作。 这些资源提供程序包括：

<!--Pending verify-->
* 84codes.CloudAMQP
* AppDynamics.APM
* Aspera.Transfers
* Auth0.Cloud
* Citrix.Cloud
* Citrix.Services
* CloudSimple.PrivateCloudIaaS
* Cloudyn.Analytics
* Conexlink.MyCloudIT
* Crypteron.DataSecurity
* Dynatrace.DynatraceSaaS
* Dynatrace.Ruxit
* LiveArena.Broadcast
* Lombiq.DotNest
* Mailjet.Email
* Myget.PackageManagement
* NewRelic.APM
* nuubit.nextgencdn
* Paraleap.CloudMonix
* Pokitdok.Platform
* RavenHq.Db
* Raygun.CrashReporting
* RevAPM.MobileCDN
* Sendgrid.Email
* Sparkpost.Basic
* stackify.retrace
* SuccessBricks.ClearDB
* TrendMicro.DeepSecurity
* U2uconsult.TheIdentityHub <!--Pending verify-->

## <a name="next-steps"></a>后续步骤

* 有关用于移动资源的命令，请参阅[将资源移到新资源组或订阅](resource-group-move-resources.md)。

<!-- Update_Description: new articles on azure resource manager move support resources -->
<!--ms.date: 09/24/2018-->