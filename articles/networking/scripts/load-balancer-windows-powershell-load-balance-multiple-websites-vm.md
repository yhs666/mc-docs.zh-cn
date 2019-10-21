---
title: Azure PowerShell 脚本示例 - 使用 Azure PowerShell 对多个网站进行负载均衡 | Microsoft Docs
description: Azure PowerShell 脚本示例 - 对指向同一虚拟机的多个网站进行负载均衡
services: load-balancer
documentationcenter: load-balancer
author: georgewallace
manager: timlt
editor: tysonn
tags: ''
ms.assetid: ''
ms.service: load-balancer
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
origin.date: 05/16/2017
ms.date: 10/17/2019
ms.author: v-tawe
ms.openlocfilehash: a905ae6f9a691df22cb091f61b3867cf8e485f6f
ms.sourcegitcommit: c21b37e8a5e7f833b374d8260b11e2fb2f451782
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/18/2019
ms.locfileid: "72584093"
---
# <a name="load-balance-multiple-websites"></a>对多个网站进行负载均衡

此脚本示例会使用可用性集中的两台虚拟机 (VM) 创建虚拟网络。 负载均衡器会将两个单独 IP 地址的流量定向到两台 VM。 运行脚本后，可将 Web 服务器软件部署到 VM 上，并可承载多个网站，其中每个网站都有其自身的 IP 地址。

必要时，请使用 [Azure PowerShell 指南](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/)中的说明安装 Azure PowerShell，并运行 `Connect-AzAccount` 创建与 Azure 的连接。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>示例脚本

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

```powershell
# Variables for common values
$rgName='MyResourceGroup'
$location='chinaeast'

# Create user object
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

# Create a resource group.
New-AzResourceGroup -Name $rgName -Location $location

# Create an availability set for the two VMs that host both websites.
$as = New-AzAvailabilitySet -ResourceGroupName $rgName -Location $location `
  -Name MyAvailabilitySet -Sku Aligned -PlatformFaultDomainCount 2 -PlatformUpdateDomainCount 2

# Create a virtual network and a subnet.
$subnet = New-AzVirtualNetworkSubnetConfig -Name 'MySubnet' -AddressPrefix 10.0.0.0/24

$vnet = New-AzVirtualNetwork -ResourceGroupName $rgName -Name MyVnet `
  -AddressPrefix 10.0.0.0/16 -Location $location -Subnet $subnet

# Create three public IP addresses; one for the load balancer and two for the front-end IP configurations.
$publicIpLB = New-AzPublicIpAddress -ResourceGroupName $rgName -Name 'MyPublicIp-LoadBalancer' `
  -Location $location -AllocationMethod Dynamic

$publicIpContoso = New-AzPublicIpAddress -ResourceGroupName $rgName -Name 'MyPublicIp-Contoso' `
  -Location $location -AllocationMethod Dynamic

$publicIpFabrikam = New-AzPublicIpAddress -ResourceGroupName $rgName -Name 'MyPublicIp-Fabrikam' `
  -Location $location -AllocationMethod Dynamic

# Create two front-end IP configurations for both web sites.
$feipcontoso = New-AzLoadBalancerFrontendIpConfig -Name 'FeContoso' -PublicIpAddress $publicIpContoso
$feipfabrikam = New-AzLoadBalancerFrontendIpConfig -Name 'FeFabrikam' -PublicIpAddress $publicIpFabrikam

# Create the back-end address pools.
$bepoolContoso = New-AzLoadBalancerBackendAddressPoolConfig -Name 'BeContoso'
$bepoolFabrikam = New-AzLoadBalancerBackendAddressPoolConfig -Name 'BeFabrikam'

# Create a probe on port 80.
$probe = New-AzLoadBalancerProbeConfig -Name 'MyProbe' -Protocol Http -Port 80 `
  -RequestPath / -IntervalInSeconds 360 -ProbeCount 5

# Create the load balancing rules.
$contosorule = New-AzLoadBalancerRuleConfig -Name 'LBRuleContoso' -Protocol Tcp `
  -Probe $probe -FrontendPort 5000 -BackendPort 5000 `
  -FrontendIpConfiguration $feipContoso -BackendAddressPool $bePoolContoso

$fabrikamrule = New-AzLoadBalancerRuleConfig -Name 'LBRuleFabrikam' -Protocol Tcp `
  -Probe $probe -FrontendPort 5000 -BackendPort 5000 `
  -FrontendIpConfiguration $feipFabrikam -BackendAddressPool $bePoolfabrikam

# Create a load balancer.
$lb = New-AzLoadBalancer -ResourceGroupName $rgName -Name 'MyLoadBalancer' -Location $location `
  -FrontendIpConfiguration $feipcontoso,$feipfabrikam -BackendAddressPool $bepoolContoso,$bepoolfabrikam `
  -Probe $probe -LoadBalancingRule $contosorule,$fabrikamrule

# ############## VM1 ###############

# Create an Public IP for the first VM.
$publicipvm1 = New-AzPublicIpAddress -ResourceGroupName $rgName -Name MyPublicIp-Vm1 `
  -location $location -AllocationMethod Dynamic

# Create IP configurations for Contoso and Fabrikam.
$ipconfig1 = New-AzNetworkInterfaceIpConfig -Name 'ipconfig1' `
  -Subnet $vnet.subnets[0] -Primary  

$ipconfig2 = New-AzNetworkInterfaceIpConfig -Name 'ipconfig2' `
  -Subnet $vnet.Subnets[0] -LoadBalancerBackendAddressPool $bepoolContoso
 
$ipconfig3 = New-AzNetworkInterfaceIpConfig -Name 'ipconfig3' `
  -Subnet $vnet.Subnets[0] -LoadBalancerBackendAddressPool $bepoolfabrikam 

# Create a network interface for VM1.
$nicVM1 = New-AzNetworkInterface -ResourceGroupName $rgName -Location $location `
-Name 'MyNic-VM1' -IpConfiguration $ipconfig1, $ipconfig2, $ipconfig3

# Create a virtual machine configuration
$vmConfig = New-AzVMConfig -VMName 'myVM1' -VMSize Standard_DS2 -AvailabilitySetId $as.Id | `
  Set-AzureRmVMOperatingSystem -Windows -ComputerName 'myVM1' -Credential $cred | `
  Set-AzureRmVMSourceImage -PublisherName 'MicrosoftWindowsServer' -Offer 'WindowsServer' `
  -Skus '2016-Datacenter' -Version latest | Add-AzureRmVMNetworkInterface -Id $nicVM1.Id

# Create a virtual machine
$vm = New-AzVM -ResourceGroupName $rgName -Location $location -VM $vmConfig

###### ######### VM2 ###############

# Create an Public IP for the second VM.

$publicipvm1 = New-AzPublicIpAddress -ResourceGroupName $rgName -Name 'MyPublicIp-Vm2' `
  -location $location -AllocationMethod Dynamic

# Create IP configurations for Contoso and Fabrikam.
$ipconfig1 = New-AzNetworkInterfaceIpConfig -Name 'ipconfig1' `
  -Subnet $vnet.subnets[0] -Primary  

$ipconfig2 = New-AzNetworkInterfaceIpConfig -Name 'ipconfig2' `
  -Subnet $vnet.Subnets[0] -LoadBalancerBackendAddressPool $bepoolContoso 

$ipconfig3 = New-AzNetworkInterfaceIpConfig -Name 'ipconfig3' `
  -Subnet $vnet.Subnets[0] -LoadBalancerBackendAddressPool $bepoolfabrikam 

# Create a network interface for VM2.
$nicVM2 = New-AzNetworkInterface -ResourceGroupName $rgName -Location $location `
-Name 'MyNic-VM2' -IpConfiguration $ipconfig1, $ipconfig2, $ipconfig3

# Create a virtual machine configuration
$vmConfig = New-AzVMConfig -VMName 'myVM2' -VMSize Standard_DS2 -AvailabilitySetId $as.Id | `
  Set-AzureRmVMOperatingSystem -Windows -ComputerName 'myVM2' -Credential $cred | `
  Set-AzureRmVMSourceImage -PublisherName 'MicrosoftWindowsServer' -Offer 'WindowsServer' `
  -Skus '2016-Datacenter' -Version latest | Add-AzureRmVMNetworkInterface -Id $nicVM2.Id

# Create a virtual machine
$vm = New-AzVM -ResourceGroupName $rgName -Location $location -VM $vmConfig
```

## <a name="clean-up-deployment"></a>清理部署 

运行以下命令来删除资源组、VM 和所有相关资源。

```powershell
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建资源组、虚拟网络、负载均衡器和所有相关资源。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) | 创建用于存储所有资源的资源组。 |
| [New-AzAvailabilitySet](https://docs.microsoft.com/powershell/module/az.compute/new-azavailabilityset) | 创建 Azure 可用性集以提供高可用性。 |
| [New-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetworksubnetconfig) | 创建子网配置。 在虚拟网络创建过程中会使用此配置。 |
| [New-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetwork) | 创建虚拟网络。 |
| [New-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipaddress) | 创建公共 IP 地址。 |
| [New-AzLoadBalancerFrontendIpConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerfrontendipconfig) | 创建负载均衡器的前端 IP 配置。 |
| [New-AzLoadBalancerBackendAddressPoolConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerbackendaddresspoolconfig) | 创建负载均衡器的后端地址池配置。 |
| [New-AzLoadBalancerProbeConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerprobeconfig) | 创建 NLB 探测。 NLB 探测用于监视 NLB 集中的每个 VM。 如果任何 VM 无法访问，流量不会路由到该 VM。 |
| [New-AzLoadBalancerRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerruleconfig) | 创建 NLB 规则。 在此示例中，为端口 80 创建一个规则。 当 HTTP 流量到达 NLB 时，它会路由到 NLB 集中的一个 VM 的端口 80。 |
| [New-AzLoadBalancer](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancer) | 创建负载均衡器。 |
| [New-AzNetworkInterfaceIpConfig](https://docs.microsoft.com/powershell/module/az.network/new-aznetworkinterfaceipconfig) | 定义虚拟网络接口的高级功能。 |
| [New-AzNetworkInterface](https://docs.microsoft.com/powershell/module/az.network/new-aznetworkinterface) | 创建网络接口。 |
| [New-AzVMConfig](https://docs.microsoft.com/powershell/module/az.compute/new-azvmconfig) | 创建 VM 配置。 此配置包括 VM 名称、操作系统和管理凭据等信息。 在创建 VM 期间使用此配置。 |
| [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) | 创建虚拟机。 |
|[Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | 删除资源组及其中包含的所有资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可在 [Azure 网络概述文档](../powershell-samples.md?toc=%2fnetworking%2ftoc.json)中找到其他网络 PowerShell 脚本示例。
