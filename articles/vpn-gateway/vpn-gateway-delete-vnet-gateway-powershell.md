---
title: "删除虚拟网络网关：PowerShell：Azure Resource Manager | Azure"
description: "在 Resource Manager 部署模型中使用 PowerShell 删除虚拟网络网关。"
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 04/12/2017
ms.date: 05/22/2017
ms.author: v-dazen
ms.openlocfilehash: dd72913fe05b003c0025fa517898089a9b1fa1e3
ms.sourcegitcommit: f2f4389152bed7e17371546ddbe1e52c21c0686a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/14/2017
---
# 使用 PowerShell 删除虚拟网络网关
<a id="delete-a-virtual-network-gateway-using-powershell" class="xliff"></a>
> [!div class="op_single_selector"]
> * [Resource Manager - Azure 门户](vpn-gateway-delete-vnet-gateway-portal.md)
> * [Resource Manager - PowerShell](vpn-gateway-delete-vnet-gateway-powershell.md)
> * [经典 - PowerShell](vpn-gateway-delete-vnet-gateway-classic-powershell.md)
>
>

可以使用多种不同的方法来删除 VPN 网关配置中的虚拟网络网关。

- 如果要删除所有信息并从头开始配置（例如，在测试环境中），可以删除资源组。 删除某个资源组时，会删除该组中的所有资源。 仅当不想保留资源组中的任何资源时，才建议使用此方法。 使用这种方法时，无法做到有选择性地删除一部分资源。

- 如果想要保留资源组中的某些资源，则删除虚拟网络网关的过程会略微复杂一些。 在删除虚拟网络网关之前，必须先删除任何依赖于该网关的资源。 遵循的步骤取决于创建的连接类型，以及每个连接的依赖资源。

## 开始之前
<a id="before-beginning" class="xliff"></a>

### 1.下载最新的 Azure Resource Manager PowerShell cmdlet。
<a id="1-download-the-latest-azure-resource-manager-powershell-cmdlets" class="xliff"></a>
下载并安装最新版本的 Azure Resource Manager PowerShell cmdlet。 有关下载和安装 PowerShell cmdlet 的详细信息，请参阅[如何安装和配置 Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)。

### 2.连接到你的 Azure 帐户。
<a id="2-connect-to-your-azure-account" class="xliff"></a> 
打开 PowerShell 控制台并连接到你的帐户。 使用以下示例帮助建立连接：

```powershell
Login-AzureRmAccount -EnvironmentName AzureChinaCloud
```

检查该帐户的订阅。

```powershell
Get-AzureRmSubscription
```

如果有多个订阅，请指定要使用的订阅。

```powershell
Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
```

## <a name="S2S"></a>删除站点到站点 VPN 网关

若要删除 S2S 配置中的虚拟网络网关，必须先删除与该网关相关的每个资源。 由于存在依赖关系，必须按特定的顺序删除资源。 在以下示例中，必须指定某些值，而其他值是输出结果。 为方便演示，我们在示例中使用了以下特定值：

VNet 名称：VNet1<br>
资源组名称：RG1<br>
虚拟网络网关名称：GW1<br>

以下步骤适用于 Resource Manager 部署模型。

### 1.单击要删除的虚拟网络网关。
<a id="1-get-the-virtual-network-gateway-that-you-want-to-delete" class="xliff"></a>

```powershell
$Gateway=get-azurermvirtualnetworkgateway -Name "GW1" -ResourceGroupName "RG1"
```

### 2.检查该虚拟网络网关是否已建立任何连接。
<a id="2-check-to-see-if-the-virtual-network-gateway-has-any-connections" class="xliff"></a>

```powershell
get-azurermvirtualnetworkgatewayconnection -ResourceGroupName "RG1" | where-object {$_.VirtualNetworkGateway1.Id -eq $GW.Id}
$Conns=get-azurermvirtualnetworkgatewayconnection -ResourceGroupName "RG1" | where-object {$_.VirtualNetworkGateway1.Id -eq $GW.Id}
```

### 3.删除所有连接。
<a id="3-delete-all-connections" class="xliff"></a>
系统可能会提示你确认是否要删除每个连接。

```powershell
$Conns | ForEach-Object {Remove-AzureRmVirtualNetworkGatewayConnection -Name $_.name -ResourceGroupName $_.ResourceGroupName}
```

### 4.获取相应本地网络网关的列表。
<a id="4-get-the-list-of-the-corresponding-local-network-gateways" class="xliff"></a>

```powershell
$LNG=Get-AzureRmLocalNetworkGateway -ResourceGroupName "RG1" | where-object {$_.Id -In $Conns.LocalNetworkGateway2.Id}
```

### 5.删除本地网络网关。
<a id="5-delete-the-local-network-gateways" class="xliff"></a>
系统可能会提示你确认是否要删除每个本地网络网关。

```powershell
$LNG | ForEach-Object {Remove-AzureRmLocalNetworkGateway -Name $_.Name -ResourceGroupName $_.ResourceGroupName}
```

### 6.删除虚拟网络网关。
<a id="6-delete-the-virtual-network-gateway" class="xliff"></a>
系统可能会提示你确认是否要删除该网关。

>[!NOTE]
> 除了 S2S 配置，如果你还有此 VNet 的 P2S 配置，则删除虚拟网络网关将自动断开所有 P2S 客户端且不发出警告。
>
>

```powershell
Remove-AzureRmVirtualNetworkGateway -Name "GW1" -ResourceGroupName "RG1"
```

### 7.获取虚拟网络网关的 IP 配置。
<a id="7-get-the-ip-configurations-of-the-virtual-network-gateway" class="xliff"></a>

```powershell
$GWIpConfigs = $Gateway.IpConfigurations
```

### 8.获取此虚拟网络网关使用的公共 IP 地址列表
<a id="8-get-the-list-of-public-ip-addresses-used-for-this-virtual-network-gateway" class="xliff"></a> 
如果虚拟网络网关采用主动-主动配置，将显示两个公共 IP 地址。

```powershell
$PubIP=Get-AzureRmPublicIpAddress | where-object {$_.Id -In $GWIpConfigs.PublicIpAddress.Id}
```

### 9.删除公共 IP。
<a id="9-delete-the-public-ips" class="xliff"></a>

```powershell
$PubIP | foreach-object {remove-azurermpublicIpAddress -Name $_.Name -ResourceGroupName "RG1"}
```

### 10.删除网关子网并设置配置。
<a id="10-delete-the-gateway-subnet-and-set-the-configuration" class="xliff"></a>

```powershell
$GWSub = Get-AzureRmVirtualNetwork -ResourceGroupName "RG1" -Name "VNet1" | Remove-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet"
Set-AzureRmVirtualNetwork -VirtualNetwork $GWSub
```

## <a name="v2v"></a>删除 VNet 到 VNet VPN 网关

若要删除 V2V 配置中的虚拟网络网关，必须先删除与该网关相关的每个资源。 由于存在依赖关系，必须按特定的顺序删除资源。 在以下示例中，必须指定某些值，而其他值是输出结果。 为方便演示，我们在示例中使用了以下特定值：

VNet 名称：VNet1<br>
资源组名称：RG1<br>
虚拟网络网关名称：GW1<br>

以下步骤适用于 Resource Manager 部署模型。

### 1.单击要删除的虚拟网络网关。
<a id="1-get-the-virtual-network-gateway-that-you-want-to-delete" class="xliff"></a>

```powershell
$Gateway=get-azurermvirtualnetworkgateway -Name "GW1" -ResourceGroupName "RG1"
```

### 2.检查该虚拟网络网关是否已建立任何连接。
<a id="2-check-to-see-if-the-virtual-network-gateway-has-any-connections" class="xliff"></a>

```powershell
get-azurermvirtualnetworkgatewayconnection -ResourceGroupName "RG1" | where-object {$_.VirtualNetworkGateway1.Id -eq $GW.Id}
```

与虚拟网络网关建立的其他连接可能属于不同的资源组。 检查其他每个资源组中的其他连接。 在此示例中，我们将检查来自 RG2 的连接。 请针对可能与虚拟网络网关建立了连接的每个资源组运行此步骤。

```powershell
get-azurermvirtualnetworkgatewayconnection -ResourceGroupName "RG2" | where-object {$_.VirtualNetworkGateway2.Id -eq $GW.Id}
```

### 3.获取两个方向的连接列表。
<a id="3-get-the-list-of-connections-in-both-directions" class="xliff"></a>
由于这是一种 VNet 到 VNet 配置，因此需要获取两个方向的连接列表。

```powershell
$ConnsL=get-azurermvirtualnetworkgatewayconnection -ResourceGroupName "RG1" | where-object {$_.VirtualNetworkGateway1.Id -eq $GW.Id}
```

在此示例中，我们将检查来自 RG2 的连接。 请针对可能与虚拟网络网关建立了连接的每个资源组运行此步骤。

```powershell
 $ConnsR=get-azurermvirtualnetworkgatewayconnection -ResourceGroupName "<NameOfResourceGroup2>" | where-object {$_.VirtualNetworkGateway2.Id -eq $GW.Id}
 ```

### 4.删除所有连接。
<a id="4-delete-all-connections" class="xliff"></a>
系统可能会提示你确认是否要删除每个连接。

```powershell
$ConnsL | ForEach-Object {Remove-AzureRmVirtualNetworkGatewayConnection -Name $_.name -ResourceGroupName $_.ResourceGroupName}
$ConnsR | ForEach-Object {Remove-AzureRmVirtualNetworkGatewayConnection -Name $_.name -ResourceGroupName $_.ResourceGroupName}
```

### 5.删除虚拟网络网关。
<a id="5-delete-the-virtual-network-gateway" class="xliff"></a>
系统可能会提示你确认是否要删除该虚拟网络网关。

>[!NOTE]
> 除了 V2V 配置，如果你还有此 VNet 的 P2S 配置，则删除虚拟网络网关将自动断开所有 P2S 客户端且不发出警告。
>
>

```powershell
Remove-AzureRmVirtualNetworkGateway -Name "GW1" -ResourceGroupName "RG1"
```

### 6.获取虚拟网络网关的 IP 配置。
<a id="6-get-the-ip-configurations-of-the-virtual-network-gateway" class="xliff"></a>

```powershell
$GWIpConfigs = $Gateway.IpConfigurations
```

### 7.获取此虚拟网络网关使用的公共 IP 地址列表。
<a id="7-get-the-list-of-public-ip-addresses-used-for-this-virtual-network-gateway" class="xliff"></a> 
如果虚拟网络网关采用主动-主动配置，将显示两个公共 IP 地址。

```powershell
$PubIP=Get-AzureRmPublicIpAddress | where-object {$_.Id -In $GWIpConfigs.PublicIpAddress.Id}
```

### 8.删除公共 IP。
<a id="8-delete-the-public-ips" class="xliff"></a>
系统可能会提示你确认是否要删除该公开 IP。

```powershell
$PubIP | foreach-object {remove-azurermpublicIpAddress -Name $_.Name -ResourceGroupName "<NameOfResourceGroup1>"}
```

### 9.删除网关子网并设置配置。
<a id="9-delete-the-gateway-subnet-and-set-the-configuration" class="xliff"></a>

```powershell
$GWSub = Get-AzureRmVirtualNetwork -ResourceGroupName "RG1" -Name "VNet1" | Remove-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet"
Set-AzureRmVirtualNetwork -VirtualNetwork $GWSub
```

## <a name="deletep2s"></a>删除点到站点 VPN 网关

若要删除 P2S 配置中的虚拟网络网关，必须先删除与该网关相关的每个资源。 由于存在依赖关系，必须按特定的顺序删除资源。 在以下示例中，必须指定某些值，而其他值是输出结果。 为方便演示，我们在示例中使用了以下特定值：

VNet 名称：VNet1<br>
资源组名称：RG1<br>
虚拟网络网关名称：GW1<br>

以下步骤适用于 Resource Manager 部署模型。

>[!NOTE]
> 删除 VPN 网关时，所有连接的客户端将与 VNet 断开连接且不发出警告。
>
>

### 1.单击要删除的虚拟网络网关。
<a id="1-get-the-virtual-network-gateway-that-you-want-to-delete" class="xliff"></a>

```powershell
$Gateway=get-azurermvirtualnetworkgateway -Name "GW1" -ResourceGroupName "RG1"
```

### 2.删除虚拟网络网关。
<a id="2-delete-the-virtual-network-gateway" class="xliff"></a>
系统可能会提示你确认是否要删除该虚拟网络网关。

```powershell
Remove-AzureRmVirtualNetworkGateway -Name "GW1" -ResourceGroupName "RG1"
```

### 3.获取虚拟网络网关的 IP 配置。
<a id="3-get-the-ip-configurations-of-the-virtual-network-gateway" class="xliff"></a>

```powershell
$GWIpConfigs = $Gateway.IpConfigurations
```

### 4.获取此虚拟网络网关使用的公共 IP 地址列表。
<a id="4-get-the-list-of-public-ip-addresses-used-for-this-virtual-network-gateway" class="xliff"></a> 
如果虚拟网络网关采用主动-主动配置，将显示两个公共 IP 地址。

```powershell
$PubIP=Get-AzureRmPublicIpAddress | where-object {$_.Id -In $GWIpConfigs.PublicIpAddress.Id}
```

### 5.删除公共 IP。
<a id="5-delete-the-public-ips" class="xliff"></a>
系统可能会提示你确认是否要删除该公开 IP。

```powershell
$PubIP | foreach-object {remove-azurermpublicIpAddress -Name $_.Name -ResourceGroupName "<NameOfResourceGroup1>"}
```

### 6.删除网关子网并设置配置。
<a id="6-delete-the-gateway-subnet-and-set-the-configuration" class="xliff"></a>

```powershell
$GWSub = Get-AzureRmVirtualNetwork -ResourceGroupName "RG1" -Name "VNet1" | Remove-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet"
Set-AzureRmVirtualNetwork -VirtualNetwork $GWSub
```

## <a name="delete"></a>通过删除资源组来删除 VPN 网关

如果不关心是否要保留资源组中的任何资源，而只是要从头开始配置，则可以删除整个资源组。 这种方法可以快速删除所有信息。 以下步骤仅适用于 Resource Manager 部署模型。

### 1.获取订阅中所有资源组的列表。
<a id="1-get-a-list-of-all-the-resource-groups-in-your-subscription" class="xliff"></a>

```powershell
Get-AzureRmResourceGroup
```

### 2.找到想要删除的资源组。
<a id="2-locate-the-resource-group-that-you-want-to-delete" class="xliff"></a>
找到想要删除的资源组，然后查看该资源组中的资源列表。 在本示例中，资源组的名称为 RG1。 请修改示例以检索所有资源的列表。

```powershell
Find-AzureRmResource -ResourceGroupNameContains RG1
```

### 3.检查列表中的资源。
<a id="3-verify-the-resources-in-the-list" class="xliff"></a>
返回列表后，请检查该列表，确认你要删除该资源组中的所有资源以及资源组本身。 如果要保留资源组中的某些资源，请使用本文之前部分中的步骤删除网关。

### 4.删除资源组和资源。
<a id="4-delete-the-resource-group-and-resources" class="xliff"></a>
若要删除资源组及其包含的所有资源，请修改本示例，然后运行。

```powershell
Remove-AzureRmResourceGroup -Name RG1
```

### 5.检查状态。
<a id="5-check-the-status" class="xliff"></a>
Azure 需要花费一段时间来删除所有资源。 可以使用以下 cmdlet 检查资源组的状态。

```powershell
Get-AzureRmResourceGroup -ResourceGroupName RG1
```

返回的结果显示“Succeeded”。

    ResourceGroupName : RG1
    Location          : chinaeast
    ProvisioningState : Succeeded
