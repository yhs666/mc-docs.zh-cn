---
title: "Azure PowerShell 脚本示例 - 添加网络安全组规则 | Azure"
description: "Azure PowerShell 脚本示例 - 添加网络安全组以允许特定端口上的入站流量。"
services: service-fabric
documentationcenter: 
author: rockboyfor
manager: digimobile
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
origin.date: 11/28/2017
ms.date: 01/01/2018
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: d8daee6e2a33d5a4313f5fcd70358078eb89d3e8
ms.sourcegitcommit: 90e4b45b6c650affdf9d62aeefdd72c5a8a56793
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/29/2017
---
# <a name="add-an-inbound-network-security-group-rule"></a>添加入站网络安全组规则

本示例脚本创建网络安全组规则，以允许端口 8081 上的入站流量。  该脚本获取群集所在的 `Microsoft.Network/networkSecurityGroups` 资源，创建新的网络安全配置规则，并更新网络安全组。 根据需要自定义参数。

必要时，请使用 [Azure PowerShell 指南](https://docs.microsoft.com/powershell/azure/overview)中的说明安装 Azure PowerShell。 

## <a name="sample-script"></a>示例脚本

```powershell
Login-AzureRmAccount -EnvironmentName AzureChinaCloud
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId "yourSubscriptionID"

$RGname="sfclustertutorialgroup"
$port=8081
$rulename="allowAppPort$port"
$nsgname="sf-vnet-security"

# Get the NSG resource
$resource = Get-AzureRmResource | Where {$_.ResourceGroupName -eq $RGname -and $_.ResourceType -eq "Microsoft.Network/networkSecurityGroups"} 
$nsg = Get-AzureRmNetworkSecurityGroup -Name $nsgname -ResourceGroupName $RGname

# Add the inbound security rule.
$nsg | Add-AzureRmNetworkSecurityRuleConfig -Name $rulename -Description "Allow app port" -Access Allow `
    -Protocol * -Direction Inbound -Priority 3891 -SourceAddressPrefix "*" -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange $port

# Update the NSG.
$nsg | Set-AzureRmNetworkSecurityGroup
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [Get-AzureRmResource](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermresource) | 获取 `Microsoft.Network/networkSecurityGroups` 资源。 |
|[Get-AzureRmNetworkSecurityGroup](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermnetworksecuritygroup)| 按名称获取网络安全组。|
|[Add-AzureRmNetworkSecurityRuleConfig](https://docs.microsoft.com/powershell/module/azurerm.network/add-azurermnetworksecurityruleconfig)| 将网络安全规则配置添加到网络安全组。 |
|[Set-AzureRmNetworkSecurityGroup](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermnetworksecuritygroup)| 设置网络安全组的目标状态。|

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。
<!-- Update_Description: new articles on adding service fabric nsg rule with powershell -->