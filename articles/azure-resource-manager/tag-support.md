---
title: 针对资源的 Azure 资源管理器标记支持
description: 显示支持标记的 Azure资源类型。 提供所有 Azure 服务的详细信息。
author: rockboyfor
ms.service: azure-resource-manager
ms.topic: reference
origin.date: 12/21/2018
ms.date: 01/21/2019
ms.author: v-yeche
ms.openlocfilehash: a98e62425ea265f88f1401139c2547e362e82614
ms.sourcegitcommit: db9c7f1a7bc94d2d280d2f43d107dc67e5f6fa4c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/10/2019
ms.locfileid: "54193191"
---
# <a name="tag-support-for-azure-resources"></a>Azure 资源的标记支持
本文介绍某一资源类型是否支持[标记](resource-group-using-tags.md)。

## <a name="aad-domain-services"></a>AAD 域服务
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| domains | 否 | 

## <a name="ad-hybrid-health-service"></a>AD 混合运行状况服务
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| services | 否 | 
| addsservices | 否 | 
| 配置 | 否 | 
| 代理 | 否 | 
| aadsupportcases | 否 | 
| 报表 | 否 | 
| servicehealthmetrics | 否 | 
| 日志 | 否 | 
| anonymousapiusers | 否 | 

## <a name="analysis-services"></a>Analysis Services
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| servers | 是 | 

## <a name="api-hubs"></a>API 中心
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| apiManagementAccounts | 否 | 
| apiManagementAccounts/connectionProviders | 否 | 
| apiManagementAccounts/connections | 否 | 
| apiManagementAccounts/connectionAcls | 否 | 
| apiManagementAccounts/connectionProviderAcls | 否 | 
| apiManagementAccounts/apis | 否 | 

## <a name="api-management"></a>API 管理
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| 服务 | 是 | 

## <a name="automation"></a>自动化
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| automationAccounts | 是 | 
| automationAccounts/runbooks | 是 | 
| automationAccounts/configurations | 是 | 
| automationAccounts/webhooks | 否 | 
| automationAccounts/softwareUpdateConfigurations | 否 | 
| automationAccounts/jobs | 否 | 

## <a name="batch"></a>批处理
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| batchAccounts | 是 | 

<!--Not Available on ## Bing Maps-->
<!--Not Available on ## Biztalk Services-->

## <a name="cache"></a>缓存
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| Redis | 是 | 

<!--Not Available on ## CDN-->
## <a name="classic-compute"></a>经典计算
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| domainNames | 否 | 
| domainNames/slots | 否 | 
| domainNames/slots/roles | 否 | 
| virtualMachines | 否 | 
| virtualMachines/diagnosticSettings | 否 | 
| virtualMachines/metricDefinitions | 否 | 
| virtualMachines/metrics | 否 | 

## <a name="classic-infrastructure-migrate"></a>经典的基础结构迁移
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| classicInfrastructureResources | 否 | 

## <a name="classic-network"></a>经典网络
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| virtualNetworks | 否 | 
| virtualNetworks/virtualNetworkPeerings | 否 | 
| virtualNetworks/remoteVirtualNetworkPeeringProxies | 否 | 

## <a name="classic-storage"></a>经典存储
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| storageAccounts/services | 否 | 
| storageAccounts/services/diagnosticSettings | 否 | 

## <a name="compute"></a>计算
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| availabilitySets | 是 | 
| virtualMachines | 是 | 
| virtualMachines/extensions | 是 | 
| virtualMachineScaleSets | 是 | 
| virtualMachineScaleSets/extensions | 否 | 
| virtualMachineScaleSets/virtualMachines | 否 | 
| virtualMachineScaleSets/networkInterfaces | 否 | 
| virtualMachineScaleSets/virtualMachines/networkInterfaces | 否 | 
| virtualMachineScaleSets/publicIPAddresses | 否 | 
| restorePointCollections | 是 | 
| restorePointCollections/restorePoints | 否 | 
| virtualMachines/diagnosticSettings | 否 | 
| virtualMachines/metricDefinitions | 否 | 
| sharedVMImages | 是 | 
| sharedVMImages/versions | 是 | 
| disks | 是 | 
| snapshots | 是 | 
| images | 是 | 

<!--Not Available on ## Container-->
<!--Not Available on ## Container Instance-->
<!--Not Available on ## Container Service-->
<!--Not Available on ## Cortana Analytics-->
## <a name="cosmos-db"></a>Cosmos DB
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| databaseAccounts | 是 | 
| databaseAccountNames | 否 | 

<!--Not Available on ## Cost Management-->
<!--Not Available on ## Data Box Edge-->
<!--Not Available on ## Data Catalog-->
<!--Not Available on ## Data Connect-->
<!--Not Available on ## Data Factory-->
## <a name="devices"></a>设备
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| IotHubs | 是 | 
| IotHubs/eventGridFilters | 否 | 
| ProvisioningServices | 是 | 

<!--Not Available on ## Devspaces-->
<!--Not Available on ## Devtest Lab-->
<!--Not Available on ## Dynamics LCS-->
<!--Not Available on ## Event Grid-->
## <a name="event-hub"></a>事件中心
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| namespaces | 是 | 
| clusters | 是 | 

<!--Not Available on ## Hana on Azure-->
## <a name="hdinsight"></a>HDInsight
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| clusters | 是 | 
| clusters/applications | 否 | 

<!--Not Available on ## Import Export-->
<!--Not Available on ## Insights-->
## <a name="key-vault"></a>密钥保管库
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| vaults | 是 | 
| vaults/secrets | 否 | 
| vaults/accessPolicies | 否 | 
| deletedVaults | 否 | 

<!--Not Available on ## Log Analytics-->
## <a name="logic"></a>逻辑
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| workflows | 是 | 
| integrationAccounts | 是 | 

<!--Not Available on ## Machine Learning Services-->
<!--Not Available on ## Managed Identity-->
## <a name="mariadb"></a>MariaDB
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| servers | 是 | 
| servers/recoverableServers | 否 | 
| servers/virtualNetworkRules | 否 | 

## <a name="marketplace-apps"></a>市场应用
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| classicDevServices | 是 | 

## <a name="marketplace-ordering"></a>市场订购
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| 协议 | 否 | 
| offertypes | 否 | 

## <a name="media"></a>媒体
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| mediaservices | 是 | 
| mediaservices/assets | 否 | 
| mediaservices/contentKeyPolicies | 否 | 
| mediaservices/streamingLocators | 否 | 
| mediaservices/streamingPolicies | 否 | 
| mediaservices/eventGridFilters | 否 | 
| mediaservices/transforms | 否 | 
| mediaservices/transforms/jobs | 否 | 
| mediaservices/streamingEndpoints | 是 | 
| mediaservices/liveEvents | 是 | 
| mediaservices/liveEvents/liveOutputs | 否 | 
| mediaservices/streamingEndpointOperations | 否 | 
| mediaservices/liveEventOperations | 否 | 
| mediaservices/liveOutputOperations | 否 | 
| mediaservices/assets/assetFilters | 否 | 
| mediaservices/accountFilters | 否 | 

## <a name="mysql"></a>MySQL
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| servers | 是 | 
| servers/recoverableServers | 否 | 
| servers/virtualNetworkRules | 否 | 

## <a name="network"></a>网络
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| virtualNetworks | 是 | 
| publicIPAddresses | 是 | 
| networkInterfaces | 是 | 
| interfaceEndpoints | 是 | 
| loadBalancers | 是 | 
| networkSecurityGroups | 是 | 
| applicationSecurityGroups | 是 | 
| serviceEndpointPolicies | 是 | 
| networkIntentPolicies | 是 | 
| routeTables | 是 | 
| publicIPPrefixes | 是 | 
| networkWatchers | 是 | 
| networkWatchers/connectionMonitors | 是 | 
| networkWatchers/lenses | 是 | 
| networkWatchers/pingMeshes | 是 | 
| virtualNetworkGateways | 是 | 
| localNetworkGateways | 是 | 
| connections | 是 | 
| applicationGateways | 是 | 
| expressRouteCircuits | 是 | 
| routeFilters | 是 | 
| virtualWans | 是 | 
| vpnSites | 是 | 
| virtualHubs | 是 | 
| vpnGateways | 是 | 
| azureFirewalls | 是 | 
| virtualNetworkTaps | 是 | 
| privateLinkServices | 是 | 
| ddosProtectionPlans | 是 | 
| networkProfiles | 是 | 
| frontdoors | 是 | 
| frontdoorWebApplicationFirewallPolicies | 是 | 
| webApplicationFirewallPolicies | 是 | 

## <a name="notification-hubs"></a>通知中心
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| namespaces | 是 | 
| namespaces/notificationHubs | 是 | 

## <a name="portal"></a>门户
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| dashboards | 是 | 

## <a name="portal-sdk"></a>门户 SDK
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| rootResources | 是 | 

## <a name="postgresql"></a>PostgreSQL
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| servers | 是 | 
| servers/recoverableServers | 否 | 
| servers/virtualNetworkRules | 否 | 
| servers/topQueryStatistics | 否 | 
| servers/queryTexts | 否 | 
| servers/waitStatistics | 否 | 
| servers/advisors | 否 | 

## <a name="power-bi"></a>Power BI
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| workspaceCollections | 是 | 

## <a name="recovery-services"></a>恢复服务
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| vaults | 是 | 
| backupProtectedItems | 否 | 

## <a name="relay"></a>中继
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| namespaces | 是 | 

## <a name="resources"></a>资源
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| resourceGroups | 是 | 
| subscriptions/resourceGroups | 是 | 

## <a name="scheduler"></a>计划程序
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| jobcollections | 是 | 
| flows | 是 | 

<!--Not Available on ## Search-->
## <a name="security"></a>安全性
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| dataCollectionAgents | 否 | 

## <a name="service-bus"></a>服务总线
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| namespaces | 是 | 
| namespaces/eventgridfilters | 否 | 

## <a name="service-fabric"></a>Service Fabric
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| clusters | 是 | 
| clusters/applications | 否 | 

<!--Not Available on ## Service Fabric Mesh-->
<!--Not Available on ## SignalR Service-->
## <a name="site-recovery"></a>站点恢复
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| SiteRecoveryVault | 是 | 

<!--Not Available on ## Solutions-->
## <a name="sql-virtual-machine"></a>SQL 虚拟机
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| DWVM | 是 | 

## <a name="storage"></a>存储
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| storageAccounts | 是 | 
| storageAccounts/blobServices | 否 | 
| storageAccounts/tableServices | 否 | 
| storageAccounts/queueServices | 否 | 
| storageAccounts/fileServices | 否 | 
| storageAccounts/services | 否 | 
| storageAccounts/services/metricDefinitions | 否 | 

## <a name="storage-sync"></a>存储同步
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| storageSyncServices | 是 | 
| storageSyncServices/syncGroups | 否 | 
| storageSyncServices/syncGroups/cloudEndpoints | 否 | 
| storageSyncServices/syncGroups/serverEndpoints | 否 | 
| storageSyncServices/registeredServers | 否 | 
| storageSyncServices/workflows | 否 | 

## <a name="storsimple"></a>Storsimple
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| managers | 是 | 

## <a name="stream-analytics"></a>流分析
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| streamingjobs | 是 | 
| streamingjobs/diagnosticSettings | 否 | 
| streamingjobs/metricDefinitions | 否 | 

## <a name="subscription"></a>订阅
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| SubscriptionDefinitions | 否 | 
| SubscriptionOperations | 否 | 

## <a name="support"></a>支持
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| supporttickets | 否 | 

## <a name="visual-studio"></a>Visual Studio
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| 帐户 | 是 | 
| account/project | 是 | 
| account/extension | 是 | 
| 帐户 | 是 | 
| account/project | 是 | 
| account/extension | 是 | 

## <a name="web"></a>Web
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| sites/instances | 否 | 
| sites/slots/instances | 否 | 
| sites/instances/extensions | 否 | 
| sites/slots/instances/extensions | 否 | 
| publishingUsers | 否 | 
| validate | 否 | 
| sourceControls | 否 | 
| sites/hostNameBindings | 否 | 
| sites/domainOwnershipIdentifiers | 否 | 
| sites/slots/hostNameBindings | 否 | 
| certificates | 是 | 
| serverFarms | 是 | 
| serverFarms/workers | 否 | 
| sites | 是 | 
| sites/slots | 是 | 
| sites/metrics | 否 | 
| sites/slots/metrics | 否 | 
| sites/premieraddons | 是 | 
| hostingEnvironments | 是 | 
| hostingEnvironments/multiRolePools | 否 | 
| hostingEnvironments/workerPools | 否 | 
| hostingEnvironments/metrics | 否 | 
| functions | 否 | 
| deletedSites | 否 | 
| apiManagementAccounts | 否 | 
| apiManagementAccounts/connections | 否 | 
| apiManagementAccounts/connectionAcls | 否 | 
| apiManagementAccounts/apis/connections/connectionAcls | 否 | 
| apiManagementAccounts/apis/connectionAcls | 否 | 
| apiManagementAccounts/apiAcls | 否 | 
| apiManagementAccounts/apis/apiAcls | 否 | 
| apiManagementAccounts/apis | 否 | 
| apiManagementAccounts/apis/localizedDefinitions | 否 | 
| apiManagementAccounts/apis/connections | 否 | 
| connections | 是 | 
| customApis | 是 | 
| connectionGateways | 是 | 
| billingMeters | 否 | 
| verifyHostingEnvironmentVnet | 否 | 

## <a name="xrm"></a>XRM
| 资源类型 | 支持标记 |
| ------------- | ----------- |
| 组织 | 否 | 

## <a name="next-steps"></a>后续步骤
若要了解如何将标记应用于资源，请参见[使用标记来组织 Azure 资源](resource-group-using-tags.md)。

<!-- Update_Description: new articles on azure resource manager tag support -->
<!--ms.date: 01/14/2019-->