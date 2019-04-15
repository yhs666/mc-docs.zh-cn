---
title: 由资源类型进行的 Azure 资源管理器完全模式删除
description: 显示资源类型如何在 Azure 资源管理器模板中进行完全模式删除。
author: rockboyfor
ms.service: azure-resource-manager
ms.topic: reference
origin.date: 02/13/2019
ms.date: 03/18/2019
ms.author: v-yeche
ms.openlocfilehash: 664635a1ee1f84ca49e534d819c1beffbfdb9cee
ms.sourcegitcommit: 2836cce46ecb3a8473dfc0ad2c55b1c47d2f0fad
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/09/2019
ms.locfileid: "59355893"
---
# <a name="deletion-of-azure-resources-for-complete-mode-deployments"></a>针对完全模式部署的 Azure 资源删除
本文描述了资源类型如何在不是以完全模式部署的模板中进行删除。

类型不处于以完全模式部署的模板中时，标记为 `Yes` 的资源类型会被删除。 

不处于此模板中时，标记为 `No` 的资源类型不会自动删除；但是，如果删除其父资源，就会删除这些资源类型。 有关此行为的完整描述，请参阅 [Azure 资源管理器部署模式](deployment-modes.md)。

若要以逗号分隔值文件的形式获取同一数据，请下载 [complete-mode-deletion.csv](https://github.com/tfitzmac/resource-capabilities/blob/master/complete-mode-deletion.csv)。

<!--Not Available on ## Microsoft.AAD-->
<!--Not Available on ## microsoft.aadiam-->
<!--Not Available on ## Microsoft.Addons-->
<!--Not Available on ## Microsoft.ADHybridHealthService-->

## <a name="microsoftadvisor"></a>Microsoft.Advisor
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| 配置 | 否 | 
| generateRecommendations | 否 | 
| 建议 | 否 | 
| 禁止显示 | 否 | 

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| actionRules | 否 | 
| alerts | 否 | 
| alertsList | 否 | 
| alertsSummary | 否 | 
| alertsSummaryList | 否 | 
| smartDetectorAlertRules | 否 | 
| smartDetectorRuntimeEnvironments | 否 | 
| smartGroups | 否 | 

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| servers | 是 | 

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| reportFeedback | 否 | 
| 服务 | 是 | 
| validateServiceName | 否 | 

<!--Not Available on ## Microsoft.Attestation-->


## <a name="microsoftauthorization"></a>Microsoft.Authorization
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| classicAdministrators | 否 | 
| denyAssignments | 否 | 
| elevateAccess | 否 | 
| 锁定 | 否 | 
| 权限 | 否 | 
| policyAssignments | 否 | 
| policyDefinitions | 否 | 
| policySetDefinitions | 否 | 
| providerOperations | 否 | 
| roleAssignments | 否 | 
| roleDefinitions | 否 | 

## <a name="microsoftautomation"></a>Microsoft.Automation
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| automationAccounts | 是 | 
| automationAccounts/configurations | 是 | 
| automationAccounts/jobs | 否 | 
| automationAccounts/runbooks | 是 | 
| automationAccounts/softwareUpdateConfigurations | 否 | 
| automationAccounts/webhooks | 否 | 

<!--Not Available on ## Microsoft.Azure.Geneva-->


## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| b2cDirectories | 是 | 

## <a name="microsoftazurestack"></a>Microsoft.AzureStack
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| registrations | 是 | 
| registrations/customerSubscriptions | 否 | 
| registrations/products | 否 | 

## <a name="microsoftbatch"></a>Microsoft.Batch
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| batchAccounts | 是 | 

<!--Not Available on ## Microsoft.Billing-->
<!--Not Available on ## ## Microsoft.BingMaps-->

<!--Not Available on ## Microsoft.BizTalkServices-->
<!--Not Available on ## Microsoft.Blueprint-->
<!--Not Available on ## Microsoft.BotService-->


## <a name="microsoftcache"></a>Microsoft.Cache
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| Redis | 是 | 
| RedisConfigDefinition | 否 | 

<!--Not Available on ## Microsoft.Capacity-->

## <a name="microsoftcdn"></a>Microsoft.Cdn
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| edgenodes | 否 | 
| 配置文件 | 是 | 
| profiles/endpoints | 是 | 
| profiles/endpoints/customdomains | 否 | 
| profiles/endpoints/origins | 否 | 
| validateProbe | 否 | 

<!-- Not Available on ## Microsoft.CertificateRegistration-->

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| capabilities | 否 | 
| domainNames | 否 | 
| domainNames/capabilities | 否 | 
| domainNames/internalLoadBalancers | 否 | 
| domainNames/serviceCertificates | 否 | 
| domainNames/slots | 否 | 
| domainNames/slots/roles | 否 | 
| moveSubscriptionResources | 否 | 
| operatingSystemFamilies | 否 | 
| operatingSystems | 否 | 
| quotas | 否 | 
| resourceTypes | 否 | 
| validateSubscriptionMoveAvailability | 否 | 
| virtualMachines | 否 | 
| virtualMachines/diagnosticSettings | 否 | 

## <a name="microsoftclassicinfrastructuremigrate"></a>Microsoft.ClassicInfrastructureMigrate
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| classicInfrastructureResources | 否 | 

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| capabilities | 否 | 
| expressRouteCrossConnections | 否 | 
| expressRouteCrossConnections/peerings | 否 | 
| gatewaySupportedDevices | 否 | 
| networkSecurityGroups | 否 | 
| quotas | 否 | 
| reservedIps | 否 | 
| virtualNetworks | 否 | 
| virtualNetworks/remoteVirtualNetworkPeeringProxies | 否 | 
| virtualNetworks/virtualNetworkPeerings | 否 | 

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| capabilities | 否 | 
| disks | 否 | 
| images | 否 | 
| osImages | 否 | 
| osPlatformImages | 否 | 
| publicImages | 否 | 
| quotas | 否 | 
| storageAccounts | 否 | 
| storageAccounts/services | 否 | 
| storageAccounts/services/diagnosticSettings | 否 | 
| storageAccounts/vmImages | 否 | 
| vmImages | 否 | 

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| accounts | 是 | 

## <a name="microsoftcommerce"></a>Microsoft.Commerce
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| RateCard | 否 | 
| UsageAggregates | 否 | 

## <a name="microsoftcompute"></a>Microsoft.Compute
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| availabilitySets | 是 | 
| disks | 是 | 
| images | 是 | 
| restorePointCollections | 是 | 
| restorePointCollections/restorePoints | 否 | 
| sharedVMImages | 是 | 
| sharedVMImages/versions | 是 | 
| snapshots | 是 | 
| virtualMachines | 是 | 
| virtualMachines/diagnosticSettings | 否 | 
| virtualMachines/extensions | 是 | 
| virtualMachineScaleSets | 是 | 
| virtualMachineScaleSets/extensions | 否 | 
| virtualMachineScaleSets/networkInterfaces | 否 | 
| virtualMachineScaleSets/publicIPAddresses | 否 | 
| virtualMachineScaleSets/virtualMachines | 否 | 
| virtualMachineScaleSets/virtualMachines/networkInterfaces | 否 | 

<!--Not Available on ## Microsoft.Consumption-->
<!--Not Available on ## Microsoft.ContainerInstance-->


## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| registries | 是 | 
| registries/builds | 否 | 
| registries/builds/cancel | 否 | 
| registries/builds/getLogLink | 否 | 
| registries/buildTasks | 是 | 
| registries/buildTasks/steps | 否 | 
| registries/eventGridFilters | 否 | 
| registries/getBuildSourceUploadUrl | 否 | 
| registries/GetCredentials | 否 | 
| registries/importImage | 否 | 
| registries/queueBuild | 否 | 
| registries/regenerateCredential | 否 | 
| registries/regenerateCredentials | 否 | 
| registries/replications | 是 | 
| registries/runs | 否 | 
| registries/runs/cancel | 否 | 
| registries/scheduleRun | 否 | 
| registries/tasks | 是 | 
| registries/updatePolicies | 否 | 
| registries/webhooks | 是 | 
| registries/webhooks/getCallbackConfig | 否 | 
| registries/webhooks/ping | 否 | 

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| containerServices | 是 | 
| managedClusters | 是 | 

<!--Not Available on ## Microsoft.ContentModerator-->
<!--Not Available on ## Microsoft.CortanaAnalytics-->
<!--Not Available on ## Microsoft.CostManagement-->
<!--Not Available on ## Microsoft.CustomerInsights-->
<!--Not Available on ## Microsoft.DataBox-->
<!--Not Available on ## Microsoft.DataBoxEdge-->
<!--Not Available on ## Microsoft.Databricks-->
<!--Not Available on ## Microsoft.DataCatalog-->
<!--Not Available on ## Microsoft.DataConnect-->
<!--Not Available on ## Microsoft.DataFactory-->
<!--Not Available on ## Microsoft.DataLakeAnalytics-->
<!--Not Available on ## Microsoft.DataLakeStore-->
<!--Not Available on ## Microsoft.DataMigration-->
<!--Not Available on ## Microsoft.DBforMariaDB-->


## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| servers | 是 | 
| servers/recoverableServers | 否 | 
| servers/virtualNetworkRules | 否 | 

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| servers | 是 | 
| servers/advisors | 否 | 
| servers/queryTexts | 否 | 
| servers/recoverableServers | 否 | 
| servers/topQueryStatistics | 否 | 
| servers/virtualNetworkRules | 否 | 
| servers/waitStatistics | 否 | 

## <a name="microsoftdevices"></a>Microsoft.Devices
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| IotHubs | 是 | 
| IotHubs/eventGridFilters | 否 | 
| ProvisioningServices | 是 | 
| usages | 否 | 

<!--Not Available on ## Microsoft.DevSpaces-->
<!--Not Available on ## Microsoft.DevTestLab-->


## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| databaseAccountNames | 否 | 
| databaseAccounts | 是 | 

<!--Not Available on ## Microsoft.DomainRegistration-->
<!--Not Available on ## Microsoft.DynamicsLcs-->
<!--Not Available on ## Microsoft.EventGrid-->


## <a name="microsofteventhub"></a>Microsoft.EventHub
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| clusters | 是 | 
| namespaces | 是 | 
| namespaces/authorizationrules | 否 | 
| namespaces/disasterrecoveryconfigs | 否 | 
| namespaces/eventhubs | 否 | 
| namespaces/eventhubs/authorizationrules | 否 | 
| namespaces/eventhubs/consumergroups | 否 | 

## <a name="microsoftfeatures"></a>Microsoft.Features
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| features | 否 | 
| providers | 否 | 

<!--Not Available on ## Microsoft.Gallery-->

## <a name="microsoftguestconfiguration"></a>Microsoft.GuestConfiguration
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| guestConfigurationAssignments | 否 | 
| software | 否 | 

<!--Not Available on ## Microsoft.HanaOnAzure-->


## <a name="microsofthdinsight"></a>Microsoft.HDInsight
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| clusters | 是 | 
| clusters/applications | 否 | 

## <a name="microsoftimportexport"></a>Microsoft.ImportExport
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| jobs | 是 | 

<!--Not Available on ## Microsoft.InformationProtection-->


## <a name="microsoftinsights"></a>microsoft.insights
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| actiongroups | 是 | 
| activityLogAlerts | 是 | 
| alertrules | 是 | 
| automatedExportSettings | 否 | 
| autoscalesettings | 是 | 
| baseline | 否 | 
| calculatebaseline | 否 | 
| components | 是 | 
| components/events | 否 | 
| components/pricingPlans | 否 | 
| components/query | 否 | 
| diagnosticSettings | 否 | 
| diagnosticSettingsCategories | 否 | 
| eventCategories | 否 | 
| eventtypes | 否 | 
| extendedDiagnosticSettings | 否 | 
| logDefinitions | 否 | 
| logprofiles | 否 | 
| 日志 | 否 | 
| metricAlerts | 是 |
| migrateToNewPricingModel | 否 | 
| myWorkbooks | 否 | 
| 查询 | 否 | 
| rollbackToLegacyPricingModel | 否 | 
| scheduledqueryrules | 是 | 
| vmInsightsOnboardingStatuses | 否 | 
| webtests | 是 | 
| workbooks | 是 | 

<!--Not Available on ## Microsoft.Intune-->
<!--Not Available on ## Microsoft.IoTCentral-->
<!--Not Available on ## Microsoft.IoTSpaces-->


## <a name="microsoftkeyvault"></a>Microsoft.KeyVault
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| deletedVaults | 否 | 
| vaults | 是 | 
| vaults/accessPolicies | 否 | 
| vaults/secrets | 否 | 

<!--Not Available on ## Microsoft.Kusto-->
<!--Not Available on ## Microsoft.LabServices-->
<!--Not Available on ## Microsoft.LocationBasedServices-->
<!--Not Available on ## Microsoft.LocationServices-->
<!--Not Available on ## Microsoft.LogAnalytics-->


## <a name="microsoftlogic"></a>Microsoft.Logic
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| integrationAccounts | 是 | 
| workflows | 是 | 

<!--Not Available on ## Microsoft.MachineLearning-->
<!--Not Available on ## Microsoft.MachineLearningExperimentation-->
<!--Not Available on ## Microsoft.MachineLearningModelManagement-->
<!--Not Available on ## Microsoft.MachineLearningServices-->


## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| 标识 | 否 | 
| userAssignedIdentities | 是 | 

## <a name="microsoftmanagement"></a>Microsoft.Management
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| getEntities | 否 | 
| managementGroups | 否 | 
| 资源 | 否 | 
| startTenantBackfill | 否 | 
| tenantBackfillStatus | 否 | 

<!--Not Available on ## Microsoft.Maps-->
<!--Not Available on ## Microsoft.Marketplace-->
<!--Not Available on ## Microsoft.MarketplaceApps-->
<!--Not Available on ## Microsoft.MarketplaceOrdering-->


## <a name="microsoftmedia"></a>Microsoft.Media
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| mediaservices | 是 | 
| mediaservices/accountFilters | 否 | 
| mediaservices/assets | 否 | 
| mediaservices/assets/assetFilters | 否 | 
| mediaservices/contentKeyPolicies | 否 | 
| mediaservices/eventGridFilters | 否 | 
| mediaservices/liveEventOperations | 否 | 
| mediaservices/liveEvents | 是 | 
| mediaservices/liveEvents/liveOutputs | 否 | 
| mediaservices/liveOutputOperations | 否 | 
| mediaservices/streamingEndpointOperations | 否 | 
| mediaservices/streamingEndpoints | 是 | 
| mediaservices/streamingLocators | 否 | 
| mediaservices/streamingPolicies | 否 | 
| mediaservices/transforms | 否 | 
| mediaservices/transforms/jobs | 否 | 

<!--Not Available on ## Microsoft.Migrate-->


## <a name="microsoftnetwork"></a>Microsoft.Network
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| applicationGateways | 是 | 
| applicationSecurityGroups | 是 | 
| azureFirewallFqdnTags | 否 | 
| azureFirewalls | 是 | 
| bgpServiceCommunities | 否 | 
| connections | 是 | 
| ddosCustomPolicies | 是 | 
| ddosProtectionPlans | 是 | 
| dnsOperationStatuses | 否 | 
| dnszones | 是 | 
| dnszones/A | 否 | 
| dnszones/AAAA | 否 | 
| dnszones/all | 否 | 
| dnszones/CAA | 否 | 
| dnszones/CNAME | 否 | 
| dnszones/MX | 否 | 
| dnszones/NS | 否 | 
| dnszones/PTR | 否 | 
| dnszones/recordsets | 否 | 
| dnszones/SOA | 否 | 
| dnszones/SRV | 否 | 
| dnszones/TXT | 否 | 
| expressRouteCircuits | 是 | 
| expressRouteServiceProviders | 否 | 
| frontdoors | 是 | 
| frontdoorWebApplicationFirewallPolicies | 是 | 
| getDnsResourceReference | 否 | 
| interfaceEndpoints | 是 | 
| internalNotify | 否 | 
| loadBalancers | 是 | 
| localNetworkGateways | 是 | 
| natGateways | 是 | 
| networkIntentPolicies | 是 | 
| networkInterfaces | 是 | 
| networkProfiles | 是 | 
| networkSecurityGroups | 是 | 
| networkWatchers | 是 | 
| networkWatchers/connectionMonitors | 是 | 
| networkWatchers/lenses | 是 | 
| networkWatchers/pingMeshes | 是 | 
| privateLinkServices | 是 | 
| publicIPAddresses | 是 | 
| publicIPPrefixes | 是 | 
| routeFilters | 是 | 
| routeTables | 是 | 
| serviceEndpointPolicies | 是 | 
| trafficManagerGeographicHierarchies | 否 | 
| trafficmanagerprofiles | 是 | 
| trafficmanagerprofiles/heatMaps | 否 | 
| virtualHubs | 是 | 
| virtualNetworkGateways | 是 | 
| virtualNetworks | 是 | 
| virtualNetworkTaps | 是 | 
| virtualWans | 是 | 
| vpnGateways | 是 | 
| vpnSites | 是 | 
| webApplicationFirewallPolicies | 是 | 

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| namespaces | 是 | 
| namespaces/notificationHubs | 是 | 

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| devices | 否 | 
| linkTargets | 否 | 
| storageInsightConfigs | 否 | 
| workspaces | 是 | 
| workspaces/dataSources | 否 | 
| workspaces/linkedServices | 否 | 
| workspaces/query | 否 | 

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| managementassociations | 否 | 
| managementconfigurations | 是 | 
| solutions | 是 | 
| 视图 | 是 | 

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| policyEvents | 否 | 
| policyStates | 否 | 
| policyTrackedResources | 否 | 
| remediations | 否 | 

## <a name="microsoftportal"></a>Microsoft.Portal
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| consoles | 否 | 
| dashboards | 是 | 
| userSettings | 否 | 

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| workspaceCollections | 是 | 

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| capacities | 是 | 

<!--Not Available on ## Microsoft.ProjectOxford-->
 

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| backupProtectedItems | 否 | 
| vaults | 是 | 

## <a name="microsoftrelay"></a>Microsoft.Relay
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| namespaces | 是 | 
| namespaces/authorizationrules | 否 | 
| namespaces/hybridconnections | 否 | 
| namespaces/hybridconnections/authorizationrules | 否 | 
| namespaces/wcfrelays | 否 | 
| namespaces/wcfrelays/authorizationrules | 否 | 

<!--Not Available on ## Microsoft.ResourceGraph-->


## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| availabilityStatuses | 否 | 
| childAvailabilityStatuses | 否 | 
| childResources | 否 | 
| events | 否 | 
| impactedResources | 否 | 
| 通知 | 否 | 

## <a name="microsoftresources"></a>Microsoft.Resources
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| deployments | 否 | 
| deployments/operations | 否 | 
| links | 否 | 
| notifyResourceJobs | 否 | 
| providers | 否 | 
| resourceGroups | 否 | 
| 资源 | 否 | 
| subscriptions | 否 | 
| subscriptions/providers | 否 | 
| subscriptions/resourceGroups | 否 | 
| subscriptions/resourcegroups/resources | 否 | 
| subscriptions/resources | 否 | 
| subscriptions/tagnames | 否 | 
| subscriptions/tagNames/tagValues | 否 | 
| tenants | 否 | 

<!--Not Available on ## Microsoft.SaaS-->


## <a name="microsoftscheduler"></a>Microsoft.Scheduler
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| flows | 是 | 
| jobcollections | 是 | 

<!-- Not Available on ## Microsoft.Search-->
<!-- Not Available on ## Microsoft.Security-->
<!-- Not Available on ## Microsoft.SecurityGraph-->


## <a name="microsoftservicebus"></a>Microsoft.ServiceBus
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| namespaces | 是 | 
| namespaces/authorizationrules | 否 | 
| namespaces/disasterrecoveryconfigs | 否 | 
| namespaces/eventgridfilters | 否 | 
| namespaces/queues | 否 | 
| namespaces/queues/authorizationrules | 否 | 
| namespaces/topics | 否 | 
| namespaces/topics/authorizationrules | 否 | 
| namespaces/topics/subscriptions | 否 | 
| namespaces/topics/subscriptions/rules | 否 | 
| premiumMessagingRegions | 否 | 

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| clusters | 是 | 
| clusters/applications | 否 | 

<!-- Not Available on ## Microsoft.ServiceFabricMesh-->
<!-- Not Available on ## Microsoft.SignalRService-->
<!-- Not Available on ## Microsoft.Solutions-->


## <a name="microsoftsql"></a>Microsoft.SQL
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| managedInstances | 是 |
| managedInstances/databases | 是（见下方备注） |
| managedInstances/databases/backupShortTermRetentionPolicies | 否 |
| managedInstances/databases/schemas/tables/columns/sensitivityLabels | 否 |
| managedInstances/databases/vulnerabilityAssessments | 否 |
| managedInstances/databases/vulnerabilityAssessments/rules/baselines | 否 |
| managedInstances/encryptionProtector | 否 |
| managedInstances/keys | 否 |
| managedInstances/restorableDroppedDatabases/backupShortTermRetentionPolicies | 否 |
| managedInstances/vulnerabilityAssessments | 否 |
| servers | 是 | 
| servers/administrators | 否 | 
| servers/communicationLinks | 否 | 
| servers/databases | 是（见下方备注） | 
| servers/encryptionProtector | 否 | 
| servers/firewallRules | 否 | 
| servers/keys | 否 | 
| servers/restorableDroppedDatabases | 否 | 
| servers/serviceobjectives | 否 | 
| servers/tdeCertificates | 否 | 

> [!NOTE]
> Master 数据库不支持标记，但其他数据库（包括数据仓库数据库）支持标记。

<!-- Not Available on ## Microsoft.SqlVirtualMachine-->

## <a name="microsoftstorage"></a>Microsoft.Storage
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| storageAccounts | 是 | 
| storageAccounts/blobServices | 否 | 
| storageAccounts/fileServices | 否 | 
| storageAccounts/queueServices | 否 | 
| storageAccounts/services | 否 | 
| storageAccounts/tableServices | 否 | 
| usages | 否 | 

<!-- Not Available on ## Microsoft.StorageSync-->
<!-- Not Available on ## Microsoft.StorSimple-->


## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| streamingjobs | 是（见下方备注） | 
| streamingjobs/diagnosticSettings | 否 | 

> [!NOTE]
> Streamingjobs 运行时无法添加标记。 停止要添加标记的资源。

<!-- Not Available on ## Microsoft.Subscription-->
<!-- Not Available on ## microsoft.support-->
<!-- Not Available on ## Microsoft.TerraformOSS-->
<!-- Not Available on ## Microsoft.TimeSeriesInsights-->
<!-- Not Available on ## microsoft.visualstudio-->


## <a name="microsoftweb"></a>Microsoft.Web
| 资源类型 | 完整模式删除 |
| ------------- | ----------- |
| apiManagementAccounts | 否 | 
| apiManagementAccounts/apiAcls | 否 | 
| apiManagementAccounts/apis | 否 | 
| apiManagementAccounts/apis/apiAcls | 否 | 
| apiManagementAccounts/apis/connectionAcls | 否 | 
| apiManagementAccounts/apis/connections | 否 | 
| apiManagementAccounts/apis/connections/connectionAcls | 否 | 
| apiManagementAccounts/apis/localizedDefinitions | 否 | 
| apiManagementAccounts/connectionAcls | 否 | 
| apiManagementAccounts/connections | 否 | 
| billingMeters | 否 | 
| certificates | 是 | 
| connectionGateways | 是 | 
| connections | 是 | 
| customApis | 是 | 
| deletedSites | 否 | 
| functions | 否 | 
| hostingEnvironments | 是 | 
| hostingEnvironments/multiRolePools | 否 | 
| hostingEnvironments/multiRolePools/instances | 否 | 
| hostingEnvironments/workerPools | 否 | 
| hostingEnvironments/workerPools/instances | 否 | 
| publishingUsers | 否 | 
| 建议 | 否 | 
| resourceHealthMetadata | 否 | 
| runtimes | 否 | 
| serverFarms | 是 | 
| serverFarms/workers | 否 | 
| sites | 是 | 
| sites/domainOwnershipIdentifiers | 否 | 
| sites/hostNameBindings | 否 | 
| sites/instances | 否 | 
| sites/instances/extensions | 否 | 
| sites/premieraddons | 是 | 
| sites/recommendations | 否 | 
| sites/resourceHealthMetadata | 否 | 
| sites/slots | 是 | 
| sites/slots/hostNameBindings | 否 | 
| sites/slots/instances | 否 | 
| sites/slots/instances/extensions | 否 | 
| sourceControls | 否 | 
| validate | 否 | 
| verifyHostingEnvironmentVnet | 否 | 

<!--Not Available on ## Microsoft.WindowsDefenderATP-->
<!--Not Available on ## Microsoft.WindowsIoT-->
<!--Not Available on ## Microsoft.WorkloadMonitor-->


## <a name="next-steps"></a>后续步骤
若要了解如何将标记应用于资源，请参见[使用标记来组织 Azure 资源](resource-group-using-tags.md)。

<!--Pending Verify-->
<!--Update_Description: new articles on  -->
<!--ms.date: 03/18/2019-->