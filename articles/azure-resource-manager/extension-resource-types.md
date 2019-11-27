---
title: 扩展资源类型
description: 列出用于扩展其他资源类型的功能的 Azure 资源类型。
ms.topic: conceptual
origin.date: 10/24/2019
ms.date: 11/25/2019
ms.openlocfilehash: bba8ce4da8b51949fd6db0ebc2a00acf2aedbec1
ms.sourcegitcommit: 9e92bcf6aa02fc9e7b3a29abadf6b6d1a8ece8c4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2019
ms.locfileid: "74389415"
---
# <a name="resource-types-that-extend-capabilities-of-other-resources"></a>用于扩展其他资源的功能的资源类型

扩展资源是扩展另一资源的功能的资源。 例如，资源锁是扩展资源。 对另一资源应用资源锁可以防止该资源被删除或修改。 单独创建资源锁没有意义。 扩展资源始终会应用到其他资源。

## <a name="extension-resource-types"></a>扩展资源类型

- Microsoft.Advisor/configurations
- Microsoft.Advisor/recommendations
- Microsoft.Advisor/suppressions
- Microsoft.AlertsManagement/alerts
- Microsoft.AlertsManagement/alertsSummary
- Microsoft.Authorization/checkAccess
- Microsoft.Authorization/denyAssignments
- Microsoft.Authorization/locks
- Microsoft.Authorization/permissions
- Microsoft.Authorization/policyAssignments
- Microsoft.Authorization/policyDefinitions
- Microsoft.Authorization/policySetDefinitions
- Microsoft.Authorization/roleAssignments
- Microsoft.Authorization/roleDefinitions

    <!--Not Available on - Microsoft.Billing/*-->
    <!--Not Available on - Microsoft.Blueprint/*-->
    <!--Not Available on - Microsoft.Consumption/*-->
    <!--Not Available on - Microsoft.ContainerInstance/*-->
    <!--Not Available on - Microsoft.CostManagement/*-->
    <!--Not Available on - Microsoft.CustomProviders/*-->
    
- Microsoft.EventGrid/eventSubscriptions
- Microsoft.EventGrid/extensionTopics
- Microsoft.GuestConfiguration/configurationProfileAssignments
- Microsoft.GuestConfiguration/guestConfigurationAssignments
- Microsoft.GuestConfiguration/software
- Microsoft.GuestConfiguration/softwareUpdateProfile
- Microsoft.GuestConfiguration/softwareUpdates
- microsoft.insights/automatedExportSettings
- microsoft.insights/baseline
- microsoft.insights/calculatebaseline
- microsoft.insights/diagnosticSettings
- microsoft.insights/diagnosticSettingsCategories
- microsoft.insights/eventtypes
- microsoft.insights/extendedDiagnosticSettings
- microsoft.insights/guestDiagnosticSettingsAssociation
- microsoft.insights/logDefinitions
- microsoft.insights/logs
- microsoft.insights/metricDefinitions
- microsoft.insights/metricNamespaces
- microsoft.insights/metricbaselines
- microsoft.insights/metrics
- microsoft.insights/myWorkbooks
- microsoft.insights/vmInsightsOnboardingStatuses
    
    <!--Not Available on - Microsoft.KubernetesConfiguration/*-->
    <!--Not Available on - Microsoft.Maintenance/*-->
    
- Microsoft.ManagedIdentity/Identities

    <!--Not Available on - Microsoft.ManagedServices/*-->
    
- Microsoft.OperationalInsights/storageInsightConfigs
- Microsoft.OperationsManagement/managementassociations
- Microsoft.PolicyInsights/policyEvents
- Microsoft.PolicyInsights/policyStates
- Microsoft.PolicyInsights/policyTrackedResources
- Microsoft.PolicyInsights/remediations
- Microsoft.RecoveryServices/backupProtectedItems
- Microsoft.ResourceHealth/availabilityStatuses
- Microsoft.ResourceHealth/childAvailabilityStatuses
- Microsoft.ResourceHealth/childResources
- Microsoft.ResourceHealth/events
- Microsoft.ResourceHealth/impactedResources
- Microsoft.ResourceHealth/notifications
- Microsoft.Resources/links
- Microsoft.Resources/tags
- Microsoft.Security/Compliances
- Microsoft.Security/InformationProtectionPolicies
- Microsoft.Security/adaptiveNetworkHardenings
- Microsoft.Security/advancedThreatProtectionSettings
- Microsoft.Security/assessmentMetadata
- Microsoft.Security/assessments
- Microsoft.Security/complianceResults
- Microsoft.Security/dataCollectionAgents
- Microsoft.Security/dataCollectionResults
- Microsoft.Security/deviceSecurityGroups
- Microsoft.Security/networkData
- Microsoft.Security/serverVulnerabilityAssessments

    <!--Not Available on - Microsoft.SecurityInsights/*-->
    <!--Not Available on - Microsoft.SoftwarePlan/*-->
    <!--Not Available on - Microsoft.Subscription/*-->
    <!--Not Available on - microsoft.support/*-->
    <!--Not Available on - Microsoft.WorkloadMonitor/*-->

## <a name="next-steps"></a>后续步骤

- 若要在 Azure 资源管理器模板中获取扩展资源的资源 ID，请使用 [extensionResourceId](resource-group-template-functions-resource.md#extensionresourceid)。

<!--Not Available on [Event Grid Event Subscriptions](https://docs.microsoft.com/azure/templates/microsoft.eventgrid/2019-06-01/eventsubscriptions)-->

<!-- Update_Description: new article about extension resource types -->
<!--NEW.date: 11/25/2019-->
