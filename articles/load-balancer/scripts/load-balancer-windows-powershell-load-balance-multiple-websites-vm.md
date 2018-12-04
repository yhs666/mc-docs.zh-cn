---
title: PowerShell 示例 - 使用 Azure PowerShell 对多个网站进行负载均衡 | Azure
description: 本 Azure PowerShell 脚本示例演示如何对指向同一虚拟机的多个网站进行负载均衡
services: load-balancer
documentationcenter: load-balancer
author: rockboyfor
manager: digimobile
editor: tysonn
tags: ''
ms.assetid: ''
ms.service: load-balancer
ms.devlang: powershell
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: infrastructure
origin.date: 04/20/2018
ms.date: 06/18/2018
ms.author: v-yeche
ms.openlocfilehash: d58e671509b8f78a3666df7de6519ed5037b27da
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52644161"
---
# <a name="azure-powershell-script-example-load-balance-multiple-websites"></a>Azure PowerShell 脚本示例：对多个网站进行负载均衡

本 Azure PowerShell 脚本示例使用可用性集中的两个虚拟机 (VM) 创建虚拟网络。 负载均衡器会将两个单独 IP 地址的流量定向到两台 VM。 运行脚本后，可将 Web 服务器软件部署到 VM 上，并可承载多个网站，其中每个网站都有其自身的 IP 地址。

必要时，请使用 [Azure PowerShell 指南](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/)中的说明安装 Azure PowerShell，并运行 `Login-AzureRmAccount -EnvironmentName AzureChinaCloud` 创建与 Azure 的连接。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>示例脚本

```powershell
# Variables for common values
$rgName='MyResourceGroup'
$location='chinaeast'

# Create user object
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

# Create a resource group.
New-AzureRmResourceGroup -Name $rgName -Location $location

# Create an availability set for the two VMs that host both websites.
$as = New-AzureRmAvailabilitySet -ResourceGroupName $rgName -Location $location `
  -Name MyAvailabilitySet -Sku Aligned -PlatformFaultDomainCount 2 -PlatformUpdateDomainCount 2

# Create a virtual network and a subnet.
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'MySubnet' -AddressPrefix 10.0.0.0/24

$vnet = New-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name MyVnet `
  -AddressPrefix 10.0.0.0/16 -Location $location -Subnet $subnet

# Create three public IP addresses; one for the load balancer and two for the front-end IP configurations.
$publicIpLB = New-AzureRmPublicIpAddress -ResourceGroupName $rgName -Name 'MyPublicIp-LoadBalancer' `
  -Location $location -AllocationMethod Dynamic

$publicIpContoso = New-AzureRmPublicIpAddress -ResourceGroupName $rgName -Name 'MyPublicIp-Contoso' `
  -Location $location -AllocationMethod Dynamic

$publicIpFabrikam = New-AzureRmPublicIpAddress -ResourceGroupName $rgName -Name 'MyPublicIp-Fabrikam' `
  -Location $location -AllocationMethod Dynamic

# Create two front-end IP configurations for both web sites.
$feipcontoso = New-AzureRmLoadBalancerFrontendIpConfig -Name 'FeContoso' -PublicIpAddress $publicIpContoso
$feipfabrikam = New-AzureRmLoadBalancerFrontendIpConfig -Name 'FeFabrikam' -PublicIpAddress $publicIpFabrikam

# Create the back-end address pools.
$bepoolContoso = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name 'BeContoso'
$bepoolFabrikam = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name 'BeFabrikam'

# Create a probe on port 80.
$probe = New-AzureRmLoadBalancerProbeConfig -Name 'MyProbe' -Protocol Http -Port 80 `
  -RequestPath / -IntervalInSeconds 360 -ProbeCount 5

# Create the load balancing rules.
$contosorule = New-AzureRmLoadBalancerRuleConfig -Name 'LBRuleContoso' -Protocol Tcp `
  -Probe $probe -FrontendPort 5000 -BackendPort 5000 `
  -FrontendIpConfiguration $feipContoso -BackendAddressPool $bePoolContoso

$fabrikamrule = New-AzureRmLoadBalancerRuleConfig -Name 'LBRuleFabrikam' -Protocol Tcp `
  -Probe $probe -FrontendPort 5000 -BackendPort 5000 `
  -FrontendIpConfiguration $feipFabrikam -BackendAddressPool $bePoolfabrikam

# Create a load balancer.
$lb = New-AzureRmLoadBalancer -ResourceGroupName $rgName -Name 'MyLoadBalancer' -Location $location `
  -FrontendIpConfiguration $feipcontoso,$feipfabrikam -BackendAddressPool $bepoolContoso,$bepoolfabrikam `
  -Probe $probe -LoadBalancingRule $contosorule,$fabrikamrule

# ############## VM1 ###############

# Create an Public IP for the first VM.
$publicipvm1 = New-AzureRmPublicIpAddress -ResourceGroupName $rgName -Name MyPublicIp-Vm1 `
  -location $location -AllocationMethod Dynamic

# Create IP configurations for Contoso and Fabrikam.
$ipconfig1 = New-AzureRmNetworkInterfaceIpConfig -Name 'ipconfig1' `
  -Subnet $vnet.subnets[0] -Primary  

$ipconfig2 = New-AzureRmNetworkInterfaceIpConfig -Name 'ipconfig2' `
  -Subnet $vnet.Subnets[0] -LoadBalancerBackendAddressPool $bepoolContoso

$ipconfig3 = New-AzureRmNetworkInterfaceIpConfig -Name 'ipconfig3' `
  -Subnet $vnet.Subnets[0] -LoadBalancerBackendAddressPool $bepoolfabrikam 

# Create a network interface for VM1.
$nicVM1 = New-AzureRmNetworkInterface -ResourceGroupName $rgName -Location $location `
-Name 'MyNic-VM1' -IpConfiguration $ipconfig1, $ipconfig2, $ipconfig3

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName 'myVM1' -VMSize Standard_DS2 -AvailabilitySetId $as.Id | `
  Set-AzureRmVMOperatingSystem -Windows -ComputerName 'myVM1' -Credential $cred | `
  Set-AzureRmVMSourceImage -PublisherName 'MicrosoftWindowsServer' -Offer 'WindowsServer' `
  -Skus '2016-Datacenter' -Version latest | Add-AzureRmVMNetworkInterface -Id $nicVM1.Id

# Create a virtual machine
$vm = New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vmConfig

############### VM2 ###############

# Create an Public IP for the second VM.

$publicipvm1 = New-AzureRmPublicIpAddress -ResourceGroupName $rgName -Name 'MyPublicIp-Vm2' `
  -location $location -AllocationMethod Dynamic

# Create IP configurations for Contoso and Fabrikam.
$ipconfig1 = New-AzureRmNetworkInterfaceIpConfig -Name 'ipconfig1' `
  -Subnet $vnet.subnets[0] -Primary  

$ipconfig2 = New-AzureRmNetworkInterfaceIpConfig -Name 'ipconfig2' `
  -Subnet $vnet.Subnets[0] -LoadBalancerBackendAddressPool $bepoolContoso 

$ipconfig3 = New-AzureRmNetworkInterfaceIpConfig -Name 'ipconfig3' `
  -Subnet $vnet.Subnets[0] -LoadBalancerBackendAddressPool $bepoolfabrikam 

# Create a network interface for VM2.
$nicVM2 = New-AzureRmNetworkInterface -ResourceGroupName $rgName -Location $location `
-Name 'MyNic-VM2' -IpConfiguration $ipconfig1, $ipconfig2, $ipconfig3

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName 'myVM2' -VMSize Standard_DS2 -AvailabilitySetId $as.Id | `
  Set-AzureRmVMOperatingSystem -Windows -ComputerName 'myVM2' -Credential $cred | `
  Set-AzureRmVMSourceImage -PublisherName 'MicrosoftWindowsServer' -Offer 'WindowsServer' `
  -Skus '2016-Datacenter' -Version latest | Add-AzureRmVMNetworkInterface -Id $nicVM2.Id

# Create a virtual machine
$vm = New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vmConfig

```

## <a name="clean-up-deployment"></a>清理部署 

运行以下命令来删除资源组、VM 和所有相关资源。

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建资源组、虚拟网络、负载均衡器和所有相关资源。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup) | 创建用于存储所有资源的资源组。 |
| [New-AzureRmAvailabilitySet](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermavailabilityset) | 创建 Azure 可用性集以提供高可用性。 |
| [New-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | 创建子网配置。 在虚拟网络创建过程中会使用此配置。 |
| [New-AzureRmVirtualNetwork](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetwork) | 创建虚拟网络。 |
| [New-AzureRmPublicIpAddress](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermpublicipaddress) | 创建公共 IP 地址。 |
| [New-AzureRmLoadBalancerFrontendIpConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig) | 创建负载均衡器的前端 IP 配置。 |
| [New-AzureRmLoadBalancerBackendAddressPoolConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig) | 创建负载均衡器的后端地址池配置。 |
| [New-AzureRmLoadBalancerProbeConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermloadbalancerprobeconfig) | 创建 NLB 探测。 NLB 探测用于监视 NLB 集中的每个 VM。 如果任何 VM 无法访问，流量不会路由到该 VM。 |
| [New-AzureRmLoadBalancerRuleConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermloadbalancerruleconfig) | 创建 NLB 规则。 在此示例中，为端口 80 创建一个规则。 当 HTTP 流量到达 NLB 时，它会路由到 NLB 集中的一个 VM 的端口 80。 |
| [New-AzureRmLoadBalancer](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermloadbalancer) | 创建负载均衡器。 |
| [New-AzureRmNetworkInterfaceIpConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermnetworkinterfaceipconfig) | 定义虚拟网络接口的高级功能。 |
| [New-AzureRmNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermnetworkinterface) | 创建网络接口。 |
| [New-AzureRmVMConfig](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvmconfig) | 创建 VM 配置。 此配置包括 VM 名称、操作系统和管理凭据等信息。 在创建 VM 期间使用此配置。 |
| [New-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvm) | 创建虚拟机。 |
|[Remove-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/remove-azurermresourcegroup) | 删除资源组及其中包含的所有资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可在 [Azure 网络概述文档](../powershell-samples.md?toc=%2fnetworking%2ftoc.json)中找到其他网络 PowerShell 脚本示例。
<!-- Update_Description: new articles on load balancer windows powershell load balance with multiple webists vm -->
<!--ms.date: 06/18/2018-->