---
title: Azure PowerShell 脚本示例 - 为多层应用程序创建网络
description: Azure PowerShell 脚本示例 - 为多层应用程序创建虚拟网络。
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
ms.openlocfilehash: 146676686229e315ee0d9fe39b0834b44a011720
ms.sourcegitcommit: a46f12240aea05f253fb4445b5e88564a2a2a120
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/26/2018
ms.locfileid: "53785294"
---
# <a name="create-a-network-for-multi-tier-applications"></a>为多层应用程序创建网络

该脚本示例创建了包含前端和后端子网的虚拟网络。 传入前端子网的流量仅限 HTTP 和 SSH，而传入后端子网的流量限于 MySQL、端口 3306。 运行该脚本后，将具有两个虚拟机（在可向其中部署 Web 服务器和 MySQL 软件的每个子网中各具有一个虚拟机）。

必要时，请使用 [Azure PowerShell 指南](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/)中的说明安装 Azure PowerShell，并运行 `Connect-AzureRmAccount` 创建与 Azure 的连接。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>示例脚本


# <a name="variables-for-common-values"></a>常见值的变量
$rgName='MyResourceGroup' $location='chinaeast'

# <a name="create-user-object"></a>创建用户对象
$cred = Get-Credential -Message "输入用于虚拟机的用户名和密码。"

# <a name="create-a-resource-group"></a>创建资源组。
New-AzureRmResourceGroup -Name $rgName -Location $location

# <a name="create-a-virtual-network-with-a-front-end-subnet-and-back-end-subnet"></a>创建包含前端子网和后端子网的虚拟网络。
$fesubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'MySubnet-FrontEnd' -AddressPrefix '10.0.1.0/24' $besubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'MySubnet-BackEnd' -AddressPrefix '10.0.2.0/24' $vnet = New-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name 'MyVnet' -AddressPrefix '10.0.0.0/16' ` -Location $location -Subnet $fesubnet, $besubnet

# <a name="create-an-nsg-rule-to-allow-http-traffic-in-from-the-internet-to-the-front-end-subnet"></a>创建允许 HTTP 流量从 Internet 流入到前端子网的 NSG 规则。
$rule1 = New-AzureRmNetworkSecurityRuleConfig -Name 'Allow-HTTP-All' -Description '允许 HTTP' `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 ` -SourceAddressPrefix Internet -SourcePortRange * ` -DestinationAddressPrefix * -DestinationPortRange 80

# <a name="create-an-nsg-rule-to-allow-rdp-traffic-from-the-internet-to-the-front-end-subnet"></a>创建允许 RDP 流量从 Internet 流入到前端子网的 NSG 规则。
$rule2 = New-AzureRmNetworkSecurityRuleConfig -Name 'Allow-RDP-All' -Description "允许 RDP" `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 200 ` -SourceAddressPrefix Internet -SourcePortRange * ` -DestinationAddressPrefix * -DestinationPortRange 3389


# <a name="create-a-network-security-group-for-the-front-end-subnet"></a>为前端子网创建网络安全组。
$nsgfe = New-AzureRmNetworkSecurityGroup -ResourceGroupName $RgName -Location $location ` -Name 'MyNsg-FrontEnd' -SecurityRules $rule1,$rule2

# <a name="associate-the-front-end-nsg-to-the-front-end-subnet"></a>将前端 NSG 关联到前端子网。
Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name 'MySubnet-FrontEnd' ` -AddressPrefix '10.0.1.0/24' -NetworkSecurityGroup $nsgfe

# <a name="create-an-nsg-rule-to-allow-sql-traffic-from-the-front-end-subnet-to-the-back-end-subnet"></a>创建允许 SQL 流量从前端子网流入到后端子网的 NSG 规则。
$rule1 = New-AzureRmNetworkSecurityRuleConfig -Name 'Allow-SQL-FrontEnd' -Description "允许 SQL" `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 ` -SourceAddressPrefix '10.0.1.0/24' -SourcePortRange * ` -DestinationAddressPrefix * -DestinationPortRange 1433

# <a name="create-an-nsg-rule-to-allow-rdp-traffic-from-the-internet-to-the-back-end-subnet"></a>创建允许 RDP 流量从 Internet 流入到后端子网的 NSG 规则。
$rule2 = New-AzureRmNetworkSecurityRuleConfig -Name 'Allow-RDP-All' -Description "允许 RDP" `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 200 ` -SourceAddressPrefix Internet -SourcePortRange * ` -DestinationAddressPrefix * -DestinationPortRange 3389

# <a name="create-a-network-security-group-for-back-end-subnet"></a>为后端子网创建网络安全组。
$nsgbe = New-AzureRmNetworkSecurityGroup -ResourceGroupName $RgName -Location $location ` -Name "MyNsg-BackEnd" -SecurityRules $rule1,$rule2

# <a name="associate-the-back-end-nsg-to-the-back-end-subnet"></a>将后端 NSG 关联到后端子网
Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name 'MySubnet-BackEnd' ` -AddressPrefix '10.0.2.0/24' -NetworkSecurityGroup $nsgbe

# <a name="create-a-public-ip-address-for-the-web-server-vm"></a>为 Web 服务器 VM 创建一个公共 IP 地址。
$publicipvm1 = New-AzureRmPublicIpAddress -ResourceGroupName $rgName -Name 'MyPublicIp-Web' ` -location $location -AllocationMethod Dynamic

# <a name="create-a-nic-for-the-web-server-vm"></a>为 Web 服务器 VM 创建一个 NIC。
$nicVMweb = New-AzureRmNetworkInterface -ResourceGroupName $rgName -Location $location ` -Name 'MyNic-Web' -PublicIpAddress $publicipvm1 -NetworkSecurityGroup $nsgfe -Subnet $vnet.Subnets[0]

# <a name="create-a-web-server-vm-in-the-front-end-subnet"></a>在前端子网中创建 Web 服务器 VM
$vmConfig = New-AzureRmVMConfig -VMName 'MyVm-Web' -VMSize 'Standard_DS2' | `
  Set-AzureRmVMOperatingSystem -Windows -ComputerName 'MyVm-Web' -Credential $cred | ` Set-AzureRmVMSourceImage -PublisherName 'MicrosoftWindowsServer' -Offer 'WindowsServer' ` -Skus '2016-Datacenter' -Version latest | Add-AzureRmVMNetworkInterface -Id $nicVMweb.Id

$vmweb = New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vmConfig

# <a name="create-a-public-ip-address-for-the-sql-vm"></a>为 SQL VM 创建一个公共 IP 地址。
$publicipvm2 = New-AzureRmPublicIpAddress -ResourceGroupName $rgName -Name MyPublicIP-Sql ` -location $location -AllocationMethod Dynamic

# <a name="create-a-nic-for-the-sql-vm"></a>为 SQL VM 创建一个 NIC。
$nicVMsql = New-AzureRmNetworkInterface -ResourceGroupName $rgName -Location $location ` -Name MyNic-Sql -PublicIpAddress $publicipvm2 -NetworkSecurityGroup $nsgbe -Subnet $vnet.Subnets[1] 

# <a name="create-a-sql-vm-in-the-back-end-subnet"></a>在后端子网中创建 SQL VM。
$vmConfig = New-AzureRmVMConfig -VMName 'MyVm-Sql' -VMSize 'Standard_DS2' | `
  Set-AzureRmVMOperatingSystem -Windows -ComputerName 'MyVm-Sql' -Credential $cred | ` Set-AzureRmVMSourceImage -PublisherName 'MicrosoftSQLServer' -Offer 'SQL2016-WS2016' ` -Skus 'Web' -Version latest | Add-AzureRmVMNetworkInterface -Id $nicVMsql.Id

$vmsql = New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vmConfig

# <a name="create-an-nsg-rule-to-block-all-outbound-traffic-from-the-back-end-subnet-to-the-internet-must-be-done-after-vm-creation"></a>创建一项 NSG 规则，阻止从后端子网流向 Internet 的所有出站流量（必须在创建 VM 后完成）
$rule3 = New-AzureRmNetworkSecurityRuleConfig -Name 'Deny-Internet-All' -Description "拒绝 Internet 的所有流量" `
  -Access Deny -Protocol Tcp -Direction Outbound -Priority 300 ` -SourceAddressPrefix * -SourcePortRange * ` -DestinationAddressPrefix Internet -DestinationPortRange *

# <a name="add-nsg-rule-to-back-end-nsg"></a>向后端 NSG 添加 NSG 规则
$nsgbe.SecurityRules.add($rule3)

Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsgbe

## <a name="clean-up-deployment"></a>清理部署 

运行以下命令来删除资源组、VM 和所有相关资源。

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建资源组、虚拟网络和网络安全组。 表中的每条命令链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [New-AzureRmResourceGroup](https://docs.microsoft.com/zh-cn/powershell/module/azurerm.resources/new-azurermresourcegroup) | 创建用于存储所有资源的资源组。 |
| [New-AzureRmVirtualNetwork](https://docs.microsoft.com/zh-cn/powershell/module/azurerm.network/new-azurermvirtualnetwork) | 创建 Azure 虚拟网络和前端子网。 |
| [New-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/zh-cn/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | 创建后端子网。 |
| [New-AzureRmPublicIpAddress](https://docs.microsoft.com/zh-cn/powershell/module/azurerm.network/new-azurermpublicipaddress) | 创建用于从 Internet 访问 VM 的公共 IP 地址。 |
| [New-AzureRmNetworkInterface](https://docs.microsoft.com/zh-cn/powershell/module/azurerm.network/new-azurermnetworkinterface) | 创建虚拟网络接口，并将其附加到虚拟网络的前端和后端子网。 |
| [New-AzureRmNetworkSecurityGroup](https://docs.microsoft.com/zh-cn/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | 创建关联到前端和后端子网的网络安全组 (NSG)。 |
| [New-AzureRmNetworkSecurityRuleConfig](https://docs.microsoft.com/zh-cn/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) |创建 NSG 规则，允许或阻止特定子网的特定端口。 |
| [New-AzureRmVM](https://docs.microsoft.com/zh-cn/powershell/module/azurerm.compute/new-azurermvm) | 创建虚拟机，并将 NIC 附加到每个 VM。 此命令还指定要使用的虚拟机映像和管理凭据。 |
| [Remove-AzureRmResourceGroup](https://docs.microsoft.com/zh-cn/powershell/module/azurerm.resources/remove-azurermresourcegroup) | 删除资源组及其包含的所有资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可在 [Azure 网络概述文档](../powershell-samples.md?toc=%2fnetworking%2ftoc.json)中找到其他网络 PowerShell 脚本示例。
