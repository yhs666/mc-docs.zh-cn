---
title: "排查常见的 Azure 部署错误 | Azure"
description: "说明如何解决使用 Azure Resource Manager 将资源部署到 Azure 时的常见错误。"
services: azure-resource-manager
documentationcenter: 
tags: top-support-issue
author: rockboyfor
manager: digimobile
editor: tysonn
keywords: "部署错误, azure 部署, 部署到 azure"
ms.assetid: c002a9be-4de5-4963-bd14-b54aa3d8fa59
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: support-article
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 08/17/2017
ms.date: 09/04/2017
ms.author: v-yeche
ms.openlocfilehash: fe0e5dc715c7d097d335c4dbd56745d311c98a27
ms.sourcegitcommit: 20f589947fbfbe791debd71674f3e4649762b70d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2017
---
# <a name="troubleshoot-common-azure-deployment-errors-with-azure-resource-manager"></a>排查使用 Azure Resource Manager 时的常见 Azure 部署错误
本主题介绍如何解决可能遇到的一些常见 Azure 部署错误。

本主题介绍以下错误代码：

* [AccountNameInvalid](#accountnameinvalid)
* [授权失败](#authorization-failed)
* [BadRequest](#badrequest)
* [DeploymentFailed](#deploymentfailed)
* [DisallowedOperation](#disallowedoperation)
* [InvalidContentLink](#invalidcontentlink)
* [InvalidTemplate](#invalidtemplate)
* [MissingSubscriptionRegistration](#noregisteredproviderfound)
* [NotFound](#notfound)
* [NoRegisteredProviderFound](#noregisteredproviderfound)
* [OperationNotAllowed](#quotaexceeded)
* [ParentResourceNotFound](#parentresourcenotfound)
* [QuotaExceeded](#quotaexceeded)
* [RequestDisallowedByPolicy](#requestdisallowedbypolicy)
* [ResourceNotFound](#notfound)
* [SkuNotAvailable](#skunotavailable)
* [StorageAccountAlreadyExists](#storagenamenotunique)
* [StorageAccountAlreadyTaken](#storagenamenotunique)

## <a name="deploymentfailed"></a> DeploymentFailed

此错误代码指示常规部署错误，但它不是开始进行故障排除所需的错误代码。 真正能够帮助解决问题的错误代码通常是该错误的下一级错误代码。 例如，下图显示了部署错误下的 **RequestDisallowedByPolicy** 错误代码。

![显示错误代码](./media/resource-manager-common-deployment-errors/error-code.png)

## <a name="skunotavailable"></a>SkuNotAvailable

在部署资源（通常为虚拟机）时，可能会收到以下错误代码和错误消息：

```
Code: SkuNotAvailable
Message: The requested tier for resource '<resource>' is currently not available in location '<location>' 
for subscription '<subscriptionID>'. Please try another tier or deploy to a different location.
```

当所选的资源 SKU（如 VM 大小）不可用于所选的位置时，会收到此错误。 若要解决此问题，需要确定区域提供哪些 SKU。 可使用 PowerShell、门户或 REST 操作查找可用的 SKU。

- 对于 PowerShell，请使用 [Get-AzureRmComputeResourceSku](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermcomputeresourcesku) 并按位置进行筛选。 必须安装最新版本 PowerShell 才能运行此命令。

  ```powershell
  Get-AzureRmComputeResourceSku | where {$_.Locations.Contains("chinaeast")}
  ```

  结果包括位置的 SKU 列表以及针对该 SKU 的任何限制。

  ```powershell
  ResourceType                Name      Locations Restriction                      Capability Value
  ------------                ----      --------- -----------                      ---------- -----
  availabilitySets         Classic chinaeast             MaximumPlatformFaultDomainCount     3
  availabilitySets         Aligned chinaeast             MaximumPlatformFaultDomainCount     3
  virtualMachines      Standard_A0 chinaeast
  virtualMachines      Standard_A1 chinaeast
  virtualMachines      Standard_A2 chinaeast
  ```

- 如果要使用 [门户](https://portal.azure.cn)，请登录到门户，并通过界面添加资源。 设置值时，可看到该资源的可用 SKU。 不需要完成部署。

    ![可用的 SKU](./media/resource-manager-common-deployment-errors/view-sku.png)

- 若要对虚拟机使用 REST API，请发送以下请求：

    ```HTTP 
    GET
    https://management.chinacloudapi.cn/subscriptions/{subscription-id}/providers/Microsoft.Compute/skus?api-version=2016-03-30
    ```

    它会用以下格式返回可用的 SKU 和区域：

    ```json
    {
        "value": [
            {
                "resourceType": "virtualMachines",
                "name": "Standard_A0",
                "tier": "Standard",
                "size": "A0",
                "locations": [
                    "chinaeast"
                ],
                "restrictions": []
            },
            {
                "resourceType": "virtualMachines",
                "name": "Standard_A1",
                "tier": "Standard",
                "size": "A1",
                "locations": [
                    "chinaeast"
                ],
                "restrictions": []
            },
            ...
        ]
    }    
    ```

如果在该区域或满足业务需求的备用区域中找不到合适的 SKU，请将 [SKU 请求](https://www.azure.cn/support/support-ticket-form)提交到 Azure 支持。

## <a name="disallowedoperation"></a> DisallowedOperation

```
Code: DisallowedOperation
Message: The current subscription type is not permitted to perform operations on any provider 
namespace. Please use a different subscription.
```

如果收到此错误，则指示你使用的是不允许访问 Azure Active Directory 之外的任何 Azure 服务的订阅。 当需要访问经典管理门户，但不允许部署资源时，可以使用此类型的订阅。 若要解决此问题，必须使用有权部署资源的订阅。  

若要使用 PowerShell 查看可用订阅，请使用：

```powershell
Get-AzureRmSubscription
```

而若要设置当前订阅，请使用：

```powershell
Set-AzureRmContext -SubscriptionName {subscription-name}
```

若要使用 Azure CLI 2.0 查看可用订阅，请使用：

```azurecli
az account list
```

而若要设置当前订阅，请使用：

```azurecli
az account set --subscription {subscription-name}
```

## <a name="invalidtemplate"></a>InvalidTemplate
此错误可由多种不同类型的错误导致。

- 语法错误

    如果有错误消息指出模板验证失败，则可能是模板中存在语法问题。

    ```
    Code=InvalidTemplate
    Message=Deployment template validation failed
    ```

    此错误很容易发生，因为模板表达式可能很复杂。 例如，存储帐户的以下名称分配包含一组方括号、三个函数、三组圆括号、一组单引号和一个属性：

    ```json
    "name": "[concat('storage', uniqueString(resourceGroup().id))]",
    ```

    如果未提供匹配的语法，该模板会生成一个意外的值。

    收到此类错误时，请仔细检查表达式语法。 考虑使用 [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) 或 [Visual Studio Code](resource-manager-vs-code.md) 等 JSON 编辑器，此类编辑器在出现语法错误时可以发出警告。

- 段长度不正确

    当资源名称的格式不正确时，会发生另一种模板无效错误。

    ```
    Code=InvalidTemplate
    Message=Deployment template validation failed: 'The template resource {resource-name}'
    for type {resource-type} has incorrect segment lengths.
    ```

    根级别的资源其名称中的段必须比资源类型中的段少一个。 段之间用斜杠隔开。 在下面的示例中，类型有两个段，名称有一个段，因此为 **有效名称**。

    ```json
    {
      "type": "Microsoft.Web/serverfarms",
      "name": "myHostingPlanName",
      ...
    }
    ```

    但下一个示例 **不是有效名称** ，因为其段数与类型的段数相同。

    ```json
    {
      "type": "Microsoft.Web/serverfarms",
      "name": "appPlan/myHostingPlanName",
      ...
    }
    ```

    对于子资源来说，类型和名称的段数必须相同。 之所以必须这样，是因为子资源的完整名称和类型包含父名称和类型。 因此，完整名称的段仍比完整类型的段少一个。

    ```json
    "resources": [
        {
            "type": "Microsoft.KeyVault/vaults",
            "name": "contosokeyvault",
            ...
            "resources": [
                {
                    "type": "secrets",
                    "name": "appPassword",
                    ...
                }
            ]
        }
    ]
    ```

    确保段数正确对于 Resource Manager 类型来说可能很困难，这些类型应用到各个资源提供程序。 例如，对网站应用资源锁需要使用包含四个段的类型。 因此，该名称包含三个段：

    ```json
    {
        "type": "Microsoft.Web/sites/providers/locks",
        "name": "[concat(variables('siteName'),'/Microsoft.Authorization/MySiteLock')]",
        ...
    }
    ```

- 不应使用 copy 索引

    将 **copy** 元素应用到不支持该元素的模板部分时，会遇到此 **InvalidTemplate** 错误。 只能将 copy 元素应用于资源类型。 不能将 copy 应用于资源类型中的属性。 例如，可以将 copy 应用于虚拟机，但不能将其应用于虚拟机的 OS 磁盘。 在某些情况下，可以将子资源转换为父资源，以创建复制循环。 有关使用 copy 的详细信息，请参阅[在 Azure Resource Manager 中创建多个资源实例](resource-group-create-multiple.md)。

- 参数无效

    如果模板指定了参数的允许值，但你提供的值并不是这些允许值之一，则会收到类似于下面的错误消息：

    ```
    Code=InvalidTemplate;
    Message=Deployment template validation failed: 'The provided value {parameter value}
    for the template parameter {parameter name} is not valid. The parameter value is not
    part of the allowed values
    ```

    请仔细检查模板中的允许值，并提供在部署过程中提供这些值之一。

- 检测到循环依赖项

    当资源以某种方式互相依赖，导致部署无法启动时，就会出现此错误。 将多个相互依赖的项组合在一起时，会导致两个或两个以上的资源等待其他资源，而后者也在进行等待。 例如，resource1 依赖于 resource3，resource2 依赖于 resource1，resource3 依赖于 resource2。 通常情况下，删除不必要的依赖项即可解决此问题。

<a id="notfound"></a>
### <a name="notfound-and-resourcenotfound"></a>NotFound 和 ResourceNotFound
如果模板包含无法解析的资源的名称，将出现类似于下面的错误：

```
Code=NotFound;
Message=Cannot find ServerFarm with name exampleplan.
```

若要部署模板中缺少的资源，请检查是否需要添加依赖关系。 如果可能，Resource Manager 会通过并行创建资源来优化部署。 如果一个资源必须在另一个资源之后部署，则需在模板中使用 **dependsOn** 元素创建与其他资源的依赖关系。 例如，在部署 Web 应用时，应用服务计划必须存在。 如果未指定该 Web 应用与应用服务计划的依赖关系，Resource Manager 将同时创建这两个资源。 此时会出现一条错误消息，指出找不到应用服务计划资源，因为尝试在 Web 应用上设置属性时它尚不存在。 在 Web 应用中设置依赖关系可避免此错误。

```json
{
  "apiVersion": "2015-08-01",
  "type": "Microsoft.Web/sites",
  "dependsOn": [
    "[variables('hostingPlanName')]"
  ],
  ...
}
```

有关依赖项错误的故障诊断建议，请参阅 [检查部署顺序](#check-deployment-sequence)。

如果资源所在的资源组不是要部署到的资源组，也可能会出现此错误。 在这种情况下，请使用 [resourceId函数](resource-group-template-functions-resource.md#resourceid)获取资源的完全限定名称。

```json
"properties": {
    "name": "[parameters('siteName')]",
    "serverFarmId": "[resourceId('plangroup', 'Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
}
```

如果尝试对无法解析的资源使用 [reference](resource-group-template-functions-resource.md#reference) 或 [listKeys](resource-group-template-functions-resource.md#listkeys) 函数，则会收到以下错误：

```
Code=ResourceNotFound;
Message=The Resource 'Microsoft.Storage/storageAccounts/{storage name}' under resource
group {resource group name} was not found.
```

查找包含 **reference** 函数的表达式。 仔细检查参数值是否正确。

## <a name="parentresourcenotfound"></a>ParentResourceNotFound

如果一项资源是另一资源的父级，则父资源必须存在才能创建子资源。 如果它尚不存在，则会收到以下错误：

```
Code=ParentResourceNotFound;
Message=Can not perform requested operation on nested resource. Parent resource 'exampleserver' not found."
```

子资源的名称包括父名称。 例如，SQL 数据库可能定义为：

```json
{
  "type": "Microsoft.Sql/servers/databases",
  "name": "[concat(variables('databaseServerName'), '/', parameters('databaseName'))]",
  ...
```

但是，如果不指定对父资源的依赖关系，则可能在部署父资源之前部署子资源。 为更正此错误，请添加依赖关系。

```json
"dependsOn": [
    "[variables('databaseServerName')]"
]
```

<a name="storagenamenotunique"></a>
## <a name="storageaccountalreadyexists-and-storageaccountalreadytaken"></a> StorageAccountAlreadyExists 和 StorageAccountAlreadyTaken
对于存储帐户，必须提供在 Azure 中唯一的资源名称。 如果不提供唯一名称，会出现类似于下面的错误：

```
Code=StorageAccountAlreadyTaken
Message=The storage account named mystorage is already taken.
```

可将命名约定与 [uniqueString](resource-group-template-functions-string.md#uniquestring) 函数的结果连接起来创建一个唯一名称。

```json
"name": "[concat('storage', uniqueString(resourceGroup().id))]",
"type": "Microsoft.Storage/storageAccounts",
```

如果部署的存储帐户与订阅中某个现有存储帐户的名称相同，但提供的位置不同，会出现一条错误消息，指出不同的位置已存在该存储帐户。 请删除现有存储帐户，或者提供与现有存储帐户相同的位置。

## <a name="accountnameinvalid"></a> AccountNameInvalid
尝试为存储帐户提供一个包含禁止字符的名称时，会出现 **AccountNameInvalid** 错误。 存储帐户名称必须为 3 到 24 个字符，只能使用数字和小写字母。 [uniqueString](resource-group-template-functions-string.md#uniquestring) 函数返回 13 个字符。 如果将前缀连接到 **uniqueString** 结果，请提供 11 个字符（或更少字符）的前缀。

## <a name="badrequest"></a> BadRequest

为属性提供的值无效时，可能遇到 BadRequest 状态。 例如，如果为存储帐户提供错误的 SKU 值，部署将失败。 若要确定属性的有效值，请查看 [REST API](https://docs.microsoft.com/rest/api) 以了解要部署的资源类型。

<a id="noregisteredproviderfound" />

## <a name="noregisteredproviderfound-and-missingsubscriptionregistration"></a>NoRegisteredProviderFound 和 MissingSubscriptionRegistration
部署资源时，可能会收到以下错误代码和消息：

```
Code: NoRegisteredProviderFound
Message: No registered resource provider found for location {location}
and API version {api-version} for type {resource-type}.
```

或者，可能收到类似的消息，指出：

```
Code: MissingSubscriptionRegistration
Message: The subscription is not registered to use namespace {resource-provider-namespace}
```

可能由于下三种原因之一而收到此错误：

1. 尚未为订阅注册资源提供程序
2. 资源类型不支持该 API 版本
3. 资源类型不支持该位置

错误消息应提供有关支持的位置和 API 版本的建议。 可以将模板更改为建议的值之一。 Azure 门户或正在使用的命令行接口会自动注册大多数提供程序；但非全部。 如果以前未使用特定的资源提供程序，则可能需要注册该提供程序。 可以通过 PowerShell 或 Azure CLI 了解有关资源提供程序的详细信息。

**门户**

可以通过门户查看注册状态，并注册资源提供程序命名空间。

1. 对于订阅，选择“资源提供程序”。

    ![选择资源提供程序](./media/resource-manager-common-deployment-errors/select-resource-provider.png)

2. 查看资源提供程序的列表，根据需要选择“注册”链接，注册你尝试部署的类型的资源提供程序。

    ![列出资源提供程序](./media/resource-manager-common-deployment-errors/list-resource-providers.png)

**PowerShell**

若要查看注册状态，请使用 **Get-AzureRmResourceProvider**。

```powershell
Get-AzureRmResourceProvider -ListAvailable
```

若要注册提供程序，请使用 **Register-AzureRmResourceProvider** ，并提供想要注册的资源提供程序的名称。

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Cdn
```

若要获取特定类型的资源支持的位置，请使用：

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
```

若要获取特定类型的资源支持的 API 版本，请使用：

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions
```

**Azure CLI**

若要查看是否已注册提供程序，请使用 `azure provider list` 命令。

```azurecli
az provider list
```

若要注册资源提供程序，请使用 `azure provider register` 命令，并指定要注册的 *命名空间* 。

```azurecli
az provider register --namespace Microsoft.Cdn
```

若要查看资源类型支持的位置和 API 版本，请使用：

```azurecli
az provider show -n Microsoft.Web --query "resourceTypes[?resourceType=='sites'].locations"
```

<a id="quotaexceeded"></a>
## <a name="operationnotallowed"></a>QuotaExceeded 和 OperationNotAllowed
部署超出配额（可能是根据资源组、订阅、帐户和其他范围指定的）时，可能会遇到问题。 例如，订阅可能配置为限制某个区域的核心数目。 如果尝试部署超过允许核心数目的虚拟机，将收到指出超过配额的错误消息。
有关完整的配额信息，请参阅 [Azure 订阅和服务限制、配额与约束](../azure-subscription-service-limits.md)。

若要检查订阅的核心配额，可以使用 Azure CLI 中的 `azure vm list-usage` 命令。 以下示例显示，试用帐户的核心配额为 4 ：

```azurecli
az vm list-usage --location "China East"
```

返回：

```azurecli
[
  {
    "currentValue": 0,
    "limit": 2000,
    "name": {
      "localizedValue": "Availability Sets",
      "value": "availabilitySets"
    }
  },
  ...
]
```

如果在中国北部区域部署一个会创建四个以上的核心的模板，将出现类似如下的部署错误：

```
Code=OperationNotAllowed
Message=Operation results in exceeding quota limits of Core.
Maximum allowed: 4, Current in use: 4, Additional requested: 2.
```

或者，可以在 PowerShell 中使用 **Get-AzureRmVMUsage** cmdlet。

```powershell
Get-AzureRmVMUsage
```

返回：

```powershell
...
CurrentValue : 0
Limit        : 4
Name         : {
                 "value": "cores",
                 "localizedValue": "Total Regional Cores"
               }
Unit         : null
...
```

在这种情况中，应前往门户并提交一份支持问题以增加你在要部署区域内的配额。

> [!NOTE]
> 请记住，对于资源组，配额针对每个单独的区域，而不是针对整个订阅。 如果需要在中国北部部署 30 个核心，则必须在中国北部寻求 30 个 Resource Manager 核心。 如果需要在有权访问的任何区域内部署总共 30 个核心，则应在所有区域内请求总共 30 个 Resource Manager 核心。
>
>

## <a name="invalidcontentlink"></a> InvalidContentLink
如果收到以下错误消息：

```
Code=InvalidContentLink
Message=Unable to download deployment content from ...
```

原因很可能是你尝试链接到一个不可用的嵌套模板。 请仔细检查提供给嵌套模板的 URI。 如果模板在存储帐户中存在，请确保 URI 可访问。 可能需要传递 SAS 令牌。 有关详细信息，请参阅[将链接的模板与 Azure 资源管理器配合使用](resource-group-linked-templates.md)。

## <a name="requestdisallowedbypolicy"></a> RequestDisallowedByPolicy
如果订阅中的某个资源策略会阻止部署期间尝试执行的操作，会出现此错误。 请在错误消息中查找策略标识符。

```
Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'
```

在 **PowerShell**  中，提供该策略标识符作为 **Id** 参数，检索阻止部署的策略的详细信息。

```powershell
(Get-AzureRmPolicyDefinition -Id "/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition").Properties.policyRule | ConvertTo-Json
```

在 **Azure CLI** 中，提供策略定义的名称：

```azurecli
az policy definition show --name regionPolicyAssignment
```

有关详细信息，请参阅以下文章：

<!-- Not Avaialble [RequestDisallowedByPolicy error](resource-manager-policy-requestdisallowedbypolicy-error.md) -->
- [使用策略来管理资源和控制访问](resource-manager-policy.md)

## <a name="authorization-failed"></a>授权失败
可能在部署期间收到错误，因为尝试部署资源的帐户或服务主体没有执行这些操作的访问权限。 Azure Active Directory 可让你或系统管理员非常精确地控制哪些标识可以访问哪些资源。 例如，如果帐户已分配到“读取者”角色，则无法创建资源。 在此情况下，会出现一条错误消息，指出授权失败。

有关基于角色的访问控制的详细信息，请参阅 [Azure Role-Based Access Control](../active-directory/role-based-access-control-configure.md)（Azure 基于角色的访问控制）。

## <a name="next-steps"></a>后续步骤
* 若要了解审核操作，请参阅[使用 Resource Manager 执行审核操作](resource-group-audit.md)。
* 若要了解部署期间为确定错误需要执行哪些操作，请参阅[查看部署操作](resource-manager-deployment-operations.md)。

<!--Update_Description: wording update, add Get-AzureRmComputeResourceSku sample code -->