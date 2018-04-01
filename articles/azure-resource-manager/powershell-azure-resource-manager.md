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
ms.topic: article
origin.date: 02/16/2018
ms.date: 03/26/2018
ms.author: v-yeche
ms.openlocfilehash: 7cbfdc9abe479b2b81e20f6982b393e627594cb0
ms.sourcegitcommit: 6d7f98c83372c978ac4030d3935c9829d6415bf4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="manage-resources-with-azure-powershell"></a>使用 Azure PowerShell 管理资源

[!INCLUDE [Resource Manager governance introduction](../../includes/resource-manager-governance-intro.md)]

<!-- Not Avaiable on [!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)] 00>

If you choose to install and use the PowerShell locally, see [Install Azure PowerShell module](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). If you are running PowerShell locally, you also need to run `Login-AzureRmAccount -EnvironmentName AzureChinaCloud` to create a connection with Azure.

## Understand scope

[!INCLUDE [Resource Manager governance scope](../../includes/resource-manager-governance-scope.md)]

In this article, you apply all management settings to a resource group so you can easily remove those settings when done.

Let's create the resource group.

```azurepowershell-interactive
Set-AzureRmContext -Subscription <subscription-name>
New-AzureRmResourceGroup -Name myResourceGroup -Location ChinaEast
```

Currently, the resource group is empty.

## Role-based access control

[!INCLUDE [Resource Manager governance policy](../../includes/resource-manager-governance-rbac.md)]

### Assign a role

In this article, you deploy a virtual machine and its related virtual network. For managing virtual machine solutions, there are three resource-specific roles that provide commonly needed access:

* [Virtual Machine Contributor](../active-directory/role-based-access-built-in-roles.md#virtual-machine-contributor)
* [Network Contributor](../active-directory/role-based-access-built-in-roles.md#network-contributor)
* [Storage Account Contributor](../active-directory/role-based-access-built-in-roles.md#storage-account-contributor)

Instead of assigning roles to individual users, it's often easier to [create an Azure Active Directory group](../active-directory/active-directory-groups-create-azure-portal.md) for users who need to take similar actions. Then, assign that group to the appropriate role. To simplify this article, you create an Azure Active Directory group without members. You can still assign this group to a role for a scope. 

The following example creates a group and assigns it to the Virtual Machine Contributor role for the resource group. To run the `New-AzureAdGroup` command, you must either use the [Azure Cloud Shell](/cloud-shell/overview) or [download the Azure AD PowerShell module](https://www.powershellgallery.com/packages/AzureAD/).

```azurepowershell-interactive
$adgroup = New-AzureADGroup -DisplayName VMDemoContributors `
  -MailNickName vmDemoGroup `
  -MailEnabled $false `
  -SecurityEnabled $true
New-AzureRmRoleAssignment -ObjectId $adgroup.ObjectId `
  -ResourceGroupName myResourceGroup `
  -RoleDefinitionName "Virtual Machine Contributor"
```

Typically, you repeat the process for **Network Contributor** and **Storage Account Contributor** to make sure users are assigned to manage the deployed resources. In this article, you can skip those steps.

<!-- Not Available on ## Azure policies -->

## <a name="deploy-the-virtual-machine"></a>部署虚拟机

分配角色和策略以后，即可部署解决方案。 默认大小为 Standard_DS1_v2，这是允许的 SKU 之一。 运行此步骤时，会提示输入凭据。 你输入的值将配置为用于虚拟机的用户名和密码。

```azurepowershell-interactive
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

```azurepowershell-interactive
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

```azurepowershell-interactive
$r = Get-AzureRmResource -ResourceName myVM `
  -ResourceGroupName myResourceGroup `
  -ResourceType Microsoft.Compute/virtualMachines
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test"; Project="Documentation" } -ResourceId $r.ResourceId -Force
```

### <a name="find-resources-by-tag"></a>按标记查找资源

若要使用标记名称和值来查找资源，请使用：

```azurepowershell-interactive
(Find-AzureRmResource -TagName Environment -TagValue Test).Name
```

可以将返回的值用于管理任务，例如停止带有某个标记值的所有虚拟机。

```azurepowershell-interactive
Find-AzureRmResource -TagName Environment -TagValue Test | Where-Object {$_.ResourceType -eq "Microsoft.Compute/virtualMachines"} | Stop-AzureRmVM
```

### <a name="view-costs-by-tag-values"></a>按标记值查看成本

对资源应用标记以后，即可使用这些标记查看资源的成本。 成本分析显示最新使用情况需要一定的时间，因此可能还看不到这些成本。 成本可用以后，即可在订阅中跨资源组查看资源的成本。 用户必须具有[计费信息的订阅级别访问权限](../billing/billing-manage-access.md)才能查看这些成本。

若要在门户中按标记查看成本，请先选择订阅，然后选择“成本分析”。

![成本分析](./media/powershell-azure-resource-manager/select-cost-analysis.png)

然后，按标记值进行筛选并选择“应用”。

![按标记查看成本](./media/powershell-azure-resource-manager/view-costs-by-tag.png)

也可使用 [Azure 计费 API](../billing/billing-usage-rate-card-overview.md) 以编程方式查看成本。

## <a name="clean-up-resources"></a>清理资源

在解除锁定之前，不能删除锁定的网络安全组。 若要解除锁定，请使用：

```azurepowershell-interactive
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

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>后续步骤
* 若要了解如何监视虚拟机，请参阅[使用 Azure PowerShell 监视和更新 Windows 虚拟机](../virtual-machines/windows/tutorial-monitoring.md)。
<!-- Not Available on [Monitor virtual machine security by using Azure Security Center](../virtual-machines/windows/tutorial-azure-security.md) -->
* 可以将现有资源移动到新的资源组。 有关示例，请参阅[将资源移动到新的资源组或订阅中](resource-group-move-resources.md)。
* 有关企业可如何使用 Resource Manager 有效管理订阅的指南，请参阅 [Azure 企业基架 - 出于合规目的监管订阅](resource-manager-subscription-governance.md)。

<!--Update_Description: update meta properties, wording update, update link -->
<!--The parent file of includes file of resource-manager-governance-intro.md-->
<!--ms.date:03/26/2018-->
