---
title: Azure PowerShell 脚本示例 - 更新 RDP 用户名和密码 | Azure
description: Azure PowerShell 脚本示例 - 更新特定节点类型的所有 Service Fabric 群集节点的 RDP 用户名和密码。
services: service-fabric
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
origin.date: 03/19/2018
ms.date: 04/30/2018
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: 9128ad81334a8f3b0780ebb11aba10d35f7b3bc8
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52654107"
---
# <a name="update-the-admin-username-and-password-of-the-vms-in-a-cluster"></a>更新群集中 VM 的管理员用户名和密码

Service Fabric 群集中的每个[节点类型](../service-fabric-cluster-nodetypes.md)是一个虚拟机规模集。 此示例脚本更新特定节点类型中群集虚拟机的管理员用户名和密码。  将 VMAccessAgent 扩展添加到规模集，因为管理员密码是不可修改的规模集属性。  用户名和密码更改将应用到规模集中的所有节点。 根据需要自定义参数。

必要时，请使用 [Azure PowerShell 指南](https://docs.microsoft.com/powershell/azure/overview)中的说明安装 Azure PowerShell。 

## <a name="sample-script"></a>示例脚本

```powershell
Login-AzureRmAccount -EnvironmentName AzureChinaCloud
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId 'yourSubscriptionID'

$nodeTypeName = 'nt1vm'
$resourceGroup = 'sfclustertutorialgroup'
$publicConfig = @{'UserName' = 'newuser'}
$privateConfig = @{'Password' = 'PasSwo0rd$#!'}
$extName = 'VMAccessAgent'
$publisher = 'Microsoft.Compute'
$node = Get-AzureRmVmss -ResourceGroupName $resourceGroup -VMScaleSetName $nodeTypeName
$node = Add-AzureRmVmssExtension -VirtualMachineScaleSet $node -Name $extName -Publisher $publisher -Setting $publicConfig -ProtectedSetting $privateConfig -Type $extName -TypeHandlerVersion '2.0' -AutoUpgradeMinorVersion $true

Update-AzureRmVmss -ResourceGroupName $resourceGroup -Name $nodeTypeName -VirtualMachineScaleSet $node
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令：表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [Get-AzureRmVmss](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmss) | 获取群集节点类型（虚拟机规模集）的属性。   |
| [Add-AzureRmVmssExtension](https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvmssextension)| 将扩展添加到虚拟机规模集。|
| [Update-AzureRmVmss](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvmss)|将虚拟机规模集的状态更新为 VMSS 对象的状态。|

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure PowerShell 示例](../service-fabric-powershell-samples.md)中找到 Azure Service Fabric 的其他 Azure Powershell 示例。
<!-- Update_Description: update meta properties, wording update -->