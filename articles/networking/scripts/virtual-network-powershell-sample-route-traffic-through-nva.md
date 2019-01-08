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
ms.date: 01/07/2019
ms.author: v-biyu
ms.openlocfilehash: 19bb8300e36773655fd051023cc5bdbb188ca848
ms.sourcegitcommit: a46f12240aea05f253fb4445b5e88564a2a2a120
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/26/2018
ms.locfileid: "53785307"
---
# <a name="route-traffic-through-a-network-virtual-appliance"></a>通过网络虚拟设备路由流量

该脚本示例创建了包含前端和后端子网的虚拟网络。 它还会创建一个 VM，并启用 IP 转发，在两个子网之间路由流量。 运行脚本后，可将网络软件（例如防火墙应用程序）部署到 VM。

必要时，请使用 [Azure PowerShell 指南](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/)中的说明安装 Azure PowerShell，并运行 `Connect-AzureRmAccount` 创建与 Azure 的连接。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>示例脚本


# <a name="variables-for-common-values"></a>常见值的变量
$rgName='MyResourceGroup' $location='chinaeast'

# <a name="create-user-object"></a>创建用户对象
$cred = Get-Credential -Message '输入用于虚拟机的用户名和密码。'

# <a name="create-a-resource-group"></a>创建资源组。
New-AzureRmResourceGroup -Name $rgName -Location $location

# <a name="create-a-virtual-network-a-front-end-subnet-a-back-end-subnet-and-a-dmz-subnet"></a>创建一个虚拟网络、一个前端子网、一个后端子网和一个 DMZ 子网。
$fesubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'MySubnet-FrontEnd' -AddressPrefix 10.0.1.0/24 $besubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'MySubnet-BackEnd' -AddressPrefix 10.0.2.0/24 $dmzsubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'MySubnet-Dmz' -AddressPrefix 10.0.0.0/24

$vnet = New-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name 'MyVnet' -AddressPrefix 10.0.0.0/16 ` -Location $location -Subnet $fesubnet, $besubnet, $dmzsubnet

# <a name="create-nsg-rules-to-allow-http--https-traffic-inbound"></a>创建允许 HTTP 和 HTTPS 入站流量的 NSG 规则。
$rule1 = New-AzureRmNetworkSecurityRuleConfig -Name 'Allow-HTTP-ALL' -Description 'Allow HTTP' `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 ` -SourceAddressPrefix Internet -SourcePortRange * ` -DestinationAddressPrefix * -DestinationPortRange 80

$rule2 = New-AzureRmNetworkSecurityRuleConfig -Name 'Allow-HTTPS-All' -Description 'Allow HTTPS' `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 200 ` -SourceAddressPrefix Internet -SourcePortRange * ` -DestinationAddressPrefix * -DestinationPortRange 443

# <a name="create-a-network-security-group-nsg-for-the-front-end-subnet"></a>为前端子网创建网络安全组 (NSG)。
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $RgName -Location $location ` -Name 'MyNsg-FrontEnd' -SecurityRules $rule1,$rule2

# <a name="associate-the-front-end-nsg-to-the-front-end-subnet"></a>将前端 NSG 关联到前端子网。
Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name 'MySubnet-FrontEnd' ` -AddressPrefix '10.0.1.0/24' -NetworkSecurityGroup $nsg

# <a name="create-a-public-ip-address-for-the-firewall-vm"></a>为防火墙 VM 创建一个公共 IP 地址。
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName $rgName -Name 'MyPublicIP-Firewall' ` -location $location -AllocationMethod Dynamic

# <a name="create-a-nic-for-the-firewall-vm-and-enable-ip-forwarding"></a>为防火墙 VM 创建一个 NIC 并启用 IP 转发。
$nicVMFW = New-AzureRmNetworkInterface -ResourceGroupName $rgName -Location $location -Name 'MyNic-Firewall' ` -PublicIpAddress $publicip -Subnet $vnet.Subnets[2] -EnableIPForwarding

#<a name="create-a-firewall-vm-to-accept-all-traffic-between-the-front-and-back-end-subnets"></a>创建一个防火墙 VM，接受前端和后端子网之间的所有流量。
$vmConfig = New-AzureRmVMConfig -VMName 'MyVm-Firewall' -VMSize Standard_DS2 | `
    Set-AzureRmVMOperatingSystem -Windows -ComputerName 'MyVm-Firewall' -Credential $cred | ` Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer ` -Skus 2016-Datacenter -Version latest | Add-AzureRmVMNetworkInterface -Id $nicVMFW.Id
    
$vm = New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vmConfig

# <a name="create-a-route-for-traffic-from-the-front-end-to-the-back-end-subnet-through-the-firewall-vm"></a>创建一个路由，该路由适用于通过防火墙 VM 从前端子网发往后端子网的流量。
$route = New-AzureRmRouteConfig -Name 'RouteToBackEnd' -AddressPrefix 10.0.2.0/24 ` -NextHopType VirtualAppliance -NextHopIpAddress $nicVMFW.IpConfigurations[0].PrivateIpAddress

# <a name="create-a-route-for-traffic-from-the-front-end-subnet-to-the-internet-through-the-firewall-vm"></a>创建一个路由，该路由适用于通过防火墙 VM 从前端子网发往 Internet 的流量。
$route2 = New-AzureRmRouteConfig -Name 'RouteToInternet' -AddressPrefix 0.0.0.0/0 ` -NextHopType VirtualAppliance -NextHopIpAddress $nicVMFW.IpConfigurations[0].PrivateIpAddress

# <a name="create-route-table-for-the-frontend-subnet"></a>创建 FrontEnd 子网的路由表。
$routeTableFEtoBE = New-AzureRmRouteTable -Name 'MyRouteTable-FrontEnd' -ResourceGroupName $rgName ` -location $location -Route $route, $route2

# <a name="associate-the-route-table-to-the-frontend-subnet"></a>将路由表关联到 FrontEnd 子网。
Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name 'MySubnet-FrontEnd' -AddressPrefix 10.0.1.0/24 ` -NetworkSecurityGroup $nsg -RouteTable $routeTableFEtoBE
  
# <a name="create-a-route-for-traffic-from-the-back-end-subnet-to-the-front-end-subnet-through-the-firewall-vm"></a>创建一个路由，该路由适用于通过防火墙 VM 从后端子网发往前端子网的流量。
$route = New-AzureRmRouteConfig -Name 'RouteToFrontEnd' -AddressPrefix '10.0.1.0/24' -NextHopType VirtualAppliance ` -NextHopIpAddress $nicVMFW.IpConfigurations[0].PrivateIPAddress

# <a name="create-a-route-for-traffic-from-the-back-end-subnet-to-the-internet-through-the-firewall-vm"></a>创建一个路由，该路由适用于通过防火墙 VM 从后端子网发往 Internet 的流量。
$route2 = New-AzureRmRouteConfig -Name 'RouteToInternet' -AddressPrefix '0.0.0.0/0' -NextHopType VirtualAppliance ` -NextHopIpAddress $nicVMFW.IpConfigurations[0].PrivateIPAddress

# <a name="create-route-table-for-the-backend-subnet"></a>创建 BackEnd 子网的路由表。
$routeTableBE = New-AzureRmRouteTable -Name 'MyRouteTable-BackEnd' -ResourceGroupName $rgName ` -location $location -Route $route, $route2

# <a name="associate-the-route-table-to-the-backend-subnet"></a>将路由表关联到 BackEnd 子网。
Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name 'MySubnet-BackEnd' ` -AddressPrefix '10.0.2.0/24' -RouteTable $routeTableBE

## <a name="clean-up-deployment"></a>清理部署 

运行以下命令来删除资源组、VM 和所有相关资源。

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```
## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建资源组、虚拟网络和网络安全组。 表中的每条命令链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [New-AzureRmResourceGroup](https://docs.microsoft.com/zh-cn/powershell/module/azurerm.resources/new-azurermresourcegroup)  | 创建用于存储所有资源的资源组。 |
| [New-AzureRmVirtualNetwork](https://docs.microsoft.com/zh-cn/powershell/module/azurerm.network/new-azurermvirtualnetwork) | 创建 Azure 虚拟网络和前端子网。 |
| [New-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/zh-cn/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | 创建后端子网和 DMZ 子网。 |
| [New-AzureRmPublicIpAddress](https://docs.microsoft.com/zh-cn/powershell/module/azurerm.network/new-azurermpublicipaddress) | 创建用于从 Internet 访问 VM 的公共 IP 地址。 |
| [New-AzureRmNetworkInterface](https://docs.microsoft.com/zh-cn/powershell/module/azurerm.network/new-azurermnetworkinterface) | 创建虚拟网络接口，并对它启用 IP 转发。 |
| [New-AzureRmNetworkSecurityGroup](https://docs.microsoft.com/zh-cn/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | 创建网络安全组 (NSG)。 |
| [New-AzureRmNetworkSecurityRuleConfig](https://docs.microsoft.com/zh-cn/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | 创建允许 HTTP 和 HTTPS 端口入站到 VM 的 NSG 规则。 |
| [Set-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/zh-cn/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig)| 将 NSG 和路由表关联到子网。 |
| [New-AzureRmRouteTable](https://docs.microsoft.com/zh-cn/powershell/module/azurerm.network/new-azurermroutetable)| 为所有路由创建路由表。 |
| [New-AzureRmRouteConfig](https://docs.microsoft.com/zh-cn/powershell/module/azurerm.network/new-azurermrouteconfig)| 创建路由，通过 VM 在子网和 Internet 之间路由流量。 |
| [New-AzureRmVM](https://docs.microsoft.com/zh-cn/powershell/module/azurerm.compute/new-azurermvm) | 创建虚拟机并向其附加 NIC。 此命令还指定要使用的虚拟机映像和管理凭据。 |
| [Remove-AzureRmResourceGroup](https://docs.microsoft.com/zh-cn/powershell/module/azurerm.resources/remove-azurermresourcegroup)  | 删除资源组及其包含的所有资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可在 [Azure 网络概述文档](../powershell-samples.md?toc=%2fnetworking%2ftoc.json)中找到其他网络 PowerShell 脚本示例。
