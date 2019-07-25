---
title: 针对资源的 Azure 资源管理器标记支持
description: 显示支持标记的 Azure资源类型。 提供所有 Azure 服务的详细信息。
author: rockboyfor
ms.service: azure-resource-manager
ms.topic: reference
origin.date: 06/07/2019
ms.date: 07/22/2019
ms.author: v-yeche
ms.openlocfilehash: 3b0809395bf8eadae358a17fbff9e34bd48640ee
ms.sourcegitcommit: 5fea6210f7456215f75a9b093393390d47c3c78d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/19/2019
ms.locfileid: "68337539"
---
# <a name="tag-support-for-azure-resources"></a>Azure 资源的标记支持
本文介绍某一资源类型是否支持[标记](resource-group-using-tags.md)。 标记为“支持标记”  的列指示资源类型是否具有标记的属性。 标记为“在成本报表中标记”  的列指示该资源类型是否将标记传递给成本报表。

<!--Not Available on [tag-support.csv](https://github.com/tfitzmac/resource-capabilities/blob/master/tag-support.csv)-->

## <a name="microsoftaad"></a>Microsoft.AAD
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| DomainServices | 是 | 是 |
| DomainServices/oucontainer | 否 | 否 |

<!--Not Available on ## microsoft.aadiam-->
<!--Not Available on ## Microsoft.Addons-->
<!--Not Available on ## Microsoft.ADHybridHealthService-->

## <a name="microsoftadvisor"></a>Microsoft.Advisor
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| 配置 | 否 |  否 |
| generateRecommendations | 否 |  否 |
| 建议 | 否 |  否 |
| 禁止显示 | 否 |  否 |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| actionRules | 否 |  否 |
| alerts | 否 |  否 |
| alertsList | 否 |  否 |
| alertsSummary | 否 |  否 |
| alertsSummaryList | 否 |  否 |
| smartDetectorAlertRules | 否 |  否 |
| smartDetectorRuntimeEnvironments | 否 |  否 |
| smartGroups | 否 |  否 |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| servers | 是 | 是 |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| reportFeedback | 否 |  否 |
| 服务 | 是 | 是 |
| validateServiceName | 否 |  否 |

<!--Not Available on ## Microsoft.Attestation-->

## <a name="microsoftauthorization"></a>Microsoft.Authorization
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| classicAdministrators | 否 |  否 |
| denyAssignments | 否 |  否 |
| elevateAccess | 否 |  否 |
| 锁定 | 否 |  否 |
| 权限 | 否 |  否 |
| policyAssignments | 否 |  否 |
| policyDefinitions | 否 |  否 |
| policySetDefinitions | 否 |  否 |
| providerOperations | 否 |  否 |
| roleAssignments | 否 |  否 |
| roleDefinitions | 否 |  否 |

## <a name="microsoftautomation"></a>Microsoft.Automation
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| automationAccounts | 是 | 是 |
| automationAccounts/configurations | 是 | 是 |
| automationAccounts/jobs | 否 |  否 |
| automationAccounts/runbooks | 是 | 是 |
| automationAccounts/softwareUpdateConfigurations | 否 | 否 |
| automationAccounts/webhooks | 否 |  否 |

<!--Not Avaialble on ## Microsoft.Azure.Geneva-->

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| b2cDirectories | 是 | 否 |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| registrations | 是 | 是 |
| registrations/customerSubscriptions | 否 |  否 |
| registrations/products | 否 |  否 |

## <a name="microsoftbatch"></a>Microsoft.Batch
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| batchAccounts | 是 | 是 |

<!--Not Available on ## Microsoft.Billing -->
<!--Not Available on ## Microsoft.BingMaps-->
<!--Not Available on ## Microsoft.BizTalkServices-->
<!--Not Available on ## Microsoft.Blueprint-->
<!--Not Available on ## Microsoft.BotService-->

## <a name="microsoftcache"></a>Microsoft.Cache
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| Redis | 是 | 是 |
| RedisConfigDefinition | 否 |  否 |

<!--Not Available on ## Microsoft.Capacity-->
## <a name="microsoftcdn"></a>Microsoft.Cdn
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| edgenodes | 否 |  否 |
| 配置文件 | 是 | 是 |
| profiles/endpoints | 是 | 是 |
| profiles/endpoints/customdomains | 否 |  否 |
| profiles/endpoints/origins | 否 |  否 |
| validateProbe | 否 |  否 |

<!--Not Available on ## Microsoft.CertificateRegistration-->

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| capabilities | 否 |  否 |
| domainNames | 否 |  否 |
| domainNames/capabilities | 否 |  否 |
| domainNames/internalLoadBalancers | 否 |  否 |
| domainNames/serviceCertificates | 否 |  否 |
| domainNames/slots | 否 |  否 |
| domainNames/slots/roles | 否 |  否 |
| moveSubscriptionResources | 否 |  否 |
| operatingSystemFamilies | 否 |  否 |
| operatingSystems | 否 |  否 |
| quotas | 否 |  否 |
| resourceTypes | 否 |  否 |
| validateSubscriptionMoveAvailability | 否 |  否 |
| virtualMachines | 否 |  否 |
| virtualMachines/diagnosticSettings | 否 |  否 |

## <a name="microsoftclassicinfrastructuremigrate"></a>Microsoft.ClassicInfrastructureMigrate
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| classicInfrastructureResources | 否 |  否 |

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| capabilities | 否 |  否 |
| expressRouteCrossConnections | 否 |  否 |
| expressRouteCrossConnections/peerings | 否 |  否 |
| gatewaySupportedDevices | 否 |  否 |
| networkSecurityGroups | 否 |  否 |
| quotas | 否 |  否 |
| reservedIps | 否 |  否 |
| virtualNetworks | 否 |  否 |
| virtualNetworks/remoteVirtualNetworkPeeringProxies | 否 |  否 |
| virtualNetworks/virtualNetworkPeerings | 否 |  否 |

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| capabilities | 否 |  否 |
| disks | 否 |  否 |
| images | 否 |  否 |
| osImages | 否 |  否 |
| osPlatformImages | 否 |  否 |
| publicImages | 否 |  否 |
| quotas | 否 |  否 |
| storageAccounts | 否 |  否 |
| storageAccounts/services | 否 |  否 |
| storageAccounts/services/diagnosticSettings | 否 |  否 |
| storageAccounts/vmImages | 否 |  否 |
| vmImages | 否 |  否 |

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| accounts | 是 | 是 |

<!--Not Available on ## Microsoft.Commerce-->
## <a name="microsoftcompute"></a>Microsoft.Compute
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| availabilitySets | 是 | 是 |
| disks | 是 | 是 |
| images | 是 | 是 |
| restorePointCollections | 是 | 是 |
| restorePointCollections/restorePoints | 否 |  否 |
| sharedVMImages | 是 | 是 |
| sharedVMImages/versions | 是 | 是 |
| snapshots | 是 | 是 |
| virtualMachines | 是 | 是 |
| virtualMachines/diagnosticSettings | 否 |  否 |
| virtualMachines/extensions | 是 | 是 |
| virtualMachineScaleSets | 是 | 是 |
| virtualMachineScaleSets/extensions | 否 |  否 |
| virtualMachineScaleSets/networkInterfaces | 否 |  否 |
| virtualMachineScaleSets/publicIPAddresses | 否 |  否 |
| virtualMachineScaleSets/virtualMachines | 否 |  否 |
| virtualMachineScaleSets/virtualMachines/networkInterfaces | 否 |  否 |

<!--Not Available on ## Microsoft.Consumption -->
<!--Not Available on ## Microsoft.ContainerInstance-->

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| registries | 是 | 是 |
| registries/builds | 否 |  否 |
| registries/builds/cancel | 否 |  否 |
| registries/builds/getLogLink | 否 |  否 |
| registries/buildTasks | 是 | 是 |
| registries/buildTasks/steps | 否 |  否 |
| registries/eventGridFilters | 否 |  否 |
| registries/getBuildSourceUploadUrl | 否 |  否 |
| registries/GetCredentials | 否 |  否 |
| registries/importImage | 否 |  否 |
| registries/queueBuild | 否 |  否 |
| registries/regenerateCredential | 否 |  否 |
| registries/regenerateCredentials | 否 |  否 |
| registries/replications | 是 | 是 |
| registries/runs | 否 |  否 |
| registries/runs/cancel | 否 |  否 |
| registries/scheduleRun | 否 |  否 |
| registries/tasks | 是 | 是 |
| registries/updatePolicies | 否 |  否 |
| registries/webhooks | 是 | 是 |
| registries/webhooks/getCallbackConfig | 否 |  否 |
| registries/webhooks/ping | 否 |  否 |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| containerServices | 是 | 是 |
| managedClusters | 是 | 是 |

<!--Not Available on ## Microsoft.ContentModerator -->
<!--Not Available on ## Microsoft.CortanaAnalytics-->

<!--Not Available on  ## Microsoft.CostManagement-->
<!--Not Available on  ## Microsoft.CustomerInsights-->
<!--Not Available on  ## Microsoft.DataBox-->
<!--Not Available on  ## Microsoft.Databricks-->
<!--Not Available on  ## Microsoft.DataCatalog-->
<!--Not Available on  ## Microsoft.DataConnect-->

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| dataFactories | 是 | 否 |
| dataFactories/diagnosticSettings | 否 |  否 |
| dataFactorySchema | 否 |  否 |
| factories | 是 | 否 |
| factories/integrationRuntimes | 否 |  否 |

<!--Not Available on  ## Microsoft.DataLakeAnalytics-->
<!--Not Available on  ## Microsoft.DataLakeStore-->
<!--Not Available on  ## Microsoft.DataMigration-->

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| servers | 是 | 是 |
| servers/recoverableServers | 否 |  否 |
| servers/virtualNetworkRules | 否 |  否 |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| servers | 是 | 是 |
| servers/recoverableServers | 否 |  否 |
| servers/virtualNetworkRules | 否 |  否 |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| servers | 是 | 是 |
| servers/advisors | 否 |  否 |
| servers/queryTexts | 否 |  否 |
| servers/recoverableServers | 否 |  否 |
| servers/topQueryStatistics | 否 |  否 |
| servers/virtualNetworkRules | 否 |  否 |
| servers/waitStatistics | 否 |  否 |

## <a name="microsoftdevices"></a>Microsoft.Devices
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| IotHubs | 是 | 是 |
| IotHubs/eventGridFilters | 否 |  否 |
| ProvisioningServices | 是 | 是 |
| usages | 否 |  否 |

<!--Not Available on ## Microsoft.DevSpaces-->
<!--Not Available on ## Microsoft.DevTestLab-->

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| databaseAccountNames | 否 |  否 |
| databaseAccounts | 是 | 是 |

<!--Not Available on ## Microsoft.DomainRegistration-->
<!--Not Available on ## Microsoft.DynamicsLcs-->
<!--Not Available on ## Microsoft.EventGrid-->

## <a name="microsofteventhub"></a>Microsoft.EventHub
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| clusters | 是 | 否 |
| namespaces | 是 | 否 |
| namespaces/authorizationrules | 否 |  否 |
| namespaces/disasterrecoveryconfigs | 否 |  否 |
| namespaces/eventhubs | 否 |  否 |
| namespaces/eventhubs/authorizationrules | 否 |  否 |
| namespaces/eventhubs/consumergroups | 否 |  否 |

## <a name="microsoftfeatures"></a>Microsoft.Features
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| features | 否 |  否 |
| providers | 否 |  否 |

<!--Not Available on ## Microsoft.Gallery-->

## <a name="microsoftguestconfiguration"></a>Microsoft.GuestConfiguration
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| guestConfigurationAssignments | 否 |  否 |
| software | 否 |  否 |

<!--Not Available on  ## Microsoft.HanaOnAzure-->

## <a name="microsofthdinsight"></a>Microsoft.HDInsight
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| clusters | 是 | 是 |
| clusters/applications | 否 |  否 |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| jobs | 是 | 是 |

<!--Not Available on ## Microsoft.InformationProtection-->

## <a name="microsoftinsights"></a>microsoft.insights
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| actiongroups | 是 | 是 |
| activityLogAlerts | 是 | 是 |
| alertrules | 是 | 是 |
| automatedExportSettings | 否 |  否 |
| autoscalesettings | 是 | 是 |
| baseline | 否 |  否 |
| calculatebaseline | 否 |  否 |
| components | 是 | 是 |
| components/events | 否 |  否 |
| components/pricingPlans | 否 |  否 |
| components/query | 否 |  否 |
| diagnosticSettings | 否 |  否 |
| diagnosticSettingsCategories | 否 |  否 |
| eventCategories | 否 |  否 |
| eventtypes | 否 |  否 |
| extendedDiagnosticSettings | 否 |  否 |
| logDefinitions | 否 |  否 |
| logprofiles | 否 |  否 |
| 日志 | 否 |  否 |
| metricAlerts | 是 | 是 |
| migrateToNewPricingModel | 否 |  否 |
| myWorkbooks | 否 |  否 |
| 查询 | 否 |  否 |
| rollbackToLegacyPricingModel | 否 |  否 |
| scheduledqueryrules | 是 | 是 |
| vmInsightsOnboardingStatuses | 否 |  否 |
| webtests | 是 | 是 |
| workbooks | 是 | 是 |

<!--Not Available on  ## Microsoft.Intune-->
<!--Not Available on  ## Microsoft.IoTCentral-->
<!--Not Available on  ## Microsoft.IoTSpaces-->

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| deletedVaults | 否 |  否 |
| vaults | 是 | 是 |
| vaults/accessPolicies | 否 |  否 |
| vaults/secrets | 否 |  否 |

## <a name="microsoftkusto"></a>Microsoft.Kusto
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| clusters | 是 | 是 |
| clusters/databases | 否 |  否 |
| clusters/databases/dataconnections | 否 |  否 |
| clusters/databases/eventhubconnections | 否 |  否 |

<!--Not Available on  ## Microsoft.LabServices-->
<!--Not Available on  ## Microsoft.LocationBasedServices-->
<!--Not Available on  ## Microsoft.LocationServices-->
<!--Not Available on  ## Microsoft.LogAnalytics-->

## <a name="microsoftlogic"></a>Microsoft.Logic
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| integrationAccounts | 是 | 是 |
| workflows | 是 | 是 |

<!--Not Available on ## Microsoft.MachineLearning-->
<!--Not Available on ## Microsoft.MachineLearningExperimentation-->
<!--Not Available on ## Microsoft.MachineLearningModelManagement-->
<!--Not Available on ## Microsoft.MachineLearningServices-->
## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| 标识 | 否 |  否 |
| userAssignedIdentities | 是 | 

## <a name="microsoftmanagement"></a>Microsoft.Management
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| getEntities | 否 |  否 |
| managementGroups | 否 |  否 |
| 资源 | 否 |  否 |
| startTenantBackfill | 否 |  否 |
| tenantBackfillStatus | 否 |  否 |

<!--Not Available on  ## Microsoft.Maps-->
<!--Not Available on  ## Microsoft.Marketplace-->
<!--Not Available on  ## Microsoft.MarketplaceApps-->
<!--Not Available on  ## Microsoft.MarketplaceOrdering-->

## <a name="microsoftmedia"></a>Microsoft.Media
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| mediaservices | 是 | 是 |
| mediaservices/accountFilters | 否 |  否 |
| mediaservices/assets | 否 |  否 |
| mediaservices/assets/assetFilters | 否 |  否 |
| mediaservices/contentKeyPolicies | 否 |  否 |
| mediaservices/eventGridFilters | 否 |  否 |
| mediaservices/liveEventOperations | 否 |  否 |
| mediaservices/liveEvents | 是 | 是 |
| mediaservices/liveEvents/liveOutputs | 否 |  否 |
| mediaservices/liveOutputOperations | 否 |  否 |
| mediaservices/streamingEndpointOperations | 否 |  否 |
| mediaservices/streamingEndpoints | 是 | 是 |
| mediaservices/streamingLocators | 否 |  否 |
| mediaservices/streamingPolicies | 否 |  否 |
| mediaservices/transforms | 否 |  否 |
| mediaservices/transforms/jobs | 否 |  否 |

<!--Not Available on  ## Microsoft.Migrate-->

## <a name="microsoftnetwork"></a>Microsoft.Network
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| applicationGateways | 是 | 否 |
| applicationSecurityGroups | 是 | 是 |
| azureFirewallFqdnTags | 否 |  否 |
| azureFirewalls | 是 | 否 |
| bgpServiceCommunities | 否 |  否 |
| connections | 是 | 是 |
| ddosCustomPolicies | 是 | 是 |
| ddosProtectionPlans | 是 | 是 |
| dnsOperationStatuses | 否 |  否 |
| dnszones | 是 | 是 |
| dnszones/A | 否 |  否 |
| dnszones/AAAA | 否 |  否 |
| dnszones/all | 否 |  否 |
| dnszones/CAA | 否 |  否 |
| dnszones/CNAME | 否 |  否 |
| dnszones/MX | 否 |  否 |
| dnszones/NS | 否 |  否 |
| dnszones/PTR | 否 |  否 |
| dnszones/recordsets | 否 |  否 |
| dnszones/SOA | 否 |  否 |
| dnszones/SRV | 否 |  否 |
| dnszones/TXT | 否 |  否 |
| expressRouteCircuits | 是  | 否 |
| expressRouteServiceProviders | 否 |  否 |
| getDnsResourceReference | 否 |  否 |
| interfaceEndpoints | 是 | 是 |
| internalNotify | 否 |  否 |
| loadBalancers | 是 | 否 |
| localNetworkGateways | 是 | 是 |
| natGateways | 是 | 是 |
| networkIntentPolicies | 是 | 是 |
| networkInterfaces | 是 | 是 |
| networkProfiles | 是 | 是 |
| networkSecurityGroups | 是 | 是 |
| networkWatchers | 是 | 否 |
| networkWatchers/connectionMonitors | 是 | 否 |
| networkWatchers/lenses | 是 | 否 |
| networkWatchers/pingMeshes | 是 | 否 |
| privateLinkServices | 是 | 是 |
| publicIPAddresses | 是 | 是 |
| publicIPPrefixes | 是 | 是 |
| routeFilters | 是 | 是 |
| routeTables | 是 | 是 |
| serviceEndpointPolicies | 是 | 是 |
| trafficManagerGeographicHierarchies | 否 |  否 |
| trafficmanagerprofiles | 是 | 是 |
| trafficmanagerprofiles/heatMaps | 否 |  否 |
| virtualHubs | 是 | 是 |
| virtualNetworkGateways | 是 | 否 |
| virtualNetworks | 是 | 是 |
| virtualNetworks/subnets | 否 |  否 |
| virtualNetworkTaps | 是 | 是 |
| virtualWans | 是 | 是 |
| vpnGateways | 是 | 否 |
| vpnSites | 是 | 是 |
| webApplicationFirewallPolicies | 是 | 是 |

<a name="frontdoor" />

<!--Not Available on Azure Front Door Service-->

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| namespaces | 是 | 否 |
| namespaces/notificationHubs | 是 | 否 |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| devices | 否 |  否 |
| linkTargets | 否 |  否 |
| storageInsightConfigs | 否 |  否 |
| workspaces | 是 | 是 |
| workspaces/dataSources | 否 |  否 |
| workspaces/linkedServices | 否 |  否 |
| workspaces/query | 否 |  否 |

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| managementassociations | 否 |  否 |
| managementconfigurations | 是 | 是 |
| solutions | 是 | 是 |
| 视图 | 是 | 是 |

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| policyEvents | 否 |  否 |
| policyStates | 否 |  否 |
| policyTrackedResources | 否 |  否 |
| remediations | 否 |  否 |

## <a name="microsoftportal"></a>Microsoft.Portal
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| consoles | 否 |  否 |
| dashboards | 是 | 是 |
| userSettings | 否 |  否 |

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| workspaceCollections | 是 | 是 |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| capacities | 是 | 是 |

<!--Not Available on  ## Microsoft.ProjectOxford-->

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| backupProtectedItems | 否 |  否 |
| vaults | 是 | 是 |

## <a name="microsoftrelay"></a>Microsoft.Relay
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| namespaces | 是 | 是 |
| namespaces/authorizationrules | 否 |  否 |
| namespaces/hybridconnections | 否 |  否 |
| namespaces/hybridconnections/authorizationrules | 否 |  否 |
| namespaces/wcfrelays | 否 |  否 |
| namespaces/wcfrelays/authorizationrules | 否 |  否 |

<!--Not Available on  ## Microsoft.ResourceGraph-->

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| availabilityStatuses | 否 |  否 |
| childAvailabilityStatuses | 否 |  否 |
| childResources | 否 |  否 |
| events | 否 |  否 |
| impactedResources | 否 |  否 |
| 通知 | 否 |  否 |

## <a name="microsoftresources"></a>Microsoft.Resources
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| deployments | 否 |  否 |
| deployments/operations | 否 |  否 |
| links | 否 |  否 |
| notifyResourceJobs | 否 |  否 |
| providers | 否 |  否 |
| resourceGroups | 否 |  否 |
| 资源 | 否 |  否 |
| subscriptions | 否 |  否 |
| subscriptions/providers | 否 |  否 |
| subscriptions/resourceGroups | 否 |  否 |
| subscriptions/resourcegroups/resources | 否 |  否 |
| subscriptions/resources | 否 |  否 |
| subscriptions/tagnames | 否 |  否 |
| subscriptions/tagNames/tagValues | 否 |  否 |
| tenants | 否 |  否 |

<!--Not Available on  ## Microsoft.SaaS-->

## <a name="microsoftscheduler"></a>Microsoft.Scheduler
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| flows | 是 | 是 |
| jobcollections | 是 | 是 |

<!--Not Available on ## Search-->
<!--Not Available on ## Microsoft.Security-->
<!--Not Available on  ## Microsoft.SecurityGraph-->
## <a name="microsoftservicebus"></a>Microsoft.ServiceBus
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| namespaces | 是 | 否 |
| namespaces/authorizationrules | 否 |  否 |
| namespaces/disasterrecoveryconfigs | 否 |  否 |
| namespaces/eventgridfilters | 否 |  否 |
| namespaces/queues | 否 |  否 |
| namespaces/queues/authorizationrules | 否 |  否 |
| namespaces/topics | 否 |  否 |
| namespaces/topics/authorizationrules | 否 |  否 |
| namespaces/topics/subscriptions | 否 |  否 |
| namespaces/topics/subscriptions/rules | 否 |  否 |
| premiumMessagingRegions | 否 |  否 |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| clusters | 是 | 是 |
| clusters/applications | 否 |  否 |

<!--Not Available on ## Service Fabric Mesh-->
<!--Not Available on ## SignalR Service-->


## <a name="microsoftsolutions"></a>Microsoft.Solutions
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| applianceDefinitions | 是 | 是 |
| appliances | 是 | 是 |
| applicationDefinitions | 是 | 是 |
| applications | 是 | 是 |
| jitRequests | 是 | 是 |

## <a name="microsoftsql"></a>Microsoft.SQL
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| managedInstances | 是 | 是 |
| managedInstances/databases | 是（见下方备注） | 是 |
| managedInstances/databases/backupShortTermRetentionPolicies | 否 | 否 |
| managedInstances/databases/schemas/tables/columns/sensitivityLabels | 否 | 否 |
| managedInstances/databases/vulnerabilityAssessments | 否 | 否 |
| managedInstances/databases/vulnerabilityAssessments/rules/baselines | 否 | 否 |
| managedInstances/encryptionProtector | 否 | 否 |
| managedInstances/keys | 否 | 否 |
| managedInstances/restorableDroppedDatabases/backupShortTermRetentionPolicies | 否 | 否 |
| managedInstances/vulnerabilityAssessments | 否 | 否 |
| servers | 是 | 是 |
| servers/administrators | 否 |  否 |
| servers/communicationLinks | 否 |  否 |
| servers/databases | 是（见下方备注） | 是 |
| servers/encryptionProtector | 否 |  否 |
| servers/firewallRules | 否 |  否 |
| servers/keys | 否 |  否 |
| servers/restorableDroppedDatabases | 否 |  否 |
| servers/serviceobjectives | 否 |  否 |
| servers/tdeCertificates | 否 |  否 |

> [!NOTE]
> Master 数据库不支持标记，但其他数据库（包括 Azure SQL 数据仓库数据库）支持标记。 Azure SQL 数据仓库数据库必须处于活动（而非暂停）状态。

<!--Not Available on ## Microsoft.SqlVirtualMachine-->

## <a name="microsoftstorage"></a>Microsoft.Storage
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| storageAccounts | 是 | 是 |
| storageAccounts/blobServices | 否 |  否 |
| storageAccounts/fileServices | 否 |  否 |
| storageAccounts/queueServices | 否 |  否 |
| storageAccounts/services | 否 |  否 |
| storageAccounts/tableServices | 否 |  否 |
| usages | 否 |  否 |

<!--Not Available on  ## Microsoft.StorageSync-->
<!--Not Available on ## Microsoft.StorSimple-->

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| streamingjobs | 是（见下方备注） | 是 |
| streamingjobs/diagnosticSettings | 否 |  否 |

> [!NOTE]
> Streamingjobs 运行时无法添加标记。 停止要添加标记的资源。

<!--Not Available on ## Microsoft.Subscription-->
<!--Not Available on ## microsoft.support-->
<!--Not Available on ## Microsoft.TerraformOSS-->

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| environments | 是 | 否 |
| environments/accessPolicies | 否 |  否 |
| environments/eventsources | 是 | 否 |
| environments/referenceDataSets | 是 | 否 |

<!--Not Available on ## microsoft.visualstudio-->

## <a name="microsoftweb"></a>Microsoft.Web
| 资源类型 | 支持标记 | 在成本报表中标记 |
| ------------- | ----------- | ----------- |
| apiManagementAccounts | 否 |  否 |
| apiManagementAccounts/apiAcls | 否 |  否 |
| apiManagementAccounts/apis | 否 |  否 |
| apiManagementAccounts/apis/apiAcls | 否 |  否 |
| apiManagementAccounts/apis/connectionAcls | 否 |  否 |
| apiManagementAccounts/apis/connections | 否 |  否 |
| apiManagementAccounts/apis/connections/connectionAcls | 否 |  否 |
| apiManagementAccounts/apis/localizedDefinitions | 否 |  否 |
| apiManagementAccounts/connectionAcls | 否 |  否 |
| apiManagementAccounts/connections | 否 |  否 |
| billingMeters | 否 |  否 |
| certificates | 是 | 是 |
| connectionGateways | 是 | 是 |
| connections | 是 | 是 |
| customApis | 是 | 是 |
| deletedSites | 否 |  否 |
| functions | 否 |  否 |
| hostingEnvironments | 是 | 是 |
| hostingEnvironments/multiRolePools | 否 |  否 |
| hostingEnvironments/multiRolePools/instances | 否 |  否 |
| hostingEnvironments/workerPools | 否 |  否 |
| hostingEnvironments/workerPools/instances | 否 |  否 |
| publishingUsers | 否 |  否 |
| 建议 | 否 |  否 |
| resourceHealthMetadata | 否 |  否 |
| runtimes | 否 |  否 |
| serverFarms | 是 | 是 |
| serverFarms/workers | 否 |  否 |
| sites | 是 | 是 |
| sites/domainOwnershipIdentifiers | 否 |  否 |
| sites/hostNameBindings | 否 |  否 |
| sites/instances | 否 |  否 |
| sites/instances/extensions | 否 |  否 |
| sites/premieraddons | 是 | 是 |
| sites/recommendations | 否 |  否 |
| sites/resourceHealthMetadata | 否 |  否 |
| sites/slots | 是 | 是 |
| sites/slots/hostNameBindings | 否 |  否 |
| sites/slots/instances | 否 |  否 |
| sites/slots/instances/extensions | 否 |  否 |
| sourceControls | 否 |  否 |
| validate | 否 |  否 |
| verifyHostingEnvironmentVnet | 否 |  否 |

<!--Not Available on  ## Microsoft.WindowsDefenderATP-->
<!--Not Available on  ## Microsoft.WindowsIoT-->
<!--Not Available on  ## Microsoft.WorkloadMonitor-->

## <a name="next-steps"></a>后续步骤
若要了解如何将标记应用于资源，请参见[使用标记来组织 Azure 资源](resource-group-using-tags.md)。

<!-- Update_Description: wording update, update meta properties -->