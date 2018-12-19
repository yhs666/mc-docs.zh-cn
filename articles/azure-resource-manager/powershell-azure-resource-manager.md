---
title: 使用 PowerShell 管理 Azure 解决方案 | Azure
description: 使用 Azure PowerShell 和 Resource Manager 管理资源。
services: azure-resource-manager
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: tysonn
ms.assetid: b33b7303-3330-4af8-8329-c80ac7e9bc7f
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: powershell
ms.devlang: na
ms.topic: conceptual
origin.date: 11/08/2018
ms.date: 12/17/2018
ms.author: v-yeche
ms.openlocfilehash: 4983c4aadfd3f13be4116b8cf9a1197cf267a0ba
ms.sourcegitcommit: 1db6f261786b4f0364f1bfd51fd2db859d0fc224
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2018
ms.locfileid: "53286750"
---
# <a name="manage-resources-with-azure-powershell"></a>使用 Azure PowerShell 管理资源

[!INCLUDE [Resource Manager governance introduction](../../includes/resource-manager-governance-intro.md)]

<!--[!INCLUDE [cloud-shell-powershell](../../../includes/cloud-shell-powershell.md)]-->

如果选择在本地安装并使用 PowerShell，请参阅[安装 Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)。 如果在本地运行 PowerShell，则还需运行 `Connect-AzureRmAccount` 来创建与 Azure 的连接。

## <a name="understand-scope"></a>了解范围

[!INCLUDE [Resource Manager governance scope](../../includes/resource-manager-governance-scope.md)]

在本文中，请将所有管理设置应用到资源组，以便在完成后可以轻松地删除这些设置。

让我们创建该资源组。

```PowerShell
Set-AzureRmContext -Subscription <subscription-name>
New-AzureRmResourceGroup -Name myResourceGroup -Location ChinaEast
```

目前，资源组为空。

## <a name="role-based-access-control"></a>基于角色的访问控制

[!INCLUDE [Resource Manager governance policy](../../includes/resource-manager-governance-rbac.md)]

### <a name="assign-a-role"></a>分配角色

在本文中，请部署一个虚拟机及其相关的虚拟网络。 若要管理虚拟机解决方案，可以使用三种特定于资源的角色来进行通常所需的访问：

* [虚拟机参与者](../role-based-access-control/built-in-roles.md#virtual-machine-contributor)
* [网络参与者](../role-based-access-control/built-in-roles.md#network-contributor)
* [存储帐户参与者](../role-based-access-control/built-in-roles.md#storage-account-contributor)

通常情况下，与其向单个用户分配角色，不如为需要进行相似操作的用户[创建一个 Azure Active Directory 组](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md)， 然后向该组分配相应的角色。 为了简单起见，本文创建一个没有成员的 Azure Active Directory 组。 仍然可以为该组分配一个负责某个范围的角色。

以下示例创建一个组，然后为其分配了资源组的“虚拟机参与者”角色。 若要运行 `New-AzureAdGroup` 命令，必须[下载 Azure AD PowerShell 模块](https://www.powershellgallery.com/packages/AzureAD/)。

<!-- Not Available on [Azure Cloud Shell](/cloud-shell/overview)-->
```PowerShell
$adgroup = New-AzureADGroup -DisplayName VMDemoContributors `
  -MailNickName vmDemoGroup `
  -MailEnabled $false `
  -SecurityEnabled $true
New-AzureRmRoleAssignment -ObjectId $adgroup.ObjectId `
  -ResourceGroupName myResourceGroup `
  -RoleDefinitionName "Virtual Machine Contributor"
```

通常情况下，请对**网络参与者**和**存储帐户参与者**重复执行此过程，确保分配用户来管理已部署的资源。 在本文中，可以跳过这些步骤。

## <a name="azure-policy"></a>Azure Policy

[Azure Policy](../governance/policy/concepts/definition-structure.md) 可帮助确保订阅中的所有资源符合企业标准。 订阅已经有多个策略定义。 若要查看可用的策略定义，请使用：

<!-- URL direct From ../azure-policy/policy-definition.md to ../governance/policy/concepts/definition-structure.md-->
```PowerShell
(Get-AzureRmPolicyDefinition).Properties | Format-Table displayName, policyType
```

可以看到现有的策略定义。 策略类型为“内置”或“自定义”。 在这些定义中查找所述条件正是你要分配的条件的定义。 在本文中，分配的策略要符合以下条件：

* 限制所有资源的位置
* 限制虚拟机的 SKU
* 审核不使用托管磁盘的虚拟机

```PowerShell
$locations ="chinaeast", "chinaeast2"
$skus = "Standard_DS1_v2", "Standard_E2s_v2"

$rg = Get-AzureRmResourceGroup -Name myResourceGroup

$locationDefinition = Get-AzureRmPolicyDefinition | where-object {$_.properties.displayname -eq "Allowed locations"}
$skuDefinition = Get-AzureRmPolicyDefinition | where-object {$_.properties.displayname -eq "Allowed virtual machine SKUs"}
$auditDefinition = Get-AzureRmPolicyDefinition | where-object {$_.properties.displayname -eq "Audit VMs that do not use managed disks"}

New-AzureRMPolicyAssignment -Name "Set permitted locations" `
  -Scope $rg.ResourceId `
  -PolicyDefinition $locationDefinition `
  -listOfAllowedLocations $locations
New-AzureRMPolicyAssignment -Name "Set permitted VM SKUs" `
  -Scope $rg.ResourceId `
  -PolicyDefinition $skuDefinition `
  -listOfAllowedSKUs $skus
New-AzureRMPolicyAssignment -Name "Audit unmanaged disks" `
  -Scope $rg.ResourceId `
  -PolicyDefinition $auditDefinition
```

## <a name="deploy-the-virtual-machine"></a>部署虚拟机

分配角色和策略以后，即可部署解决方案。 默认大小为 Standard_DS1_v2，这是允许的 SKU 之一。 运行此步骤时，会提示输入凭据。 输入的值将配置为用于虚拟机的用户名和密码。

```PowerShell
New-AzureRmVm -ResourceGroupName "myResourceGroup" `
     -Name "myVM" `
     -Location "China East" `
     -VirtualNetworkName "myVnet" `
     -SubnetName "mySubnet" `
     -SecurityGroupName "myNetworkSecurityGroup" `
     -PublicIpAddressName "myPublicIpAddress" `
     -OpenPorts 80,3389
```

部署完成后，可以对解决方案应用更多的管理设置。

## <a name="lock-resources"></a>锁定资源

[!INCLUDE [Resource Manager governance locks](../../includes/resource-manager-governance-locks.md)]

### <a name="lock-a-resource"></a>锁定资源

若要锁定虚拟机和网络安全组，请使用：

```PowerShell
New-AzureRmResourceLock -LockLevel CanNotDelete `
  -LockName LockVM `
  -ResourceName myVM `
  -ResourceType Microsoft.Compute/virtualMachines `
  -ResourceGroupName myResourceGroup
New-AzureRmResourceLock -LockLevel CanNotDelete `
  -LockName LockNSG `
  -ResourceName myNetworkSecurityGroup `
  -ResourceType Microsoft.Network/networkSecurityGroups `
  -ResourceGroupName myResourceGroup
```

只有在明确解除锁定以后，才能删除虚拟机。 该步骤显示在[清理资源](#clean-up-resources)中。

## <a name="tag-resources"></a>标记资源

[!INCLUDE [Resource Manager governance tags](../../includes/resource-manager-governance-tags.md)]

### <a name="tag-resources"></a>标记资源

[!INCLUDE [Resource Manager governance tags Powershell](../../includes/resource-manager-governance-tags-powershell.md)]

若要将标记应用到虚拟机，请使用：

```PowerShell
$r = Get-AzureRmResource -ResourceName myVM `
  -ResourceGroupName myResourceGroup `
  -ResourceType Microsoft.Compute/virtualMachines
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test"; Project="Documentation" } -ResourceId $r.ResourceId -Force
```

### <a name="find-resources-by-tag"></a>按标记查找资源

若要使用标记名称和值来查找资源，请使用：

```PowerShell
(Find-AzureRmResource -TagName Environment -TagValue Test).Name
```

可以将返回的值用于管理任务，例如停止带有某个标记值的所有虚拟机。

```PowerShell
Find-AzureRmResource -TagName Environment -TagValue Test | Where-Object {$_.ResourceType -eq "Microsoft.Compute/virtualMachines"} | Stop-AzureRmVM
```

<!-- Not Available on ### View costs by tag values -->

## <a name="clean-up-resources"></a>清理资源

在解除锁定之前，不能删除锁定的网络安全组。 若要解除锁定，请使用：

```PowerShell
Remove-AzureRmResourceLock -LockName LockVM `
  -ResourceName myVM `
  -ResourceType Microsoft.Compute/virtualMachines `
  -ResourceGroupName myResourceGroup
Remove-AzureRmResourceLock -LockName LockNSG `
  -ResourceName myNetworkSecurityGroup `
  -ResourceType Microsoft.Network/networkSecurityGroups `
  -ResourceGroupName myResourceGroup
```

如果不再需要资源组、VM 和所有相关的资源，可以使用 [Remove-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/remove-azurermresourcegroup) 命令将其删除。

```PowerShell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>后续步骤

* 若要了解如何监视虚拟机，请参阅[使用 Azure PowerShell 监视和更新 Windows 虚拟机](../virtual-machines/windows/tutorial-monitoring.md)。
* 可以将现有资源移动到新的资源组。 有关示例，请参阅[将资源移动到新的资源组或订阅中](resource-group-move-resources.md)。

<!-- Not Available on Line 205 [Monitor virtual machine security by using Azure Security Center](../virtual-machines/windows/tutorial-azure-security.md) -->
<!-- Not Available on [Azure enterprise scaffold - prescriptive subscription governance](https://docs.microsoft.com/azure/architecture/cloud-adoption-guide/subscription-governance)-->

<!--Update_Description: update meta properties, wording update -->

