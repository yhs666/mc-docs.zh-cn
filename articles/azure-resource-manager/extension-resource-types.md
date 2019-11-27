---
title: 扩展资源类型
description: 列出用于扩展其他资源类型的功能的 Azure 资源类型。
ms.topic: conceptual
origin.date: 10/24/2019
ms.date: 11/25/2019
ms.openlocfilehash: 45b97832a1de25b4fa3be8d09e93d4770c42b775
ms.sourcegitcommit: c5e012385df740bf4a326eaedabb987314c571a1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2019
ms.locfileid: "74203684"
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
- Microsoft.Billing/billingPeriods
- Microsoft.Billing/billingPermissions
- Microsoft.Billing/billingRoleAssignments
- Microsoft.Billing/billingRoleDefinitions
- Microsoft.Billing/createBillingRoleAssignment
- Microsoft.Blueprint/blueprintAssignments
- Microsoft.Blueprint/blueprints
- Microsoft.Consumption/AggregatedCost
- Microsoft.Consumption/Balances
- Microsoft.Consumption/Budgets
- Microsoft.Consumption/Charges
- Microsoft.Consumption/CostTags
- Microsoft.Consumption/Forecasts
- Microsoft.Consumption/Marketplaces
- Microsoft.Consumption/OperationResults
- Microsoft.Consumption/OperationStatus
- Microsoft.Consumption/Pricesheets
- Microsoft.Consumption/ReservationDetails
- Microsoft.Consumption/ReservationRecommendations
- Microsoft.Consumption/ReservationSummaries
- Microsoft.Consumption/ReservationTransactions
- Microsoft.Consumption/Tags
- Microsoft.Consumption/Terms
- Microsoft.Consumption/UsageDetails
- Microsoft.Consumption/credits
- Microsoft.Consumption/events
- Microsoft.Consumption/lots
- Microsoft.Consumption/products
- Microsoft.Consumption/tenants
- Microsoft.ContainerInstance/serviceAssociationLinks
- Microsoft.CostManagement/Alerts
- Microsoft.CostManagement/Budgets
- Microsoft.CostManagement/Dimensions
- Microsoft.CostManagement/Exports
- Microsoft.CostManagement/ExternalSubscriptions
- Microsoft.CostManagement/Forecast
- Microsoft.CostManagement/Query
- Microsoft.CostManagement/Reportconfigs
- Microsoft.CostManagement/Reports
- Microsoft.CostManagement/Views
- Microsoft.CostManagement/showbackRules
- Microsoft.CustomProviders/associations
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
- Microsoft.KubernetesConfiguration/sourceControlConfigurations
- Microsoft.Maintenance/applyUpdates
- Microsoft.Maintenance/configurationAssignments
- Microsoft.Maintenance/updates
- Microsoft.ManagedIdentity/Identities
- Microsoft.ManagedServices/registrationAssignments
- Microsoft.ManagedServices/registrationDefinitions
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
- Microsoft.SecurityInsights/aggregations
- Microsoft.SecurityInsights/alertRuleTemplates
- Microsoft.SecurityInsights/alertRules
- Microsoft.SecurityInsights/bookmarks
- Microsoft.SecurityInsights/cases
- Microsoft.SecurityInsights/dataConnectors
- Microsoft.SecurityInsights/entities
- Microsoft.SecurityInsights/entityQueries
- Microsoft.SecurityInsights/officeConsents
- Microsoft.SecurityInsights/settings
- Microsoft.SoftwarePlan/hybridUseBenefits
- Microsoft.Subscription/CreateSubscription
- microsoft.support/createsupportticket
- microsoft.support/supporttickets
- Microsoft.WorkloadMonitor/components
- Microsoft.WorkloadMonitor/monitorInstances
- Microsoft.WorkloadMonitor/monitors
- Microsoft.WorkloadMonitor/notificationSettings

## <a name="next-steps"></a>后续步骤

- 若要在 Azure 资源管理器模板中获取扩展资源的资源 ID，请使用 [extensionResourceId](resource-group-template-functions-resource.md#extensionresourceid)。

<!--Not Available on [Event Grid Event Subscriptions](https://docs.microsoft.com/azure/templates/microsoft.eventgrid/2019-06-01/eventsubscriptions)-->

<!-- Update_Description: new article about extension resource types -->
<!--NEW.date: 11/25/2019-->
