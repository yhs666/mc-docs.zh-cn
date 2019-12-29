---
title: Azure PowerShell 脚本示例 - 添加网络安全组规则 | Azure
description: Azure PowerShell 脚本示例 - 添加网络安全组以允许特定端口上的入站流量。
services: service-fabric
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.topic: sample
origin.date: 11/28/2017
ms.date: 12/09/2019
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: caa583aa976c0870f174e6844cc0ca14c93d7a19
ms.sourcegitcommit: 4a09701b1cbc1d9ccee46d282e592aec26998bff
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75335732"
---
# <a name="add-an-inbound-network-security-group-rule"></a>添加入站网络安全组规则

本示例脚本创建网络安全组规则，以允许端口 8081 上的入站流量。 该脚本获取网络安全组、创建新的网络安全配置规则，并更新网络安全组。 根据需要自定义参数。

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

必要时，请使用 [Azure PowerShell 指南](https://docs.microsoft.com/powershell/azure/overview)中的说明安装 Azure PowerShell。 

## <a name="sample-script"></a>示例脚本

```powershell
Connect-AzAccount -Environment AzureChinaCloud
Get-AzSubscription
Set-AzContext -SubscriptionId "yourSubscriptionID"

$RGname="sfclustertutorialgroup"
$port=8081
$rulename="allowAppPort$port"
$nsgname="sf-vnet-security"

# Get the NSG resource
$nsg = Get-AzNetworkSecurityGroup -Name $nsgname -ResourceGroupName $RGname

# Add the inbound security rule.
$nsg | Add-AzNetworkSecurityRuleConfig -Name $rulename -Description "Allow app port" -Access Allow `
    -Protocol * -Direction Inbound -Priority 3891 -SourceAddressPrefix "*" -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange $port

# Update the NSG.
$nsg | Set-AzNetworkSecurityGroup

```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [Get-AzResource](https://docs.microsoft.com/powershell/module/az.resources/get-azresource) | 获取 `Microsoft.Network/networkSecurityGroups` 资源。 |
|[Get-AzNetworkSecurityGroup](https://docs.microsoft.com/powershell/module/az.network/get-aznetworksecuritygroup)| 按名称获取网络安全组。|
|[Add-AzNetworkSecurityRuleConfig](https://docs.microsoft.com/powershell/module/az.network/add-aznetworksecurityruleconfig)| 将网络安全规则配置添加到网络安全组。 |
|[Set-AzNetworkSecurityGroup](https://docs.microsoft.com/powershell/module/az.network/set-aznetworksecuritygroup)| 设置网络安全组的目标状态。|

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

<!-- Update_Description: update meta properties, wording update, update link -->