---
title: 使用 Azure PowerShell 管理基于角色的访问控制 (RBAC) | Microsoft Docs
description: 如何使用 Azure PowerShell 管理 RBAC，包括列出角色、分配角色和删除角色分配。
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 9e225dba-9044-4b13-b573-2f30d77925a9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
origin.date: 04/13/2018
ms.date: 05/02/2018
ms.author: v-junlch
ms.reviewer: rqureshi
ms.openlocfilehash: 50df21f5e480dbc542f06ffa7fe88bc41aea5c50
ms.sourcegitcommit: 17369f8efdf3ec80c2448412e3425ee10042a31a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/03/2018
---
# <a name="manage-role-based-access-control-with-azure-powershell"></a>使用 Azure PowerShell 管理基于角色的访问控制
> [!div class="op_single_selector"]
> * [PowerShell](role-assignments-powershell.md)
> * [Azure CLI](role-assignments-cli.md)
> * [REST API](role-assignments-rest.md)

使用基于角色的访问控制 (RBAC) 时，可以通过分配特定范围的角色，为用户、组和服务主体定义访问权限。 本文介绍如何使用 Azure PowerShell 管理访问权限。

在使用 PowerShell 管理 RBAC 之前，需具备以下先决条件：

- Azure PowerShell 0.8.8 版或更高版本。 若要安装最新版本并将其与 Azure 订阅相关联，请参阅[如何安装和配置 Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)。
- Azure Resource Manager cmdlets。 在 PowerShell 中安装 [Azure Resource Manager cmdlets](https://docs.microsoft.com/powershell/azure/overview) 。

## <a name="list-roles"></a>列出角色
### <a name="list-all-available-roles"></a>列出所有可用的角色
若要列出可以进行分配的 RBAC 角色，并检查其授权访问的操作，请使用 `Get-AzureRmRoleDefinition`。

```powershell
Get-AzureRmRoleDefinition | FT Name, Description
```

![RBAC PowerShell - Get-AzureRmRoleDefinition - 屏幕截图](./media/role-assignments-powershell/1-get-azure-rm-role-definition1.png)

### <a name="list-actions-of-a-role"></a>列出角色的操作
若要列出特定角色的操作，请使用 `Get-AzureRmRoleDefinition <role name>`。

```powershell
Get-AzureRmRoleDefinition Contributor | FL Actions, NotActions

(Get-AzureRmRoleDefinition "Virtual Machine Contributor").Actions
```

![RBAC PowerShell - Get-AzureRmRoleDefinition for a specific role - 屏幕截图](./media/role-assignments-powershell/1-get-azure-rm-role-definition2.png)

## <a name="see-who-has-access"></a>查看谁具有访问权限
若要列出 RBAC 访问权限分配，请使用 `Get-AzureRmRoleAssignment`。

### <a name="list-role-assignments-at-a-specific-scope"></a>列举特定范围内的角色分配
可以查看指定订阅、资源组或资源的所有访问权限分配。 例如，若要查看资源组的所有活动分配，请使用 `Get-AzureRmRoleAssignment -ResourceGroupName <resource group name>`。

```powershell
Get-AzureRmRoleAssignment -ResourceGroupName Pharma-Sales-ProjectForcast | FL DisplayName, RoleDefinitionName, Scope
```

![RBAC PowerShell - Get-AzureRmRoleAssignment for a resource group - 屏幕截图](./media/role-assignments-powershell/4-get-azure-rm-role-assignment1.png)

### <a name="list-roles-assigned-to-a-user"></a>列举分配到用户的角色
若要列出分配给特定用户的所有角色以及分配给用户所属组的角色，请使用 `Get-AzureRmRoleAssignment -SignInName <User email> -ExpandPrincipalGroups`。

```powershell
Get-AzureRmRoleAssignment -SignInName sameert@aaddemo.com | FL DisplayName, RoleDefinitionName, Scope

Get-AzureRmRoleAssignment -SignInName sameert@aaddemo.com -ExpandPrincipalGroups | FL DisplayName, RoleDefinitionName, Scope
```

![RBAC PowerShell - Get-AzureRmRoleAssignment for a user - 屏幕截图](./media/role-assignments-powershell/4-get-azure-rm-role-assignment2.png)

### <a name="list-classic-service-administrator-and-coadmin-role-assignments"></a>列出经典服务管理员和共同管理员角色分配
若要列出经典订阅管理员和共同管理员的访问权限分配，请使用：

```powershell
Get-AzureRmRoleAssignment -IncludeClassicAdministrators
```

## <a name="grant-access"></a>授予访问权限
### <a name="search-for-object-ids"></a>搜索对象 ID
若要分配角色，需要标识对象（用户、组或应用程序）和范围。

如果不知道订阅 ID，可以在 Azure 门户的“订阅”  边栏选项卡中查找。 若要了解如何通过查询获取订阅 ID，请参阅 MSDN 上的 [Get-AzureSubscription](https://docs.microsoft.com/powershell/module/azure/get-azuresubscription?view=azuresmps-3.7.0) 。

若要获取 Azure AD 组的对象 ID，请使用：

```powershell
Get-AzureRmADGroup -SearchString <group name in quotes>
```

若要获取 Azure AD 服务主体或应用程序的对象 ID，请使用：

```powershell
Get-AzureRmADServicePrincipal -SearchString <service name in quotes>
```

### <a name="assign-a-role-to-an-application-at-the-subscription-scope"></a>将角色分配给订阅范围内的应用程序
若要授权访问订阅范围内的应用程序，请使用：

```powershell
New-AzureRmRoleAssignment -ObjectId <application id> -RoleDefinitionName <role name> -Scope <subscription id>
```

![RBAC PowerShell - New-AzureRmRoleAssignment - 屏幕截图](./media/role-assignments-powershell/2-new-azure-rm-role-assignment2.png)

### <a name="assign-a-role-to-a-user-at-the-resource-group-scope"></a>将角色分配给资源组范围内的用户
若要授权访问资源组范围内的用户，请使用：

```powershell
New-AzureRmRoleAssignment -SignInName <email of user> -RoleDefinitionName <role name in quotes> -ResourceGroupName <resource group name>
```

![RBAC PowerShell - New-AzureRmRoleAssignment - 屏幕截图](./media/role-assignments-powershell/2-new-azure-rm-role-assignment3.png)

### <a name="assign-a-role-to-a-group-at-the-resource-scope"></a>将角色分配给资源范围内的组
若要授权访问资源范围内的组，请使用：

```powershell
New-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name in quotes> -ResourceName <resource name> -ResourceType <resource type> -ParentResource <parent resource> -ResourceGroupName <resource group name>
```

![RBAC PowerShell - New-AzureRmRoleAssignment - 屏幕截图](./media/role-assignments-powershell/2-new-azure-rm-role-assignment4.png)

## <a name="remove-access"></a>删除访问权限
若要删除用户、组和应用程序的访问权限，请使用：

```powershell
Remove-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name> -Scope <scope such as subscription id>
```

![RBAC PowerShell - Remove-AzureRmRoleAssignment - 屏幕截图](./media/role-assignments-powershell/3-remove-azure-rm-role-assignment.png)

## <a name="create-a-custom-role"></a>创建自定义角色
若要创建自定义角色，请使用 ```New-AzureRmRoleDefinition``` 命令。 可以通过两种方法来构造角色，即使用 PSRoleDefinitionObject 或 JSON 模板。 

### <a name="get-operations-for-a-resource-provider"></a>获取资源提供程序的操作
创建自定义角色时，请务必了解资源提供程序的所有可能操作。
可以查看[资源提供程序操作](resource-provider-operations.md)的列表，也可以使用 ```Get-AzureRMProviderOperation``` 命令获取该信息。
例如，如果想要查看虚拟机的所有可用操作，请使用此命令：

```powershell
Get-AzureRMProviderOperation "Microsoft.Compute/virtualMachines/*" | FT OperationName, Operation , Description -AutoSize
```

### <a name="create-a-role-with-psroledefinitionobject"></a>使用 PSRoleDefinitionObject 创建角色

使用 PowerShell 创建自定义角色时，可以使用某个[内置角色](built-in-roles.md)作为起点，也可以从头开始。 本部分中的第一个示例以内置角色开始，并为它自定义更多的权限。 编辑属性，添加所需的 *Actions*、*NotActions* 或 *AssignableScopes*，然后将所做的更改另存为新角色。

以下示例从[虚拟机参与者](built-in-roles.md#virtual-machine-contributor)内置角色开始，使用该角色创建名为“虚拟机操作员”的自定义角色。 该新角色授权访问 Microsoft.Compute、Microsoft.Storage 和 Microsoft.Network 资源提供程序的所有读取操作，并授权访问启动、重新启动和监视操作。 该自定义角色可以在两个订阅中使用。

```powershell
$role = Get-AzureRmRoleDefinition "Virtual Machine Contributor"
$role.Id = $null
$role.Name = "Virtual Machine Operator"
$role.Description = "Can monitor and restart virtual machines."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Storage/*/read")
$role.Actions.Add("Microsoft.Network/*/read")
$role.Actions.Add("Microsoft.Compute/*/read")
$role.Actions.Add("Microsoft.Compute/virtualMachines/start/action")
$role.Actions.Add("Microsoft.Compute/virtualMachines/restart/action")
$role.Actions.Add("Microsoft.Authorization/*/read")
$role.Actions.Add("Microsoft.Resources/subscriptions/resourceGroups/read")
$role.Actions.Add("Microsoft.Insights/alertRules/*")
$role.Actions.Add("Microsoft.Support/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e")
$role.AssignableScopes.Add("/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624")
New-AzureRmRoleDefinition -Role $role
```

以下示例显示创建“虚拟机操作员”自定义角色的另一种方式。 通过创建新的 PSRoleDefinition 对象开始。 在 `perms` 变量中指定操作，然后将操作设置为 *Actions* 属性。 通过从[虚拟机参与者](built-in-roles.md#virtual-machine-contributor)内置角色读取 *NotActions* 设置 *NotActions* 属性。 由于[虚拟机参与者](built-in-roles.md#virtual-machine-contributor)没有任何 *NotActions*，因此不需要此行，但它显示了从另一个角色检索信息的方式。

```powershell
$role = [Microsoft.Azure.Commands.Resources.Models.Authorization.PSRoleDefinition]::new()
$role.Name = 'Virtual Machine Operator 2'
$role.Description = 'Can monitor and restart virtual machines.'
$role.IsCustom = $true
$perms = 'Microsoft.Storage/*/read','Microsoft.Network/*/read','Microsoft.Compute/*/read'
$perms += 'Microsoft.Compute/virtualMachines/start/action','Microsoft.Compute/virtualMachines/restart/action'
$perms += 'Microsoft.Authorization/*/read','Microsoft.Resources/subscriptions/resourceGroups/read'
$perms += 'Microsoft.Insights/alertRules/*','Microsoft.Support/*'
$role.Actions = $perms
$role.NotActions = (Get-AzureRmRoleDefinition -Name 'Virtual Machine Contributor').NotActions
$subs = '/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e','/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624'
$role.AssignableScopes = $subs
New-AzureRmRoleDefinition -Role $role
```

### <a name="create-role-with-json-template"></a>使用 JSON 模板创建角色
JSON 模板可以用作自定义角色的源定义。 以下示例创建一个可以对存储和计算资源进行读取访问以及获取支持的自定义角色，并将该角色添加到两个订阅。 创建包含以下示例的新文件 `C:\CustomRoles\customrole1.json`。 创建初始角色时，应将 ID 设置为 `null`，因为会自动生成新的 ID。 

```json
{
  "Name": "Custom Role 1",
  "Id": null,
  "IsCustom": true,
  "Description": "Allows for read access to Azure storage and compute resources and access to support",
  "Actions": [
    "Microsoft.Compute/*/read",
    "Microsoft.Storage/*/read",
    "Microsoft.Support/*"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624"
  ]
}
```

要将角色添加到订阅，请运行以下 PowerShell 命令：

```powershell
New-AzureRmRoleDefinition -InputFile "C:\CustomRoles\customrole1.json"
```

## <a name="modify-a-custom-role"></a>修改自定义角色
可以使用 PSRoleDefinitionObject 或 JSON 模板修改现有的自定义角色，这与创建自定义角色类似。

### <a name="modify-role-with-psroledefinitionobject"></a>使用 PSRoleDefinitionObject 修改角色
若要修改自定义角色，首先，请使用 `Get-AzureRmRoleDefinition` 命令检索角色定义。 然后，对角色定义做出所需更改。 最后，使用 `Set-AzureRmRoleDefinition` 命令保存修改后的角色定义。

以下示例将 `Microsoft.Insights/diagnosticSettings/*` 操作添加到*虚拟机操作员*自定义角色。

```powershell
$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.Actions.Add("Microsoft.Insights/diagnosticSettings/*")
Set-AzureRmRoleDefinition -Role $role
```

![RBAC PowerShell - Set-AzureRmRoleDefinition - 屏幕截图](./media/role-assignments-powershell/3-set-azurermroledefinition-1.png)

以下示例将 Azure 订阅添加到*虚拟机操作员*自定义角色的可分配范围。

```powershell
Get-AzureRmSubscription - SubscriptionName Production3

$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.AssignableScopes.Add("/subscriptions/34370e90-ac4a-4bf9-821f-85eeedead1a2")
Set-AzureRmRoleDefinition -Role $role
```

![RBAC PowerShell - Set-AzureRmRoleDefinition - 屏幕截图](./media/role-assignments-powershell/3-set-azurermroledefinition-2.png)

### <a name="modify-role-with-json-template"></a>使用 JSON 模板修改角色
使用以前的 JSON 模板可以轻松修改现有的自定义角色，以便添加或删除 Actions。 更新 JSON 模板，为网络添加读取操作，如以下示例所示。 模板中列出的定义不是以累积方式应用到现有定义的，这意味着角色的显示方式完全符合模板中的指定。 还需使用角色的 ID 更新“ID”字段。 如果不确定该值是什么，可以使用 `Get-AzureRmRoleDefinition` cmdlet 获取相关信息。

```json
{
  "Name": "Custom Role 1",
  "Id": "acce7ded-2559-449d-bcd5-e9604e50bad1",
  "IsCustom": true,
  "Description": "Allows for read access to Azure storage and compute resources and access to support",
  "Actions": [
    "Microsoft.Compute/*/read",
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Support/*"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624"
  ]
}
```

若要更新现有角色，请运行以下 PowerShell 命令：

```powershell
Set-AzureRmRoleDefinition -InputFile "C:\CustomRoles\customrole1.json"
```

## <a name="delete-a-custom-role"></a>删除自定义角色
若要删除自定义角色，请使用 `Remove-AzureRmRoleDefinition` 命令。

以下示例删除了 *虚拟机操作员* 自定义角色。

```powershell
Get-AzureRmRoleDefinition "Virtual Machine Operator"

Get-AzureRmRoleDefinition "Virtual Machine Operator" | Remove-AzureRmRoleDefinition
```

![RBAC PowerShell - Remove-AzureRmRoleDefinition - 屏幕截图](./media/role-assignments-powershell/4-remove-azurermroledefinition.png)

## <a name="list-custom-roles"></a>列出自定义角色
若要列出可在某范围内进行分配的角色，请使用 `Get-AzureRmRoleDefinition` 命令。

以下示例列出了可在所选订阅中进行分配的所有角色。

```powershell
Get-AzureRmRoleDefinition | FT Name, IsCustom
```

![RBAC PowerShell - Get-AzureRmRoleDefinition - 屏幕截图](./media/role-assignments-powershell/5-get-azurermroledefinition-1.png)

在下面的示例中，虚拟机操作员自定义角色在 Production4 订阅中不可用，因为该订阅不在角色的 AssignableScopes 中。

![RBAC PowerShell - Get-AzureRmRoleDefinition - 屏幕截图](./media/role-assignments-powershell/5-get-azurermroledefinition2.png)

## <a name="see-also"></a>另请参阅
- [将 Azure PowerShell 与 Azure 资源管理器配合使用](../azure-resource-manager/powershell-azure-resource-manager.md)
  [!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]


