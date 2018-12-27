---
title: 创建基于路由的 Azure VPN 网关：PowerShell | Microsoft Docs
description: 使用 PowerShell 快速创建基于路由的 VPN 网关
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: article
origin.date: 10/18/2018
ms.date: 12/10/2018
ms.author: v-jay
ms.openlocfilehash: 97df7c9dd0d8704031c096229b3af8735e4cc7e3
ms.sourcegitcommit: 5f2849d5751cb634f1cdc04d581c32296e33ef1b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2018
ms.locfileid: "53028394"
---
# <a name="create-a-route-based-vpn-gateway-using-powershell"></a>使用 PowerShell 创建基于路由的 VPN 网关

本文可帮助你使用 PowerShell 快速创建基于路由的 Azure VPN 网关。 创建与本地网络的 VPN 连接时使用 VPN 网关。 还可以使用 VPN 网关连接 VNet。

本文中的步骤将创建 VNet、子网、网关子网和基于路由的 VPN 网关（虚拟网络网关）。 完成网关创建后，可以创建连接。 执行这些步骤需要 Azure 订阅。 如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial/?WT.mc_id=A261C142F)。

本教程需要 Azure PowerShell 模块 5.3.0 或更高版本。 运行 ` Get-Module -ListAvailable AzureRM` 即可查找版本。 如果需要升级，请参阅[安装 Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)。 如果在本地运行 PowerShell，则还需运行 `Connect-AzureRmAccount -Environment AzureChinaCloud` 来创建与 Azure 的连接。

## <a name="create-a-resource-group"></a>创建资源组

使用 [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup) 创建 Azure 资源组。 资源组是在其中部署和管理 Azure 资源的逻辑容器。 

```azurepowershell
New-AzureRmResourceGroup -Name TestRG1 -Location ChinaNorth
```

## <a name="vnet"></a>创建虚拟网络

使用 [New-AzureRmVirtualNetwork](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetwork) 创建虚拟网络。 以下示例在“ChinaNorth”位置创建一个名为“VNet1”的虚拟网络：

```azurepowershell
$virtualNetwork = New-AzureRmVirtualNetwork `
  -ResourceGroupName TestRG1 `
  -Location ChinaNorth `
  -Name VNet1 `
  -AddressPrefix 10.1.0.0/16
```

使用 [New-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) cmdlet 创建子网配置。

```azurepowershell
$subnetConfig = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Frontend `
  -AddressPrefix 10.1.0.0/24 `
  -VirtualNetwork $virtualNetwork
```

使用 [Set-AzureRmVirtualNetwork](https://docs.microsoft.com/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork) cmdlet 为该虚拟网络设置子网配置。


```azurepowershell
$virtualNetwork | Set-AzureRmVirtualNetwork
```

## <a name="gwsubnet"></a>添加网关子网

网关子网包含虚拟网络网关服务使用的保留 IP 地址。 使用下面的示例添加网关子网：

为 VNet 设置变量。

```azurepowershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG1 -Name VNet1
```

使用 [Add-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/Add-AzureRmVirtualNetworkSubnetConfig) cmdlet 创建网关子网。

```azurepowershell
Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.1.255.0/27 -VirtualNetwork $vnet
```

使用 [Set-AzureRmVirtualNetwork](https://docs.microsoft.com/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork) cmdlet 为该虚拟网络设置子网配置。

```azurepowershell
$virtualNetwork | Set-AzureRmVirtualNetwork
```

## <a name="PublicIP"></a>请求公共 IP 地址

VPN 网关必须具有动态分配的公共 IP 地址。 创建与 VPN 网关的连接时，这是你指定的 IP 地址。 使用下面的示例请求一个公共 IP 地址：

```azurepowershell
$gwpip= New-AzureRmPublicIpAddress -Name VNet1GWIP -ResourceGroupName TestRG1 -Location 'China North' -AllocationMethod Dynamic
```

## <a name="GatewayIPConfig"></a>创建网关 IP 地址配置

网关配置定义要使用的子网和公共 IP 地址。 使用以下示例创建网关配置：

```azurepowershell
$vnet = Get-AzureRmVirtualNetwork -Name VNet1 -ResourceGroupName TestRG1
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
$gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name gwipconfig1 -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id
```
## <a name="CreateGateway"></a>创建 VPN 网关

创建 VPN 网关可能需要 45 分钟或更长时间。 完成创建网关后，可以创建虚拟网络与另一个 VNet 之间的连接。 或者，创建虚拟网络与本地位置之间的连接。 使用 [New-AzureRmVirtualNetworkGateway](https://docs.microsoft.com/powershell/module/azurerm.network/New-AzureRmVirtualNetworkGateway) cmdlet 创建 VPN 网关。

```azurepowershell
New-AzureRmVirtualNetworkGateway -Name VNet1GW -ResourceGroupName TestRG1 `
-Location 'China North' -IpConfigurations $gwipconfig -GatewayType Vpn `
-VpnType RouteBased -GatewaySku VpnGw1
```

## <a name="viewgw"></a>查看 VPN 网关

可以使用 [Get-AzureRmVirtualNetworkGateway](https://docs.microsoft.com/powershell/module/azurerm.network/Get-AzureRmVirtualNetworkGateway) cmdlet 查看 VPN 网关。

```azurepowershell
Get-AzureRmVirtualNetworkGateway -Name Vnet1GW -ResourceGroup TestRG1
```

输出将类似于以下示例：

```
Name                   : VNet1GW
ResourceGroupName      : TestRG1
Location               : chinanorth
Id                     : /subscriptions/<subscription ID>/resourceGroups/TestRG1/provide
                         rs/Microsoft.Network/virtualNetworkGateways/VNet1GW
Etag                   : W/"0952d-9da8-4d7d-a8ed-28c8ca0413"
ResourceGuid           : dc6ce1de-2c4494-9d0b-20b03ac595
ProvisioningState      : Succeeded
Tags                   :
IpConfigurations       : [
                           {
                             "PrivateIpAllocationMethod": "Dynamic",
                             "Subnet": {
                               "Id": "/subscriptions/<subscription ID>/resourceGroups/Te
                         stRG1/providers/Microsoft.Network/virtualNetworks/VNet1/subnets/GatewaySubnet"
                             },
                             "PublicIpAddress": {
                               "Id": "/subscriptions/<subscription ID>/resourceGroups/Te
                         stRG1/providers/Microsoft.Network/publicIPAddresses/VNet1GWIP"
                             },
                             "Name": "default",
                             "Etag": "W/\"0952d-9da8-4d7d-a8ed-28c8ca0413\"",
                             "Id": "/subscriptions/<subscription ID>/resourceGroups/Test
                         RG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW/ipConfigurations/de
                         fault"
                           }
                         ]
GatewayType            : Vpn
VpnType                : RouteBased
EnableBgp              : False
ActiveActive           : False
GatewayDefaultSite     : null
Sku                    : {
                           "Capacity": 2,
                           "Name": "VpnGw1",
                           "Tier": "VpnGw1"
                         }
VpnClientConfiguration : null
BgpSettings            : {
     
```

## <a name="viewgwpip"></a>查看公共 IP 地址

若要查看 VPN 网关的公共 IP 地址，请使用 [Get-AzureRmPublicIpAddress](https://docs.microsoft.com/powershell/module/azurerm.network/Get-AzureRmPublicIpAddress) cmdlet。

```azurepowershell
Get-AzureRmPublicIpAddress -Name VNet1GWIP -ResourceGroupName TestRG1
```

在此示例响应中，IpAddress 值是公共 IP 地址。

```
Name                     : VNet1GWIP
ResourceGroupName        : TestRG1
Location                 : chinanorth
Id                       : /subscriptions/<subscription ID>/resourceGroups/TestRG1/provi
                           ders/Microsoft.Network/publicIPAddresses/VNet1GWIP
Etag                     : W/"5001666a-bc2a-484b-bcf5-ad488dabd8ca"
ResourceGuid             : 3c7c481e-9828-4dae-abdc-f95b383
ProvisioningState        : Succeeded
Tags                     :
PublicIpAllocationMethod : Dynamic
IpAddress                : 13.90.153.3
PublicIpAddressVersion   : IPv4
IdleTimeoutInMinutes     : 4
IpConfiguration          : {
                             "Id": "/subscriptions/<subscription ID>/resourceGroups/Test
                           RG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW/ipConfigurations/
                           default"
                           }
DnsSettings              : null
Zones                    : {}
Sku                      : {
                             "Name": "Basic"
                           }
IpTags                   : {}
```

## <a name="clean-up-resources"></a>清理资源

如果不再需要所创建的资源，请使用 [Remove-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/remove-azurermresourcegroup) 命令删除资源组。 这将删除资源组及其包含的所有资源。

```azurepowershell
Remove-AzureRmResourceGroup -Name TestRG1
```

## <a name="next-steps"></a>后续步骤

完成创建网关后，可以创建虚拟网络与另一个 VNet 之间的连接。 或者，创建虚拟网络与本地位置之间的连接。

> [!div class="nextstepaction"]
> [创建站点到站点连接](vpn-gateway-create-site-to-site-rm-powershell.md)<br><br>
> [创建点到站点连接](vpn-gateway-howto-point-to-site-rm-ps.md)<br><br>
> [创建与另一个 VNet 的连接](vpn-gateway-vnet-vnet-rm-ps.md)

<!-- Update_Description: code update -->