---
title: "Azure 监视 REST API 演练 | Azure"
description: "如何对请求进行身份验证，以及如何使用 Azure Monitor REST API 检索可用的指标定义和指标值。"
author: mcollier
manager: 
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 565e6a88-3131-4a48-8b82-3effc9a3d5c6
ms.service: monitoring-and-diagnostics
ms.workload: 
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
origin.date: 09/18/2017
ms.author: v-yiso
ms.date: 12/11/2017
ms.openlocfilehash: e8023ebc85fc162d2db52b03de6fc42ee3c50578
ms.sourcegitcommit: 2291ca1f5cf86b1402c7466d037a610d132dbc34
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2017
---
# <a name="azure-monitoring-rest-api-walkthrough"></a>Azure 监视 REST API 演练
本文说明如何执行身份验证，使代码能够遵循 [Microsoft Azure Monitor REST API 参考](https://msdn.microsoft.com/library/azure/dn931943.aspx)。         

使用 Azure Monitor API 能够以编程方式检索可用的默认指标定义、粒度和指标值。 可将数据保存在独立的数据存储（例如 Azure SQL 数据库、Azure Cosmos DB）中。 然后，可以根据需要从该处执行其他分析。

除了处理各种指标数据点以外，使用监视 API 还可以列出警报规则、查看活动日志以及执行其他许多操作。 有关可用操作的完整列表，请参阅 [Microsoft Azure Monitor REST API 参考](https://msdn.microsoft.com/library/azure/dn931943.aspx)。

## <a name="authenticating-azure-monitor-requests"></a>对 Azure Monitor 请求进行身份验证
第一步是对请求进行身份验证。

针对 Azure 监视器 API 执行的所有任务都使用 Azure Resource Manager 身份验证模型。 因此，所有请求必须使用 Azure Active Directory (Azure AD) 进行身份验证。 对客户端应用程序进行身份验证的方法之一是创建 Azure AD 服务主体，并检索身份验证 (JWT) 令牌。 以下示例脚本演示如何通过 PowerShell 创建 Azure AD 服务主体。 有关更详细的演练，请参阅有关[使用 Azure PowerShell 创建用于访问资源的服务主体](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-password)的文档。 还可以[通过 Azure 门户创建服务主体](../azure-resource-manager/resource-group-create-service-principal-portal.md)。

```PowerShell
$subscriptionId = "{azure-subscription-id}"
$resourceGroupName = "{resource-group-name}"
$location = "chinaeast"

# Authenticate to a specific Azure subscription.
Login-AzureRmAccount -SubscriptionId $subscriptionId

# Password for the service principal
$pwd = "{service-principal-password}"

# Create a new Azure AD application
$azureAdApplication = New-AzureRmADApplication `
                        -DisplayName "My Azure Monitor" `
                        -HomePage "https://localhost/azure-monitor" `
                        -IdentifierUris "https://localhost/azure-monitor" `
                        -Password $pwd

# Create a new service principal associated with the designated application
New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

# Assign Reader role to the newly created service principal
New-AzureRmRoleAssignment -RoleDefinitionName Reader `
                          -ServicePrincipalName $azureAdApplication.ApplicationId.Guid

```

若要查询 Azure 监视器 API，客户端应用程序应使用事先创建的服务主体进行身份验证。 以下示例 PowerShell 脚本演示了一种使用 [Active Directory 身份验证库](../active-directory/active-directory-authentication-libraries.md) (ADAL) 来获取 JWT 身份验证令牌的方法。 JWT 令牌作为请求中 HTTP 授权参数的一部分传递给 Azure Monitor REST API。

```PowerShell
$azureAdApplication = Get-AzureRmADApplication -IdentifierUri "https://localhost/azure-monitor"

$subscription = Get-AzureRmSubscription -SubscriptionId $subscriptionId

$clientId = $azureAdApplication.ApplicationId.Guid
$tenantId = $subscription.TenantId
$authUrl = "https://login.chinacloudapi.cn/${tenantId}"

$AuthContext = [Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext]$authUrl
$cred = New-Object -TypeName Microsoft.IdentityModel.Clients.ActiveDirectory.ClientCredential -ArgumentList ($clientId, $pwd)

$result = $AuthContext.AcquireToken("https://management.core.chinacloudapi.cn/", $cred)

# Build an array of HTTP header values
$authHeader = @{
'Content-Type'='application/json'
'Accept'='application/json'
'Authorization'=$result.CreateAuthorizationHeader()
}
```

进行身份验证后，可以针对 Azure Monitor REST API 执行查询。 有两个有用的查询：

1. 列出资源的指标定义
2. 检索指标值

## <a name="retrieve-metric-definitions-multi-dimensional-api"></a>检索指标定义（多维 API）

使用[Azure 监视器指标定义 REST API](https://docs.microsoft.com/en-us/rest/api/monitor/metricdefinitions)访问服务可用的指标列表。

**方法**：GET

**请求 URI**：https://management.azure.cn/subscriptions/*{subscriptionId}*/resourceGroups/*{resourceGroupName}*/providers/*{resourceProviderNamespace}*/*{resourceType}*/*{resourceName*/providers/microsoft.insights/metricDefinitions?api-version=*{apiVersion}*

例如，若要检索 Azure 存储帐户的指标定义，请求将如下所示：

```PowerShell
$request = "https://management.azure.cn/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/accounts/ContosoStorage/providers/microsoft.insights/metricDefinitions?api-version=2017-05-01-preview"

Invoke-RestMethod -Uri $request `
                  -Headers $authHeader `
                  -Method Get `
                  -OutFile ".\contosostorage-metricdef-results.json" `
                  -Verbose

```
> [!NOTE]
> 若要使用多维 Azure Monitor 指标 REST API 检索指标定义，请使用“2017-05-01-preview”作为 API 版本。
>
>

生成的 JSON 响应正文将类似于以下示例：（请注意，第二个指标具有维）

```JSON
{
    "value": [
        {
            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/accounts/ContosoStorage/providers/microsoft.insights/metricdefinitions/UsedCapacity",
            "resourceId": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/accounts/ContosoStorage",
            "category": "Capacity",
            "name": {
                "value": "UsedCapacity",
                "localizedValue": "Used capacity"
            },
            "isDimensionRequired": false,
            "unit": "Bytes",
            "primaryAggregationType": "Average",
            "metricAvailabilities": [
                {
                    "timeGrain": "PT1M",
                    "retention": "P30D"
                },
                {
                    "timeGrain": "PT1H",
                    "retention": "P30D"
                }
            ]
        },
        {
            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/accounts/ContosoStorage/providers/microsoft.insights/metricdefinitions/Transactions",
            "resourceId": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/accounts/ContosoStorage",
            "category": "Transaction",
            "name": {
                "value": "Transactions",
                "localizedValue": "Transactions"
            },
            "isDimensionRequired": false,
            "unit": "Count",
            "primaryAggregationType": "Total",
            "metricAvailabilities": [
                {
                    "timeGrain": "PT1M",
                    "retention": "P30D"
                },
                {
                    "timeGrain": "PT1H",
                    "retention": "P30D"
                }
            ],
            "dimensions": [
                {
                    "value": "ResponseType",
                    "localizedValue": "Response type"
                },
                {
                    "value": "GeoType",
                    "localizedValue": "Geo type"
                },
                {
                    "value": "ApiName",
                    "localizedValue": "API name"
                }
            ]
        },
        ...
    ]
}
```

## <a name="retrieve-dimension-values-multi-dimensional-api"></a>检索维值（多维 API）
了解可用的指标定义后，可能会发现一些指标具有多个维。 在查询指标前，可能需要查明某个维具有的值的范围。 然后，根据这些维值，在查询指标时，可以选择根据维值对指标进行筛选或分段。 将指标的名称“value”（而不是“localizedValue”）用于任何筛选请求（例如，检索“CpuTime”和“Requests”指标数据点）。 如果未指定筛选器，则返回默认指标。

> [!NOTE]
> 若要使用 Azure Monitor REST API 检索维值，请使用“2017-05-01-preview”作为 API 版本。
>
>

**方法**：GET

**请求 URI**：https://management.azure.cn/subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/*{resource-provider-namespace}*/*{resource-type}*/*{resource-name}*/providers/microsoft.insights/metrics?metric=*{metric}*&timespan=*{starttime/endtime}*&$filter=*{filter}*&resultType=metadata&api-version=*{apiVersion}*

例如，若要检索“Transactions”指标的“API Name 维”在给定时间范围内的可能值的列表，请求将如下所示：

```PowerShell
$filter = "APIName eq '*'"
$request = "https://management.azure.cn/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/accounts/ContosoStorage/providers/microsoft.insights/metrics?metric=Transactions&timespan=2017-09-01T00:00:00Z/2017-09-10T00:00:00Z&resultType=metadata&$filter=${filter}&api-version=2017-05-01-preview"
Invoke-RestMethod -Uri $request `
    -Headers $authHeader `
    -Method Get `
    -OutFile ".\contosostorage-dimension-values.json" `
    -Verbose
```
生成的 JSON 响应正文将类似于以下示例：

```JSON
{
  "timespan": "2017-09-01T00:00:00Z/2017-09-10T00:00:00Z",
  "value": [
    {
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/accounts/ContosoStorage/providers/Microsoft.Insights/metrics/Transactions",
      "type": "Microsoft.Insights/metrics",
      "name": {
        "value": "Transactions",
        "localizedValue": "Transactions"
      },
      "unit": "Count",
      "timeseries": [
        {
          "metadatavalues": [
            {
              "name": {
                "value": "apiname",
                "localizedValue": "apiname"
              },
              "value": "DeleteBlob"
            }
          ]
        },
        {
          "metadatavalues": [
            {
              "name": {
                "value": "apiname",
                "localizedValue": "apiname"
              },
              "value": "SetBlobProperties"
            }
          ]
        },
        {
          "metadatavalues": [
            {
              "name": {
                "value": "apiname",
                "localizedValue": "apiname"
              },
              "value": "PutPage"
            }
          ]
        },
        {
          "metadatavalues": [
            {
              "name": {
                "value": "apiname",
                "localizedValue": "apiname"
              },
              "value": "Unknown"
            }
          ]
        },
        ...
      ]    
    }
  ]
}
```

## <a name="retrieve-metric-values-multi-dimensional-api"></a>检索指标值（多维 API）
知道可用的指标定义和可能的维值后，即可检索相关的指标值。 对于任何筛选请求，请使用指标的名称“value”（而非“localizedValue”）。 如果未指定维筛选器，则会返回汇总的聚合指标。

> [!NOTE]
> 若要使用 Azure Monitor REST API 检索多维指标值，请使用“2017-05-01-preview”作为 API 版本。
>
>

**方法**：GET

**请求 URI**：https://management.azure.cn/subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/*{resource-provider-namespace}*/*{resource-type}*/*{resource-name}*/providers/microsoft.insights/metrics?metric=*{metric}*&timespan=*{starttime/endtime}*&$filter=*{filter}*&interval=*{timeGrain}*&aggregation=*{aggreation}*&api-version=*{apiVersion}*

例如，若要针对 API 名称“GetBlobProperties”的所有事务处理检索存储“Transactions”指标在 5 分钟范围内的指标值，请求将如下所示：

```PowerShell
$filter = "APIName eq 'GetBlobProperties'"
$request = "https://management.azure.cn/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/accounts/ContosoStorage/providers/microsoft.insights/metrics?metric=Transactions&timespan=2017-09-19T02:00:00Z/2017-09-19T02:05:00Z&$filter=${filter}&interval=PT1M&aggregation=Count&api-version=2017-05-01-preview"
Invoke-RestMethod -Uri $request `
                  -Headers $authHeader `
                  -Method Get `
    -OutFile ".\contosostorage-metric-values.json" `
                  -Verbose
```
生成的 JSON 响应正文将类似于以下示例：

```JSON
{
  "cost": 0,
  "timespan": "2017-09-19T02:00:00Z/2017-09-19T02:05:00Z",
  "interval": "PT1M",
  "value": [
    {
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/accounts/ContosoStorage/providers/Microsoft.Insights/metrics/Transactions",
      "type": "Microsoft.Insights/metrics",
      "name": {
        "value": "Transactions",
        "localizedValue": "Transactions"
      },
      "unit": "Count",
      "timeseries": [
        {
          "metadatavalues": [
            {
              "name": {
                "value": "apiname",
                "localizedValue": "apiname"
              },
              "value": "GetBlobProperties"
            }
          ],
          "data": [
            {
              "timeStamp": "2017-09-19T02:00:00Z",
              "count": 2.0
            },
            {
              "timeStamp": "2017-09-19T02:01:00Z",
              "count": 1.0
            },
            {
              "timeStamp": "2017-09-19T02:02:00Z",
              "count": 3.0
            },
            {
              "timeStamp": "2017-09-19T02:03:00Z",
              "count": 7.0
            },
            {
              "timeStamp": "2017-09-19T02:04:00Z",
              "count": 2.0
            }
          ]
        }
      ]
    }
  ]
}
```

## <a name="retrieve-metric-definitions"></a>检索指标定义
使用 [Azure Monitor 指标定义 REST API](https://msdn.microsoft.com/library/mt743621.aspx) 可以访问服务可用的指标列表。

**方法**：GET

**请求 URI**：https://management.azure.cn/subscriptions/*{subscriptionId}*/resourceGroups/*{resourceGroupName}*/providers/*{resourceProviderNamespace}*/*{resourceType}*/*{resourceName}*/providers/microsoft.insights/metricDefinitions?api-version=*{apiVersion}*


```
> [!NOTE]
> To retrieve metric definitions using the Azure Monitor REST API, use "2016-03-01" as the API version.
>
>


For more information, see the [List the metric definitions for a resource in Azure Monitor REST API](https://msdn.microsoft.com/library/azure/mt743621.aspx) documentation.

## Retrieve metric values
Once the available metric definitions are known, it is then possible to retrieve the related metric values. Use the metric’s name ‘value’ (not the ‘localizedValue’) for any filtering requests (for example, retrieve the ‘CpuTime’ and ‘Requests’ metric data points). If no filters are specified, the default metric is returned.

> [!NOTE]
> To retrieve metric values using the Azure Monitor REST API, use "2016-09-01" as the API version.
>
>

**Method**: GET

**Request URI**: https://management.azure.cn/subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/*{resource-provider-namespace}*/*{resource-type}*/*{resource-name}*/providers/microsoft.insights/metrics?$filter=*{filter}*&api-version=*{apiVersion}*

For example, to retrieve the RunsSucceeded metric data points for the given time range and for a time grain of 1 hour, the request would be as follows:

```PowerShell
$filter = "(name.value eq 'RunsSucceeded') and aggregationType eq 'Total' and startTime eq 2017-08-18T19:00:00 and endTime eq 2017-08-18T23:00:00 and timeGrain eq duration'PT1H'"
$request = "https://management.azure.cn/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/microsoft.insights/metrics?$filter=${filter}&api-version=2016-09-01"
Invoke-RestMethod -Uri $request `
    -Headers $authHeader `
    -Method Get `
    -OutFile ".\contostweets-metrics-results.json" `
    -Verbose
```

生成的 JSON 响应正文将类似于以下示例：

```JSON
{
  "value": [
    {
      "data": [
        {
          "timeStamp": "2017-08-18T19:00:00Z",
          "total": 0.0
        },
        {
          "timeStamp": "2017-08-18T20:00:00Z",
          "total": 159.0
        },
        {
          "timeStamp": "2017-08-18T21:00:00Z",
          "total": 174.0
        },
        {
          "timeStamp": "2017-08-18T22:00:00Z",
          "total": 97.0
        }
      ],
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/Microsoft.Insights/metrics/RunsSucceeded",
      "name": {
        "value": "RunsSucceeded",
        "localizedValue": "Runs Succeeded"
      },
      "type": "Microsoft.Insights/metrics",
      "unit": "Count"
    }
  ]
}
```

要检索多个数据点或聚合点，请将指标定义名称和聚合类型添加到筛选器，如以下示例中所示：

```PowerShell
$filter = "(name.value eq 'ActionsCompleted' or name.value eq 'RunsSucceeded') and (aggregationType eq 'Total' or aggregationType eq 'Average') and startTime eq 2017-08-18T21:00:00 and endTime eq 2017-08-18T21:30:00 and timeGrain eq duration'PT1M'"
$request = "https://management.azure.cn/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/microsoft.insights/metrics?$filter=${filter}&api-version=2016-09-01"
Invoke-RestMethod -Uri $request `
    -Headers $authHeader `
    -Method Get `
    -OutFile ".\contostweets-metrics-multiple-results.json" `
    -Verbose
```
生成的 JSON 响应正文将类似于以下示例：

```JSON
{
  "value": [
    {
      "data": [
        {
          "timeStamp": "2017-08-18T21:03:00Z",
          "total": 5.0,
          "average": 1.0
        },
        {
          "timeStamp": "2017-08-18T21:04:00Z",
          "total": 7.0,
          "average": 1.0
        }
      ],
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/Microsoft.Insights/metrics/ActionsCompleted",
      "name": {
        "value": "ActionsCompleted",
        "localizedValue": "Actions Completed "
      },
      "type": "Microsoft.Insights/metrics",
      "unit": "Count"
    },
    {
      "data": [
        {
          "timeStamp": "2017-08-18T21:03:00Z",
          "total": 5.0,
          "average": 1.0
        },
        {
          "timeStamp": "2017-08-18T21:04:00Z",
          "total": 7.0,
          "average": 1.0
        }
      ],
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/Microsoft.Insights/metrics/RunsSucceeded",
      "name": {
        "value": "RunsSucceeded",
        "localizedValue": "Runs Succeeded"
      },
      "type": "Microsoft.Insights/metrics",
      "unit": "Count"
    }
  ]
}
```

### <a name="use-armclient"></a>使用 ARMClient
另一种方法是使用 Windows 计算机上的 [ARMClient](https://github.com/projectkudu/armclient)。 ARMClient 将自动处理 Azure AD 身份验证（及生成的 JWT 令牌）。 以下步骤概述如何使用 ARMClient 检索指标数据：

1. 安装 [Chocolatey](https://chocolatey.org/) 和 [ARMClient](https://github.com/projectkudu/armclient)。
2. 在终端窗口中，键入 *armclient.exe login*。 这样做会提示登录到 Azure。
3. 键入 *armclient GET [your_resource_id]/providers/microsoft.insights/metricdefinitions?api-version=2016-03-01*
4. 键入 *armclient GET [your_resource_id]/providers/microsoft.insights/metrics?api-version=2016-09-01*



## <a name="retrieve-the-resource-id"></a>检索资源 ID
使用 REST API 确实有助于了解可用的指标定义、粒度和相关值。 使用 [Azure 管理库](https://msdn.microsoft.com/library/azure/mt417623.aspx)时，这些信息很有用。

对于上述代码，要使用的资源 ID 是所需 Azure 资源的完整路径。 例如，要针对某个 Azure Web 应用执行查询，资源 ID 将是：

*/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{site-name}/*

以下列表包含各种 Azure 资源的几个资源 ID 格式示例：

* **IoT 中心** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Devices/IotHubs/*{iot-hub-name}*
* **SQL 弹性池** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Sql/servers/*{pool-db}*/elasticpools/*{sql-pool-name}*
* **SQL 数据库 (v12)** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Sql/servers/*{server-name}*/databases/*{database-name}*
* **服务总线** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.ServiceBus/*{namespace}*/*{servicebus-name}*
* **虚拟机规模集** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Compute/virtualMachineScaleSets/*{vm-name}*
* **VM** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Compute/virtualMachines/*{vm-name}*
* **事件中心** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.EventHub/namespaces/*{eventhub-namespace}*

还可以通过其他方法检索资源 ID，包括通过 Azure 门户、PowerShell 或 Azure CLI 查看所需的资源。

### <a name="azure-portal"></a>Azure 门户
还可以从 Azure 门户获取资源 ID。 为此，请导航到所需的资源，并选择“属性”。 资源 ID 将显示在“属性”部分中，如以下屏幕截图中所示：

![Alt "Azure 门户的“属性”边栏选项卡中显示的资源 ID"](./media/monitoring-rest-api-walkthrough/resourceid_azure_portal.png)

### <a name="azure-powershell"></a>Azure PowerShell
也可以使用 Azure PowerShell cmdlet 检索资源 ID。 例如，若要获取某个 Azure Web 应用的资源 ID，请执行 Get-AzureRmWebApp cmdlet，如以下屏幕截图中所示：

![Alt "通过 PowerShell 获取资源 ID"](./media/monitoring-rest-api-walkthrough/resourceid_powershell.png)

### <a name="azure-cli"></a>Azure CLI
若要使用 Azure CLI 检索某个 Azure 存储帐户的资源 ID，请执行“az storage account show”命令，如以下示例中所示：

```
az storage account show -g azmon-rest-api-walkthrough -n contosotweets2017
```

结果应类似于以下示例：
```JSON
{
  "accessTier": null,
  "creationTime": "2017-08-18T19:58:41.840552+00:00",
  "customDomain": null,
  "enableHttpsTrafficOnly": false,
  "encryption": null,
  "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/storageAccounts/contosotweets2017",
  "identity": null,
  "kind": "Storage",
  "lastGeoFailoverTime": null,
  "location": "centralus",
  "name": "contosotweets2017",
  "networkAcls": null,
  "primaryEndpoints": {
    "blob": "https://contosotweets2017.blob.core.windows.net/",
    "file": "https://contosotweets2017.file.core.windows.net/",
    "queue": "https://contosotweets2017.queue.core.windows.net/",
    "table": "https://contosotweets2017.table.core.windows.net/"
  },
  "primaryLocation": "centralus",
  "provisioningState": "Succeeded",
  "resourceGroup": "azmon-rest-api-walkthrough",
  "secondaryEndpoints": null,
  "secondaryLocation": "eastus2",
  "sku": {
    "name": "Standard_GRS",
    "tier": "Standard"
  },
  "statusOfPrimary": "available",
  "statusOfSecondary": "available",
  "tags": {},
  "type": "Microsoft.Storage/storageAccounts"
}
```


## <a name="retrieve-activity-log-data"></a>检索活动日志数据
除了指标定义和相关值以外，还可以使用 Azure Monitor REST API 检索有关 Azure 资源的其他需关注的深入信息。 例如，可查询[活动日志](https://msdn.microsoft.com/library/azure/dn931934.aspx)数据。 以下示例演示如何使用 Azure 监视器 REST API 查询 Azure 订阅在特定日期范围内的活动日志数据：

```PowerShell
$apiVersion = "2015-04-01"
$filter = "eventTimestamp ge '2017-08-18' and eventTimestamp le '2017-08-19'and eventChannels eq 'Admin, Operation'"
$request = "https://management.azure.cn/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/providers/microsoft.insights/eventtypes/management/values?api-version=${apiVersion}&$filter=${filter}"
Invoke-RestMethod -Uri $request `
    -Headers $authHeader `
    -Method Get `
    -Verbose
```

## <a name="next-steps"></a>后续步骤
* 查看[监视概述](monitoring-overview.md)。
* 查看 [Azure 监视器支持的指标](monitoring-supported-metrics.md)。
* 查看 [Microsoft Azure 监视器 REST API 参考](https://msdn.microsoft.com/library/azure/dn931943.aspx)。
* 查看 [Azure 管理库](https://msdn.microsoft.com/library/azure/mt417623.aspx)。
