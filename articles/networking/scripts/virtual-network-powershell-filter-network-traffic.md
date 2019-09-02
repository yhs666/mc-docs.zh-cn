---
title: Azure PowerShell 脚本示例 - 筛选 VM 网络流量
description: Azure PowerShell 脚本示例 - 筛选入站和出站 VM 网络流量。
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
ms.openlocfilehash: 8683ba5bcb4e1a97d260f563de1e0e4401e77aec
ms.sourcegitcommit: df1adc5cce721db439c1a7af67f1b19280004b2d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2019
ms.locfileid: "63827154"
---
# <a name="filter-inbound-and-outbound-vm-network-traffic"></a>筛选入站和出站 VM 网络流量

该脚本示例创建了包含前端和后端子网的虚拟网络。 前端子网的入站网络流量仅限于 HTTP 和 HTTPS，而从后端子网到 Internet 的出站流量则不受限制。 运行该脚本后，将具有一个包含两个 NIC 的虚拟机。 每个 NIC 连接到不同的子网。

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

# <a name="create-a-virtual-network-a-front-end-subnet-and-a-back-end-subnet"></a>创建一个虚拟网络、一个前端子网和一个后端子网。
$fesubnet = New-AzVirtualNetworkSubnetConfig -Name 'MySubnet-FrontEnd' -AddressPrefix '10.0.1.0/24' $besubnet = New-AzVirtualNetworkSubnetConfig -Name 'MySubnet-BackEnd' -AddressPrefix '10.0.2.0/24'

$vnet = New-AzVirtualNetwork -ResourceGroupName $rgName -Name 'MyVnet' -AddressPrefix '10.0.0.0/16' ` -Location $location -Subnet $fesubnet, $besubnet

# <a name="create-nsg-rules-to-allow-http--https-traffic-inbound"></a>创建允许 HTTP 和 HTTPS 入站流量的 NSG 规则。
$rule1 = New-AzNetworkSecurityRuleConfig -Name 'Allow-HTTP-ALL' -Description 'Allow HTTP' `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 ` -SourceAddressPrefix Internet -SourcePortRange * ` -DestinationAddressPrefix * -DestinationPortRange 80

$rule2 = New-AzNetworkSecurityRuleConfig -Name 'Allow-HTTPS-All' -Description 'Allow HTTPS' `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 200 ` -SourceAddressPrefix Internet -SourcePortRange * ` -DestinationAddressPrefix * -DestinationPortRange 443

# <a name="create-an-nsg-rule-to-allow-rdp-traffic-in-from-the-internet-to-the-front-end-subnet"></a>创建允许 RDP 流量从 Internet 流入到前端子网的 NSG 规则。
$rule3 = New-AzNetworkSecurityRuleConfig -Name 'Allow-RDP-All' -Description 'Allow RDP' `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 300 ` -SourceAddressPrefix Internet -SourcePortRange * ` -DestinationAddressPrefix * -DestinationPortRange 3389

# <a name="create-a-network-security-group-nsg-for-the-front-end-subnet"></a>为前端子网创建网络安全组 (NSG)。
$nsg = New-AzNetworkSecurityGroup -ResourceGroupName $RgName -Location $location ` -Name "MyNsg-FrontEnd" -SecurityRules $rule1,$rule2,$rule3

# <a name="associate-the-front-end-nsg-to-the-front-end-subnet"></a>将前端 NSG 关联到前端子网。
Set-AzVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name 'MySubnet-FrontEnd' ` -AddressPrefix 10.0.1.0/24 -NetworkSecurityGroup $nsg

# <a name="create-an-nsg-rule-to-block-all-outbound-traffic-from-the-back-end-subnet-to-the-internet-inbound-blocked-by-default"></a>创建一项 NSG 规则，阻止从后端子网流向 Internet 的所有出站流量（默认情况下会阻止入站流量）。
$rule1 = New-AzNetworkSecurityRuleConfig -Name 'Deny-Internet-All' -Description "Deny all Internet" `
  -Access Allow -Protocol Tcp -Direction Outbound -Priority 100 ` -SourceAddressPrefix * -SourcePortRange * ` -DestinationAddressPrefix Internet -DestinationPortRange *

# <a name="create-a-network-security-group-for-the-back-end-subnet"></a>为后端子网创建网络安全组。
$nsg = New-AzNetworkSecurityGroup -ResourceGroupName $RgName -Location $location ` -Name "MyNsg-BackEnd" -SecurityRules $rule1

# <a name="associate-the-back-end-nsg-to-the-back-end-subnet"></a>将后端 NSG 关联到后端子网。
Set-AzVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name 'MySubnet-backEnd' ` -AddressPrefix 10.0.2.0/24 -NetworkSecurityGroup $nsg

# <a name="create-a-public-ip-address-for-the-vm-front-end-network-interface"></a>为 VM 前端网络接口创建公共 IP 地址。
$publicipvm = New-AzPublicIpAddress -ResourceGroupName $rgName -Name 'MyPublicIp-FrontEnd' ` -location $location -AllocationMethod Dynamic

# <a name="create-a-network-interface-for-the-vm-attached-to-the-front-end-subnet"></a>为附加到前端子网的 VM 创建网络接口。
$nicVMfe = New-AzNetworkInterface -ResourceGroupName $rgName -Location $location ` -Name MyNic-FrontEnd -PublicIpAddress $publicipvm -Subnet $vnet.Subnets[0]

# <a name="create-a-network-interface-for-the-vm-attached-to-the-back-end-subnet"></a>为附加到后端子网的 VM 创建网络接口。
$nicVMbe = New-AzNetworkInterface -ResourceGroupName $rgName -Location $location ` -Name MyNic-BackEnd -Subnet $vnet.Subnets[1]

# <a name="create-the-vm-with-both-the-frontend-and-backend-nics"></a>创建带 FrontEnd NIC 和 BackEnd NIC 的 VM。
$vmConfig = New-AzVMConfig -VMName 'myVM' -VMSize Standard_DS2 | `
  Set-AzureRmVMOperatingSystem -Windows -ComputerName 'myVM' -Credential $cred | ` Set-AzureRmVMSourceImage -PublisherName 'MicrosoftWindowsServer' -Offer 'WindowsServer' ` -Skus '2016-Datacenter' -Version 'latest'
    
$vmconfig = Add-AzVMNetworkInterface -VM $vmConfig -id $nicVMfe.Id -Primary $vmconfig = Add-AzVMNetworkInterface -VM $vmConfig -id $nicVMbe.Id

# <a name="create-a-virtual-machine"></a>创建虚拟机
$vm = New-AzVM -ResourceGroupName $rgName -Location $location -VM $vmConfig

## <a name="clean-up-deployment"></a>清理部署 

运行以下命令来删除资源组、VM 和所有相关资源。

```powershell
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建资源组、虚拟网络和网络安全组。 表中的每条命令链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [New-AzResourceGroup](https://docs.microsoft.com/zh-cn/powershell/module/az.resources/new-azresourcegroup) | 创建用于存储所有资源的资源组。 |
| [New-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/zh-cn/powershell/module/az.network/new-azvirtualnetworksubnetconfig) | 创建子网配置对象 |
| [New-AzVirtualNetwork](https://docs.microsoft.com/zh-cn/powershell/module/az.network/new-azvirtualnetwork) | 创建 Azure 虚拟网络和前端子网。 |
| [New-AzNetworkSecurityRuleConfig](https://docs.microsoft.com/zh-cn/powershell/module/az.network/new-aznetworksecurityruleconfig) | 创建要分配到网络安全组的安全规则。 |
| [New-AzNetworkSecurityGroup](https://docs.microsoft.com/zh-cn/powershell/module/az.network/new-aznetworksecuritygroup) |创建 NSG 规则，允许或阻止特定子网的特定端口。 |
| [Set-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/zh-cn/powershell/module/az.network/set-azvirtualnetworksubnetconfig) | 将 NSG 关联到子网。 |
| [New-AzPublicIpAddress](https://docs.microsoft.com/zh-cn/powershell/module/az.network/new-azpublicipaddress) | 创建用于从 Internet 访问 VM 的公共 IP 地址。 |
| [New-AzNetworkInterface](https://docs.microsoft.com/zh-cn/powershell/module/az.network/new-aznetworkinterface) | 创建虚拟网络接口，并将其附加到虚拟网络的前端和后端子网。 |
| [New-AzVMConfig](https://docs.microsoft.com/zh-cn/powershell/module/az.compute/new-azvmconfig) | 创建 VM 配置。 此配置包括 VM 名称、操作系统和管理凭据等信息。 在创建 VM 期间使用此配置。 |
| [New-AzVM](https://docs.microsoft.com/zh-cn/powershell/module/az.compute/new-azvm) | 创建虚拟机。 |
|[Remove-AzResourceGroup](https://docs.microsoft.com/zh-cn/powershell/module/az.resources/remove-azresourcegroup) | 删除资源组及其中包含的所有资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可在 [Azure 网络概述文档](../powershell-samples.md?toc=%2fnetworking%2ftoc.json)中找到其他网络 PowerShell 脚本示例。
