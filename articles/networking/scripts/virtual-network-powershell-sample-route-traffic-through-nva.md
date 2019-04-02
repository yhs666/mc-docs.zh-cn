---
title: Azure PowerShell 脚本示例 - 通过网络虚拟设备路由流量
description: Azure PowerShell 脚本示例 - 通过防火墙网络虚拟设备路由流量。
services: virtual-network
documentationcenter: virtual-network
author: georgewallace
manager: timlt
editor: tysonn
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
origin.date: 05/16/2017
ms.date: 03/11/2019
ms.author: v-biyu
ms.openlocfilehash: 33432e8b3dcaa39f0f95253603b1bbb75209c787
ms.sourcegitcommit: b8fb6890caed87831b28c82738d6cecfe50674fd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/29/2019
ms.locfileid: "58625578"
---
# <a name="route-traffic-through-a-network-virtual-appliance"></a>通过网络虚拟设备路由流量

该脚本示例创建了包含前端和后端子网的虚拟网络。 它还会创建一个 VM，并启用 IP 转发，在两个子网之间路由流量。 运行脚本后，可将网络软件（例如防火墙应用程序）部署到 VM。

必要时，请使用 [Azure PowerShell 指南](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/)中的说明安装 Azure PowerShell，并运行 `Connect-AzAccount` 创建与 Azure 的连接。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>示例脚本

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]
# <a name="variables-for-common-values"></a>常见值的变量
$rgName='MyResourceGroup' $location='chinaeast'

# <a name="create-user-object"></a>创建用户对象
$cred = Get-Credential -Message '输入用于虚拟机的用户名和密码。'

# <a name="create-a-resource-group"></a>创建资源组。
New-AzResourceGroup -Name $rgName -Location $location

# <a name="create-a-virtual-network-a-front-end-subnet-a-back-end-subnet-and-a-dmz-subnet"></a>创建一个虚拟网络、一个前端子网、一个后端子网和一个 DMZ 子网。
$fesubnet = New-AzVirtualNetworkSubnetConfig -Name 'MySubnet-FrontEnd' -AddressPrefix 10.0.1.0/24 $besubnet = New-AzVirtualNetworkSubnetConfig -Name 'MySubnet-BackEnd' -AddressPrefix 10.0.2.0/24 $dmzsubnet = New-AzVirtualNetworkSubnetConfig -Name 'MySubnet-Dmz' -AddressPrefix 10.0.0.0/24

$vnet = New-AzVirtualNetwork -ResourceGroupName $rgName -Name 'MyVnet' -AddressPrefix 10.0.0.0/16 ` -Location $location -Subnet $fesubnet, $besubnet, $dmzsubnet

# <a name="create-nsg-rules-to-allow-http--https-traffic-inbound"></a>创建允许 HTTP 和 HTTPS 入站流量的 NSG 规则。
$rule1 = New-AzNetworkSecurityRuleConfig -Name 'Allow-HTTP-ALL' -Description 'Allow HTTP' `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 ` -SourceAddressPrefix Internet -SourcePortRange * ` -DestinationAddressPrefix * -DestinationPortRange 80

$rule2 = New-AzNetworkSecurityRuleConfig -Name 'Allow-HTTPS-All' -Description 'Allow HTTPS' `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 200 ` -SourceAddressPrefix Internet -SourcePortRange * ` -DestinationAddressPrefix * -DestinationPortRange 443

# <a name="create-a-network-security-group-nsg-for-the-front-end-subnet"></a>为前端子网创建网络安全组 (NSG)。
$nsg = New-AzNetworkSecurityGroup -ResourceGroupName $RgName -Location $location ` -Name 'MyNsg-FrontEnd' -SecurityRules $rule1,$rule2

# <a name="associate-the-front-end-nsg-to-the-front-end-subnet"></a>将前端 NSG 关联到前端子网。
Set-AzVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name 'MySubnet-FrontEnd' ` -AddressPrefix '10.0.1.0/24' -NetworkSecurityGroup $nsg

# <a name="create-a-public-ip-address-for-the-firewall-vm"></a>为防火墙 VM 创建一个公共 IP 地址。
$publicip = New-AzPublicIpAddress -ResourceGroupName $rgName -Name 'MyPublicIP-Firewall' ` -location $location -AllocationMethod Dynamic

# <a name="create-a-nic-for-the-firewall-vm-and-enable-ip-forwarding"></a>为防火墙 VM 创建一个 NIC 并启用 IP 转发。
$nicVMFW = New-AzNetworkInterface -ResourceGroupName $rgName -Location $location -Name 'MyNic-Firewall' ` -PublicIpAddress $publicip -Subnet $vnet.Subnets[2] -EnableIPForwarding

# <a name="create-a-firewall-vm-to-accept-all-traffic-between-the-front-and-back-end-subnets"></a>创建一个防火墙 VM，接受前端和后端子网之间的所有流量。
$vmConfig = New-AzVMConfig -VMName 'MyVm-Firewall' -VMSize Standard_DS2 | `
    Set-AzureRmVMOperatingSystem -Windows -ComputerName 'MyVm-Firewall' -Credential $cred | ` Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer ` -Skus 2016-Datacenter -Version latest | Add-AzureRmVMNetworkInterface -Id $nicVMFW.Id
    
$vm = New-AzVM -ResourceGroupName $rgName -Location $location -VM $vmConfig

# <a name="create-a-route-for-traffic-from-the-front-end-to-the-back-end-subnet-through-the-firewall-vm"></a>创建一个路由，该路由适用于通过防火墙 VM 从前端子网发往后端子网的流量。
$route = New-AzRouteConfig -Name 'RouteToBackEnd' -AddressPrefix 10.0.2.0/24 ` -NextHopType VirtualAppliance -NextHopIpAddress $nicVMFW.IpConfigurations[0].PrivateIpAddress

# <a name="create-a-route-for-traffic-from-the-front-end-subnet-to-the-internet-through-the-firewall-vm"></a>创建一个路由，该路由适用于通过防火墙 VM 从前端子网发往 Internet 的流量。
$route2 = New-AzRouteConfig -Name 'RouteToInternet' -AddressPrefix 0.0.0.0/0 ` -NextHopType VirtualAppliance -NextHopIpAddress $nicVMFW.IpConfigurations[0].PrivateIpAddress

# <a name="create-route-table-for-the-frontend-subnet"></a>创建 FrontEnd 子网的路由表。
$routeTableFEtoBE = New-AzRouteTable -Name 'MyRouteTable-FrontEnd' -ResourceGroupName $rgName ` -location $location -Route $route, $route2

# <a name="associate-the-route-table-to-the-frontend-subnet"></a>将路由表关联到 FrontEnd 子网。
Set-AzVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name 'MySubnet-FrontEnd' -AddressPrefix 10.0.1.0/24 ` -NetworkSecurityGroup $nsg -RouteTable $routeTableFEtoBE
  
# <a name="create-a-route-for-traffic-from-the-back-end-subnet-to-the-front-end-subnet-through-the-firewall-vm"></a>创建一个路由，该路由适用于通过防火墙 VM 从后端子网发往前端子网的流量。
$route = New-AzRouteConfig -Name 'RouteToFrontEnd' -AddressPrefix '10.0.1.0/24' -NextHopType VirtualAppliance ` -NextHopIpAddress $nicVMFW.IpConfigurations[0].PrivateIPAddress

# <a name="create-a-route-for-traffic-from-the-back-end-subnet-to-the-internet-through-the-firewall-vm"></a>创建一个路由，该路由适用于通过防火墙 VM 从后端子网发往 Internet 的流量。
$route2 = New-AzRouteConfig -Name 'RouteToInternet' -AddressPrefix '0.0.0.0/0' -NextHopType VirtualAppliance ` -NextHopIpAddress $nicVMFW.IpConfigurations[0].PrivateIPAddress

# <a name="create-route-table-for-the-backend-subnet"></a>创建 BackEnd 子网的路由表。
$routeTableBE = New-AzRouteTable -Name 'MyRouteTable-BackEnd' -ResourceGroupName $rgName ` -location $location -Route $route, $route2

# <a name="associate-the-route-table-to-the-backend-subnet"></a>将路由表关联到 BackEnd 子网。
Set-AzVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name 'MySubnet-BackEnd' ` -AddressPrefix '10.0.2.0/24' -RouteTable $routeTableBE

## <a name="clean-up-deployment"></a>清理部署 

运行以下命令来删除资源组、VM 和所有相关资源。

```powershell
Remove-AzResourceGroup -Name myResourceGroup
```
## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建资源组、虚拟网络和网络安全组。 表中的每条命令链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [New-AzResourceGroup](https://docs.microsoft.com/zh-cn/powershell/module/az.resources/new-azresourcegroup)  | 创建用于存储所有资源的资源组。 |
| [New-AzVirtualNetwork](https://docs.microsoft.com/zh-cn/powershell/module/az.network/new-azvirtualnetwork) | 创建 Azure 虚拟网络和前端子网。 |
| [New-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/zh-cn/powershell/module/az.network/new-azvirtualnetworksubnetconfig) | 创建后端子网和 DMZ 子网。 |
| [New-AzPublicIpAddress](https://docs.microsoft.com/zh-cn/powershell/module/az.network/new-azpublicipaddress) | 创建用于从 Internet 访问 VM 的公共 IP 地址。 |
| [New-AzNetworkInterface](https://docs.microsoft.com/zh-cn/powershell/module/az.network/new-aznetworkinterface) | 创建虚拟网络接口，并对它启用 IP 转发。 |
| [New-AzNetworkSecurityGroup](https://docs.microsoft.com/zh-cn/powershell/module/az.network/new-aznetworksecuritygroup) | 创建网络安全组 (NSG)。 |
| [New-AzNetworkSecurityRuleConfig](https://docs.microsoft.com/zh-cn/powershell/module/az.network/new-aznetworksecurityruleconfig) | 创建允许 HTTP 和 HTTPS 端口入站到 VM 的 NSG 规则。 |
| [Set-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/zh-cn/powershell/module/az.network/set-azvirtualnetworksubnetconfig)| 将 NSG 和路由表关联到子网。 |
| [New-AzRouteTable](https://docs.microsoft.com/zh-cn/powershell/module/az.network/new-azroutetable)| 为所有路由创建路由表。 |
| [New-AzRouteConfig](https://docs.microsoft.com/zh-cn/powershell/module/az.network/new-azrouteconfig)| 创建路由，通过 VM 在子网和 Internet 之间路由流量。 |
| [New-AzVM](https://docs.microsoft.com/zh-cn/powershell/module/az.compute/new-azvm) | 创建虚拟机并向其附加 NIC。 此命令还指定要使用的虚拟机映像和管理凭据。 |
| [Remove-AzResourceGroup](https://docs.microsoft.com/zh-cn/powershell/module/az.resources/remove-azresourcegroup)  | 删除资源组及其包含的所有资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可在 [Azure 网络概述文档](../powershell-samples.md?toc=%2fnetworking%2ftoc.json)中找到其他网络 PowerShell 脚本示例。
