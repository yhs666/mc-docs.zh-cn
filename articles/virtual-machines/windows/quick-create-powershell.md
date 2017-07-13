---
title: "Azure 快速入门 - 创建 Windows VM PowerShell | Azure"
description: "快速了解如何使用 PowerShell 创建 Windows 虚拟机"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
origin.date: 05/02/2017
ms.date: 07/03/2017
ms.author: v-dazen
ms.custom: mvc
ms.openlocfilehash: 017b178ee0766d199361f0fadc9b4d424d3cadc4
ms.sourcegitcommit: b3e981fc35408835936113e2e22a0102a2028ca0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/30/2017
---
# 使用 PowerShell 创建 Windows 虚拟机
<a id="create-a-windows-virtual-machine-with-powershell" class="xliff"></a>

Azure PowerShell 模块用于从 PowerShell 命令行或脚本创建和管理 Azure 资源。 本指南详细说明了如何使用 PowerShell 创建运行 Windows Server 2016 的 Azure 虚拟机。 部署完成后，我们将连接到服务器并安装 IIS。  

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial/?WT.mc_id=A261C142F)。

本快速入门需要 Azure PowerShell 模块 3.6 或更高版本。 运行 ` Get-Module -ListAvailable AzureRM` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)。

## 登录 Azure
<a id="log-in-to-azure" class="xliff"></a>

使用 `Login-AzureRmAccount` 命令登录到 Azure 订阅，并按照屏幕上的说明进行操作。

```powershell
Login-AzureRmAccount -EnvironmentName AzureChinaCloud
```

## 创建资源组
<a id="create-resource-group" class="xliff"></a>

使用 [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup) 创建 Azure 资源组。 资源组是在其中部署和管理 Azure 资源的逻辑容器。 

```powershell
New-AzureRmResourceGroup -Name myResourceGroup -Location ChinaEast
```

## 创建网络资源
<a id="create-networking-resources" class="xliff"></a>

### 创建虚拟网络、子网和公共 IP 地址。
<a id="create-a-virtual-network-subnet-and-a-public-ip-address" class="xliff"></a> 
这些资源用来与虚拟机建立网络连接，以及连接到 Internet。

```powershell
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork -ResourceGroupName myResourceGroup -Location ChinaEast `
    -Name MYvNET -AddressPrefix 192.168.0.0/16 -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$pip = New-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup -Location ChinaEast `
    -AllocationMethod Static -IdleTimeoutInMinutes 4 -Name "mypublicdns$(Get-Random)"
```

### 创建网络安全组和网络安全组规则。
<a id="create-a-network-security-group-and-a-network-security-group-rule" class="xliff"></a> 
网络安全组使用入站和出站规则保护虚拟机。 在本例中，将为端口 3389 创建一个入站规则，该规则允许传入的远程桌面连接。 我们还需要为端口 80 创建入站规则，以允许传入的 Web 流量。

```powershell
# Create an inbound network security group rule for port 3389
$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleRDP  -Protocol Tcp `
    -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
    -DestinationPortRange 3389 -Access Allow

# Create an inbound network security group rule for port 80
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleWWW  -Protocol Tcp `
    -Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
    -DestinationPortRange 80 -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName myResourceGroup -Location ChinaEast `
    -Name myNetworkSecurityGroup -SecurityRules $nsgRuleRDP,$nsgRuleWeb
```

### 为虚拟机创建网卡。
<a id="create-a-network-card-for-the-virtual-machine" class="xliff"></a> 
使用 [New-AzureRmNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermnetworkinterface) 为虚拟机创建网卡。 网卡将虚拟机连接到子网、网络安全组和公共 IP 地址。

```powershell
# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface -Name myNic -ResourceGroupName myResourceGroup -Location ChinaEast `
    -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id
```

## 创建虚拟机
<a id="create-virtual-machine" class="xliff"></a>

创建虚拟机配置。 此配置包括部署虚拟机时使用的设置，例如虚拟机映像、大小和身份验证配置。 运行此步骤时，会提示你输入凭据。 你输入的值将配置为用于虚拟机的用户名和密码。

```powershell
# Define a credential object
$cred = Get-Credential

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName myVM -VMSize Standard_DS2 | `
    Set-AzureRmVMOperatingSystem -Windows -ComputerName myVM -Credential $cred | `
    Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer `
    -Skus 2016-Datacenter -Version latest | Add-AzureRmVMNetworkInterface -Id $nic.Id
```

使用 [New-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvm) 创建虚拟机。

```powershell
New-AzureRmVM -ResourceGroupName myResourceGroup -Location ChinaEast -VM $vmConfig
```

## 连接到虚拟机
<a id="connect-to-virtual-machine" class="xliff"></a>

在部署完成后，创建到虚拟机的远程桌面连接。

使用 [Get-AzureRmPublicIpAddress](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermpublicipaddress) 命令返回虚拟机的公共 IP 地址。 需记下此 IP 地址，以便在后续步骤中使用浏览器连接到它测试 Web 连接。

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup | Select IpAddress
```

使用以下命令创建与虚拟机的远程桌面会话。 将 IP 地址替换为虚拟机的 publicIPAddress。 出现提示时，输入创建虚拟机时使用的凭据。

```bash 
mstsc /v:<publicIpAddress>
```

## 通过 PowerShell 安装 IIS
<a id="install-iis-via-powershell" class="xliff"></a>

现已登录到 Azure VM，可以使用单行 PowerShell 安装 IIS，并启用本地防火墙规则以允许 Web 流量。 打开 PowerShell 提示符并运行以下命令：

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

## 查看 IIS 欢迎页
<a id="view-the-iis-welcome-page" class="xliff"></a>

IIS 已安装，并且现在已从 Internet 打开 VM 上的端口 80 - 你可以使用所选的 Web 浏览器查看默认的 IIS 欢迎页。 请务必使用前面记录的 *publicIpAddress* 访问默认页面。 

![IIS 默认站点](./media/quick-create-powershell/default-iis-website.png) 

## 清理资源
<a id="clean-up-resources" class="xliff"></a>

如果不再需要资源组、VM 和所有相关的资源，可以使用 [Remove-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/remove-azurermresourcegroup) 命令将其删除。

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## 后续步骤
<a id="next-steps" class="xliff"></a>

在本快速入门中，部署了一个简单的虚拟机、一条网络安全组规则，并安装了一个 Web 服务器。 若要深入了解 Azure 虚拟机，请继续学习 Windows VM 教程。

> [!div class="nextstepaction"]
> [Azure Windows 虚拟机教程](./tutorial-manage-vm.md)
