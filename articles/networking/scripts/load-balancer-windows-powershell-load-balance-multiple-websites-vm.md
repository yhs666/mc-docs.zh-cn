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
ms.date: 03/11/2019
ms.author: v-biyu
ms.openlocfilehash: 82f9b607d7b423a19c2a4a6c072baa4133ae7020
ms.sourcegitcommit: df1adc5cce721db439c1a7af67f1b19280004b2d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2019
ms.locfileid: "65835742"
---
# <a name="load-balance-multiple-websites"></a>对多个网站进行负载均衡

此脚本示例会使用可用性集中的两台虚拟机 (VM) 创建虚拟网络。 负载均衡器会将两个单独 IP 地址的流量定向到两台 VM。 运行脚本后，可将 Web 服务器软件部署到 VM 上，并可承载多个网站，其中每个网站都有其自身的 IP 地址。

必要时，请使用 [Azure PowerShell 指南](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/)中的说明安装 Azure PowerShell，并运行 `Connect-AzAccount` 创建与 Azure 的连接。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>示例脚本

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

# <a name="variables-for-common-values"></a>常见值的变量
$rgName='MyResourceGroup' $location='chinaeast'

# <a name="create-user-object"></a>创建用户对象
$cred = Get-Credential -Message "输入用于虚拟机的用户名和密码。"

# <a name="create-a-resource-group"></a>创建资源组。
New-AzResourceGroup -Name $rgName -Location $location

# <a name="create-an-availability-set-for-the-two-vms-that-host-both-websites"></a>为这两个托管了两个网站的 VM 创建可用性集。
$as = New-AzAvailabilitySet -ResourceGroupName $rgName -Location $location ` -Name MyAvailabilitySet -Sku Aligned -PlatformFaultDomainCount 2 -PlatformUpdateDomainCount 2

# <a name="create-a-virtual-network-and-a-subnet"></a>创建虚拟网络和子网。
$subnet = New-AzVirtualNetworkSubnetConfig -Name 'MySubnet' -AddressPrefix 10.0.0.0/24

$vnet = New-AzVirtualNetwork -ResourceGroupName $rgName -Name MyVnet ` -AddressPrefix 10.0.0.0/16 -Location $location -Subnet $subnet

# <a name="create-three-public-ip-addresses-one-for-the-load-balancer-and-two-for-the-front-end-ip-configurations"></a>创建三个公共 IP 地址，一个用于负载均衡器，两个用于前端 IP 配置。
$publicIpLB = New-AzPublicIpAddress -ResourceGroupName $rgName -Name 'MyPublicIp-LoadBalancer' ` -Location $location -AllocationMethod Dynamic

$publicIpContoso = New-AzPublicIpAddress -ResourceGroupName $rgName -Name 'MyPublicIp-Contoso' ` -Location $location -AllocationMethod Dynamic

$publicIpFabrikam = New-AzPublicIpAddress -ResourceGroupName $rgName -Name 'MyPublicIp-Fabrikam' ` -Location $location -AllocationMethod Dynamic

# <a name="create-two-front-end-ip-configurations-for-both-web-sites"></a>为两个网站创建两个前端 IP 配置。
$feipcontoso = New-AzLoadBalancerFrontendIpConfig -Name 'FeContoso' -PublicIpAddress $publicIpContoso $feipfabrikam = New-AzLoadBalancerFrontendIpConfig -Name 'FeFabrikam' -PublicIpAddress $publicIpFabrikam

# <a name="create-the-back-end-address-pools"></a>创建后端地址池。
$bepoolContoso = New-AzLoadBalancerBackendAddressPoolConfig -Name 'BeContoso' $bepoolFabrikam = New-AzLoadBalancerBackendAddressPoolConfig -Name 'BeFabrikam'

# <a name="create-a-probe-on-port-80"></a>在端口 80 上创建探测。
$probe = New-AzLoadBalancerProbeConfig -Name 'MyProbe' -Protocol Http -Port 80 ` -RequestPath / -IntervalInSeconds 360 -ProbeCount 5

# <a name="create-the-load-balancing-rules"></a>创建负载均衡规则。
$contosorule = New-AzLoadBalancerRuleConfig -Name 'LBRuleContoso' -Protocol Tcp `
  -Probe $probe -FrontendPort 5000 -BackendPort 5000 ` -FrontendIpConfiguration $feipContoso -BackendAddressPool $bePoolContoso

$fabrikamrule = New-AzLoadBalancerRuleConfig -Name 'LBRuleFabrikam' -Protocol Tcp `
  -Probe $probe -FrontendPort 5000 -BackendPort 5000 ` -FrontendIpConfiguration $feipFabrikam -BackendAddressPool $bePoolfabrikam

# <a name="create-a-load-balancer"></a>创建负载均衡器。
$lb = New-AzLoadBalancer -ResourceGroupName $rgName -Name 'MyLoadBalancer' -Location $location `
  -FrontendIpConfiguration $feipcontoso,$feipfabrikam -BackendAddressPool $bepoolContoso,$bepoolfabrikam ` -Probe $probe -LoadBalancingRule $contosorule,$fabrikamrule

# <a name="-vm1"></a>############## VM1 ###############

# <a name="create-an-public-ip-for-the-first-vm"></a>为第一个 VM 创建公共 IP。
$publicipvm1 = New-AzPublicIpAddress -ResourceGroupName $rgName -Name MyPublicIp-Vm1 ` -location $location -AllocationMethod Dynamic

# <a name="create-ip-configurations-for-contoso-and-fabrikam"></a>为 Contoso 和 Fabrikam 创建 IP 配置。
$ipconfig1 = New-AzNetworkInterfaceIpConfig -Name 'ipconfig1' ` -Subnet $vnet.subnets[0] -Primary  

$ipconfig2 = New-AzNetworkInterfaceIpConfig -Name 'ipconfig2' ` -Subnet $vnet.Subnets[0] -LoadBalancerBackendAddressPool $bepoolContoso
 
$ipconfig3 = New-AzNetworkInterfaceIpConfig -Name 'ipconfig3' ` -Subnet $vnet.Subnets[0] -LoadBalancerBackendAddressPool $bepoolfabrikam 

# <a name="create-a-network-interface-for-vm1"></a>为 VM1 创建网络接口。
$nicVM1 = New-AzNetworkInterface -ResourceGroupName $rgName -Location $location ` -Name 'MyNic-VM1' -IpConfiguration $ipconfig1, $ipconfig2, $ipconfig3

# <a name="create-a-virtual-machine-configuration"></a>创建虚拟机配置
$vmConfig = New-AzVMConfig -VMName 'myVM1' -VMSize Standard_DS2 -AvailabilitySetId $as.Id | `
  Set-AzureRmVMOperatingSystem -Windows -ComputerName 'myVM1' -Credential $cred | ` Set-AzureRmVMSourceImage -PublisherName 'MicrosoftWindowsServer' -Offer 'WindowsServer' ` -Skus '2016-Datacenter' -Version latest | Add-AzureRmVMNetworkInterface -Id $nicVM1.Id

# <a name="create-a-virtual-machine"></a>创建虚拟机
$vm = New-AzVM -ResourceGroupName $rgName -Location $location -VM $vmConfig

###### <a name="-vm2"></a>######### VM2 ###############

# <a name="create-an-public-ip-for-the-second-vm"></a>为第二个 VM 创建公共 IP。

$publicipvm1 = New-AzPublicIpAddress -ResourceGroupName $rgName -Name 'MyPublicIp-Vm2' ` -location $location -AllocationMethod Dynamic

# <a name="create-ip-configurations-for-contoso-and-fabrikam"></a>为 Contoso 和 Fabrikam 创建 IP 配置。
$ipconfig1 = New-AzNetworkInterfaceIpConfig -Name 'ipconfig1' ` -Subnet $vnet.subnets[0] -Primary  

$ipconfig2 = New-AzNetworkInterfaceIpConfig -Name 'ipconfig2' ` -Subnet $vnet.Subnets[0] -LoadBalancerBackendAddressPool $bepoolContoso 

$ipconfig3 = New-AzNetworkInterfaceIpConfig -Name 'ipconfig3' ` -Subnet $vnet.Subnets[0] -LoadBalancerBackendAddressPool $bepoolfabrikam 

# <a name="create-a-network-interface-for-vm2"></a>为 VM2 创建网络接口。
$nicVM2 = New-AzNetworkInterface -ResourceGroupName $rgName -Location $location ` -Name 'MyNic-VM2' -IpConfiguration $ipconfig1, $ipconfig2, $ipconfig3

# <a name="create-a-virtual-machine-configuration"></a>创建虚拟机配置
$vmConfig = New-AzVMConfig -VMName 'myVM2' -VMSize Standard_DS2 -AvailabilitySetId $as.Id | `
  Set-AzureRmVMOperatingSystem -Windows -ComputerName 'myVM2' -Credential $cred | ` Set-AzureRmVMSourceImage -PublisherName 'MicrosoftWindowsServer' -Offer 'WindowsServer' ` -Skus '2016-Datacenter' -Version latest | Add-AzureRmVMNetworkInterface -Id $nicVM2.Id

# <a name="create-a-virtual-machine"></a>创建虚拟机
$vm = New-AzVM -ResourceGroupName $rgName -Location $location -VM $vmConfig

## <a name="clean-up-deployment"></a>清理部署 

运行以下命令来删除资源组、VM 和所有相关资源。

```powershell
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建资源组、虚拟网络、负载均衡器和所有相关资源。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [New-AzResourceGroup](https://docs.microsoft.com/zh-cn/powershell/module/az.resources/new-azresourcegroup) | 创建用于存储所有资源的资源组。 |
| [New-AzAvailabilitySet](https://docs.microsoft.com/zh-cn/powershell/module/az.compute/new-azavailabilityset) | 创建 Azure 可用性集以提供高可用性。 |
| [New-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/zh-cn/powershell/module/az.network/new-azvirtualnetworksubnetconfig) | 创建子网配置。 在虚拟网络创建过程中会使用此配置。 |
| [New-AzVirtualNetwork](https://docs.microsoft.com/zh-cn/powershell/module/az.network/new-azvirtualnetwork) | 创建虚拟网络。 |
| [New-AzPublicIpAddress](https://docs.microsoft.com/zh-cn/powershell/module/az.network/new-azpublicipaddress) | 创建公共 IP 地址。 |
| [New-AzLoadBalancerFrontendIpConfig](https://docs.microsoft.com/zh-cn/powershell/module/az.network/new-azloadbalancerfrontendipconfig) | 创建负载均衡器的前端 IP 配置。 |
| [New-AzLoadBalancerBackendAddressPoolConfig](https://docs.microsoft.com/zh-cn/powershell/module/az.network/new-azloadbalancerbackendaddresspoolconfig) | 创建负载均衡器的后端地址池配置。 |
| [New-AzLoadBalancerProbeConfig](https://docs.microsoft.com/zh-cn/powershell/module/az.network/new-azloadbalancerprobeconfig) | 创建 NLB 探测。 NLB 探测用于监视 NLB 集中的每个 VM。 如果任何 VM 无法访问，流量不会路由到该 VM。 |
| [New-AzLoadBalancerRuleConfig](https://docs.microsoft.com/zh-cn/powershell/module/az.network/new-azloadbalancerruleconfig) | 创建 NLB 规则。 在此示例中，为端口 80 创建一个规则。 当 HTTP 流量到达 NLB 时，它会路由到 NLB 集中的一个 VM 的端口 80。 |
| [New-AzLoadBalancer](https://docs.microsoft.com/zh-cn/powershell/module/az.network/new-azloadbalancer) | 创建负载均衡器。 |
| [New-AzNetworkInterfaceIpConfig](https://docs.microsoft.com/zh-cn/powershell/module/az.network/new-aznetworkinterfaceipconfig) | 定义虚拟网络接口的高级功能。 |
| [New-AzNetworkInterface](https://docs.microsoft.com/zh-cn/powershell/module/az.network/new-aznetworkinterface) | 创建网络接口。 |
| [New-AzVMConfig](https://docs.microsoft.com/zh-cn/powershell/module/az.compute/new-azvmconfig) | 创建 VM 配置。 此配置包括 VM 名称、操作系统和管理凭据等信息。 在创建 VM 期间使用此配置。 |
| [New-AzVM](https://docs.microsoft.com/zh-cn/powershell/module/az.compute/new-azvm) | 创建虚拟机。 |
|[Remove-AzResourceGroup](https://docs.microsoft.com/zh-cn/powershell/module/az.resources/remove-azresourcegroup) | 删除资源组及其中包含的所有资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可在 [Azure 网络概述文档](../powershell-samples.md?toc=%2fnetworking%2ftoc.json)中找到其他网络 PowerShell 脚本示例。
