---
title: "Azure 资源提供程序和资源类型 | Azure"
description: "介绍支持 Resource Manager 的资源提供程序及其架构和可用 API 版本，以及可托管资源的区域。"
services: azure-resource-manager
documentationcenter: na
author: rockboyfor
manager: digimobile
editor: tysonn
ms.assetid: 3c7a6fe4-371a-40da-9ebe-b574f583305b
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 07/25/2017
ms.date: 09/04/2017
ms.author: v-yeche
ms.openlocfilehash: bbd3cfb35d4dde22e28ca1d89af8cff36b2857a1
ms.sourcegitcommit: 20f589947fbfbe791debd71674f3e4649762b70d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2017
---
# <a name="resource-providers-and-types"></a>资源提供程序和类型

部署资源时，经常需要检索有关资源提供程序和类型的信息。 本文介绍如何执行以下操作：

* 查看 Azure 中的所有资源提供程序
* 检查资源提供程序的注册状态
* 注册资源提供程序
* 查看资源提供程序的资源类型
* 查看资源类型的有效位置
* 查看资源类型的有效 API 版本

可以通过门户、PowerShell 或 Azure CLI 执行这些步骤。

## <a name="powershell"></a>PowerShell

若要查看 Azure 中的所有资源提供程序和订阅的注册状态，请使用：

```powershell
Get-AzureRmResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState
```

这会返回类似于下面的结果：

```powershell
ProviderNamespace                RegistrationState
-------------------------------- ------------------
Microsoft.ClassicCompute         Registered
Microsoft.ClassicNetwork         Registered
Microsoft.ClassicStorage         Registered
Microsoft.CognitiveServices      Registered
...
```

通过注册资源提供程序，将订阅配置为使用资源提供程序。 注册的作用域始终是订阅。 默认情况下，将自动注册许多资源提供程序。 但可能需要手动注册某些资源提供程序。 若要注册资源提供程序，必须具备为资源提供程序执行 `/register/action` 操作的权限。 此操作包含在“参与者”和“所有者”角色中。

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
```

这会返回类似于下面的结果：

```powershell
ProviderNamespace : Microsoft.Batch
RegistrationState : Registering
ResourceTypes     : {batchAccounts, operations, locations, locations/quotas}
Locations         : {China North, China East, China East 2, China North...}
```

当订阅中仍有某个资源提供程序的资源类型时，不能注销该资源提供程序。

若要查看特定资源提供程序的信息，请使用：

```powershell
Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
```

这会返回类似于下面的结果：

```powershell
{ProviderNamespace : Microsoft.Batch
RegistrationState : Registered
ResourceTypes     : {batchAccounts}
Locations         : {China North, China East, China East 2, China North...}

...
```

若要查看资源提供程序的资源类型，请使用：

```powershell
(Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes.ResourceTypeName
```

返回：

```powershell
batchAccounts
operations
locations
locations/quotas
```

API 版本对应于资源提供程序发布的 REST API 操作版本。 资源提供程序启用新功能时，会发布 REST API 的新版本。 

若要获取资源类型的可用 API 版本，请使用：

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes | Where-Object ResourceTypeName -eq batchAccounts).ApiVersions
```

返回：

```powershell
2017-05-01
2017-01-01
2015-12-01
2015-09-01
2015-07-01
```

所有区域都支持 Resource Manager，但部署的资源可能无法在所有区域中受到支持。 此外，订阅可能存在一些限制，以防止用户使用某些支持该资源的区域。 

若要获取资源类型支持的位置，请使用：

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes | Where-Object ResourceTypeName -eq batchAccounts).Locations
```

返回：

```powershell
China North
China East
```

## <a name="azure-cli"></a>Azure CLI
若要查看 Azure 中的所有资源提供程序和订阅的注册状态，请使用：

```azurecli
az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
```

这会返回类似于下面的结果：

```azurecli
Provider                         Status
-------------------------------- ----------------
Microsoft.ClassicCompute         Registered
Microsoft.ClassicNetwork         Registered
Microsoft.ClassicStorage         Registered
Microsoft.CognitiveServices      Registered
...
```

通过注册资源提供程序，将订阅配置为使用资源提供程序。 注册的作用域始终是订阅。 默认情况下，将自动注册许多资源提供程序。 但可能需要手动注册某些资源提供程序。 若要注册资源提供程序，必须具备为资源提供程序执行 `/register/action` 操作的权限。 此操作包含在“参与者”和“所有者”角色中。

```azurecli
az provider register --namespace Microsoft.Batch
```

这会返回一条消息，指出注册正在进行。

当订阅中仍有某个资源提供程序的资源类型时，不能注销该资源提供程序。

若要查看特定资源提供程序的信息，请使用：

```azurecli
az provider show --namespace Microsoft.Batch
```

这会返回类似于下面的结果：

```azurecli
{
    "id": "/subscriptions/####-####/providers/Microsoft.Batch",
    "namespace": "Microsoft.Batch",
    "registrationsState": "Registering",
    "resourceTypes:" [
        ...
    ]
}
```

若要查看资源提供程序的资源类型，请使用：

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[*].resourceType" --out table
```

返回：

```azurecli
Result
---------------
batchAccounts
operations
locations
locations/quotas
```

API 版本对应于资源提供程序发布的 REST API 操作版本。 资源提供程序启用新功能时，会发布 REST API 的新版本。 

若要获取资源类型的可用 API 版本，请使用：

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[?resourceType=='batchAccounts'].apiVersions | [0]" --out table
```

返回：

```azurecli
Result
---------------
2017-05-01
2017-01-01
2015-12-01
2015-09-01
2015-07-01
```

所有区域都支持 Resource Manager，但部署的资源可能无法在所有区域中受到支持。 此外，订阅可能存在一些限制，以防止用户使用某些支持该资源的区域。 

若要获取资源类型支持的位置，请使用：

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[?resourceType=='batchAccounts'].locations | [0]" --out table
```

返回：

```azurecli
Result
---------------
China North
China East
```

## <a name="portal"></a>门户

若要查看 Azure 中的所有资源提供程序以及订阅的注册状态，请选择“订阅”。

![选择订阅](./media/resource-manager-supported-services/select-subscriptions.png)

选择要查看的订阅。

![指定订阅](./media/resource-manager-supported-services/subscription.png)

选择“资源提供程序”并查看可用的资源提供程序列表。

![显示资源提供程序](./media/resource-manager-supported-services/show-resource-providers.png)

通过注册资源提供程序，将订阅配置为使用资源提供程序。 注册的作用域始终是订阅。 默认情况下，将自动注册许多资源提供程序。 但可能需要手动注册某些资源提供程序。 若要注册资源提供程序，必须具备为资源提供程序执行 `/register/action` 操作的权限。 此操作包含在“参与者”和“所有者”角色中。 若要注册资源提供程序，请选择“注册”。

![注册资源提供程序](./media/resource-manager-supported-services/register-provider.png)

当订阅中仍有某个资源提供程序的资源类型时，不能注销该资源提供程序。

若要查看特定资源提供程序的信息，请选择“更多服务”。

![选择更多服务](./media/resource-manager-supported-services/more-services.png)

搜索“资源浏览器”，然后从可用选项中选择它。

![选择资源浏览器](./media/resource-manager-supported-services/select-resource-explorer.png)

选择“提供程序”。

![选择提供程序](./media/resource-manager-supported-services/select-providers.png)

选择要查看的资源提供程序和资源类型。

![选择资源类型](./media/resource-manager-supported-services/select-resource-type.png)

所有区域都支持 Resource Manager，但部署的资源可能无法在所有区域中受到支持。 此外，订阅可能存在一些限制，以防止用户使用某些支持该资源的区域。 资源浏览器将显示该资源类型的有效位置。

![显示位置](./media/resource-manager-supported-services/show-locations.png)

API 版本对应于资源提供程序发布的 REST API 操作版本。 资源提供程序启用新功能时，会发布 REST API 的新版本。 资源浏览器将显示该资源类型的有效 API 版本。

![显示 API 版本](./media/resource-manager-supported-services/show-api-versions.png)

## <a name="next-steps"></a>后续步骤
* 若要了解如何创建 Resource Manager 模板，请参阅[创作 Azure Resource Manager 模板](resource-group-authoring-templates.md)。
* 若要了解如何部署资源，请参阅[使用 Azure Resource Manager 模板部署应用程序](resource-group-template-deploy.md)。
* 若要查看资源提供程序的操作，请参阅 [Azure REST API](https://docs.microsoft.com/rest/api/)。

<!--Update_Description: update meta properties, wording update -->