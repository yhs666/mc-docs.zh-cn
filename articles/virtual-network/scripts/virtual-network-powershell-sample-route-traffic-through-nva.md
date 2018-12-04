---
title: Azure PowerShell 脚本示例 - 通过网络虚拟设备路由流量 | Azure
description: Azure PowerShell 脚本示例 - 通过防火墙网络虚拟设备路由流量。
services: virtual-network
documentationcenter: virtual-network
author: rockboyfor
manager: digimobile
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: powershell
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: infrastructure
origin.date: 03/20/2018
ms.date: 05/07/2018
ms.author: v-yeche
ms.openlocfilehash: fdc4a036df7956a0b7fe0d6f69eb1cd1a39975a5
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52652852"
---
# <a name="route-traffic-through-a-network-virtual-appliance-script-sample"></a>通过网络虚拟设备脚本示例路由流量

该脚本示例创建了包含前端和后端子网的虚拟网络。 它还会创建一个 VM，并启用 IP 转发，在两个子网之间路由流量。 运行脚本后，可将网络软件（例如防火墙应用程序）部署到 VM。

可以通过 Azure [Cloud Shell](https://shell.azure.com/powershell) 或本地 PowerShell 安装来执行脚本。 如果在本地使用 PowerShell，则此脚本需要 AzureRM PowerShell 模块 5.4.1 或更高版本。 要查找已安装的版本，请运行 `Get-Module -ListAvailable AzureRM`。 如果需要进行升级，请参阅 [Install Azure PowerShell module](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)（安装 Azure PowerShell 模块）。 如果在本地运行 PowerShell，则还需运行 `Login-AzureRmAccount -EnvironmentName AzureChinaCloud` 以创建与 Azure 的连接。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>示例脚本

```powershell
# Variables for common values
$rgName='MyResourceGroup'
$location='chinaeast'

# Create user object
$cred = Get-Credential -Message 'Enter a username and password for the virtual machine.'

# Create a resource group.
New-AzureRmResourceGroup -Name $rgName -Location $location

# Create a virtual network, a front-end subnet, a back-end subnet, and a DMZ subnet.
$fesubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'MySubnet-FrontEnd' -AddressPrefix 10.0.1.0/24
$besubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'MySubnet-BackEnd' -AddressPrefix 10.0.2.0/24
$dmzsubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'MySubnet-Dmz' -AddressPrefix 10.0.0.0/24

$vnet = New-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name 'MyVnet' -AddressPrefix 10.0.0.0/16 `
  -Location $location -Subnet $fesubnet, $besubnet, $dmzsubnet

# Create NSG rules to allow HTTP & HTTPS traffic inbound.
$rule1 = New-AzureRmNetworkSecurityRuleConfig -Name 'Allow-HTTP-ALL' -Description 'Allow HTTP' `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 `
  -SourceAddressPrefix Internet -SourcePortRange * `
  -DestinationAddressPrefix * -DestinationPortRange 80

$rule2 = New-AzureRmNetworkSecurityRuleConfig -Name 'Allow-HTTPS-All' -Description 'Allow HTTPS' `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 200 `
  -SourceAddressPrefix Internet -SourcePortRange * `
  -DestinationAddressPrefix * -DestinationPortRange 443

# Create a network security group (NSG) for the front-end subnet.
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $RgName -Location $location `
-Name 'MyNsg-FrontEnd' -SecurityRules $rule1,$rule2

# Associate the front-end NSG to the front-end subnet.
Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name 'MySubnet-FrontEnd' `
  -AddressPrefix '10.0.1.0/24' -NetworkSecurityGroup $nsg

# Create a public IP address for the firewall VM.
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName $rgName -Name 'MyPublicIP-Firewall' `
  -location $location -AllocationMethod Dynamic

# Create a NIC for the firewall VM and enable IP forwarding.
$nicVMFW = New-AzureRmNetworkInterface -ResourceGroupName $rgName -Location $location -Name 'MyNic-Firewall' `
  -PublicIpAddress $publicip -Subnet $vnet.Subnets[2] -EnableIPForwarding

#Create a firewall VM to accept all traffic between the front and back-end subnets.
$vmConfig = New-AzureRmVMConfig -VMName 'MyVm-Firewall' -VMSize Standard_DS2 | `
    Set-AzureRmVMOperatingSystem -Windows -ComputerName 'MyVm-Firewall' -Credential $cred | `
    Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer `
    -Skus 2016-Datacenter -Version latest | Add-AzureRmVMNetworkInterface -Id $nicVMFW.Id

$vm = New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vmConfig

# Create a route for traffic from the front-end to the back-end subnet through the firewall VM.
$route = New-AzureRmRouteConfig -Name 'RouteToBackEnd' -AddressPrefix 10.0.2.0/24 `
  -NextHopType VirtualAppliance -NextHopIpAddress $nicVMFW.IpConfigurations[0].PrivateIpAddress

# Create a route for traffic from the front-end subnet to the Internet through the firewall VM.
$route2 = New-AzureRmRouteConfig -Name 'RouteToInternet' -AddressPrefix 0.0.0.0/0 `
  -NextHopType VirtualAppliance -NextHopIpAddress $nicVMFW.IpConfigurations[0].PrivateIpAddress

# Create route table for the FrontEnd subnet.
$routeTableFEtoBE = New-AzureRmRouteTable -Name 'MyRouteTable-FrontEnd' -ResourceGroupName $rgName `
  -location $location -Route $route, $route2

# Associate the route table to the FrontEnd subnet.
Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name 'MySubnet-FrontEnd' -AddressPrefix 10.0.1.0/24 `
  -NetworkSecurityGroup $nsg -RouteTable $routeTableFEtoBE

# Create a route for traffic from the back-end subnet to the front-end subnet through the firewall VM.
$route = New-AzureRmRouteConfig -Name 'RouteToFrontEnd' -AddressPrefix '10.0.1.0/24' -NextHopType VirtualAppliance `
  -NextHopIpAddress $nicVMFW.IpConfigurations[0].PrivateIPAddress

# Create a route for traffic from the back-end subnet to the Internet through the firewall VM.
$route2 = New-AzureRmRouteConfig -Name 'RouteToInternet' -AddressPrefix '0.0.0.0/0' -NextHopType VirtualAppliance `
  -NextHopIpAddress $nicVMFW.IpConfigurations[0].PrivateIPAddress

# Create route table for the BackEnd subnet.
$routeTableBE = New-AzureRmRouteTable -Name 'MyRouteTable-BackEnd' -ResourceGroupName $rgName `
  -location $location -Route $route, $route2

# Associate the route table to the BackEnd subnet.
Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name 'MySubnet-BackEnd' `
  -AddressPrefix '10.0.2.0/24' -RouteTable $routeTableBE
```

## <a name="clean-up-deployment"></a>清理部署 

运行以下命令来删除资源组、VM 和所有相关资源：

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```
## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建资源组、虚拟网络和网络安全组。 下表中的每条命令均链接到特定于命令的文档：

| 命令 | 注释 |
|---|---|
| [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup)  | 创建用于存储所有资源的资源组。 |
| [New-AzureRmVirtualNetwork](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetwork) | 创建 Azure 虚拟网络和前端子网。 |
| [New-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | 创建后端子网和 DMZ 子网。 |
| [New-AzureRmPublicIpAddress](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermpublicipaddress) | 创建用于从 Internet 访问 VM 的公共 IP 地址。 |
| [New-AzureRmNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermnetworkinterface) | 创建虚拟网络接口，并对它启用 IP 转发。 |
| [New-AzureRmNetworkSecurityGroup](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | 创建网络安全组 (NSG)。 |
| [New-AzureRmNetworkSecurityRuleConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | 创建允许 HTTP 和 HTTPS 端口入站到 VM 的 NSG 规则。 |
| [Set-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig)| 将 NSG 和路由表关联到子网。 |
| [New-AzureRmRouteTable](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermroutetable)| 为所有路由创建路由表。 |
| [New-AzureRmRouteConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermrouteconfig)| 创建路由，通过 VM 在子网和 Internet 之间路由流量。 |
| [New-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvm) | 创建虚拟机并向其附加 NIC。 此命令还指定要使用的虚拟机映像和管理凭据。 |
| [Remove-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/remove-azurermresourcegroup)  | 删除资源组及其包含的所有资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可在[虚拟网络 PowerShell 示例](../powershell-samples.md)中查找其他虚拟网络 PowerShell 脚本示例。
<!-- Update_Description: new articles on virtual network powershell sample route traffic through nva script -->
<!--ms.date: 05/07/2018-->