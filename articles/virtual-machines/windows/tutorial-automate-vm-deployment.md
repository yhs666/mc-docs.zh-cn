---
title: "在 Azure 中自定义 Windows VM | Azure"
description: "了解如何在 Azure 中使用自定义脚本扩展和 Key Vault 来自定义 Windows VM"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
origin.date: 05/02/2017
ms.date: 07/03/2017
ms.author: v-dazen
ms.custom: mvc
ms.openlocfilehash: f64e63a86c01aa2e8c163c1e18acf83157e92b31
ms.sourcegitcommit: b1d2bd71aaff7020dfb3f7874799e03df3657cd4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
# 如何在 Azure 中自定义 Windows 虚拟机
<a id="how-to-customize-a-windows-virtual-machine-in-azure" class="xliff"></a>
若要以快速一致的方式配置虚拟机 (VM)，通常需要某种形式的自动化。 自定义 Windows VM 的一种常用方法是使用[适用于 Windows 的自定义脚本扩展](extensions-customscript.md)。 本教程介绍如何执行下列操作：

> [!div class="checklist"]
> * 使用自定义脚本扩展安装 IIS
> * 创建使用自定义脚本扩展的 VM
> * 在应用扩展后查看正在运行的 IIS 站点

本教程需要 Azure PowerShell 模块 3.6 或更高版本。 运行 ` Get-Module -ListAvailable AzureRM` 即可查找版本。 如果需要进行升级，请参阅 [Install Azure PowerShell module](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)（安装 Azure PowerShell 模块）。

## 自定义脚本扩展概述
<a id="custom-script-extension-overview" class="xliff"></a>
自定义脚本扩展在 Azure VM 上下载和执行脚本。 此扩展适用于部署后配置、软件安装或其他任何配置/管理任务。 可以从 Azure 存储或 GitHub 下载脚本，或者在扩展运行时将脚本提供给 Azure 门户。

自定义脚本扩展与 Azure Resource Manager 模板集成，也可以使用 Azure CLI、PowerShell、Azure 门户或 Azure 虚拟机 REST API 来运行它。

自定义脚本扩展适用于 Windows 和 Linux VM。

## 创建虚拟机
<a id="create-virtual-machine" class="xliff"></a>
创建 VM 之前，需使用 [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup) 创建资源组。 以下示例在“ChinaEast”位置创建名为“myResourceGroupAutomate”的资源组：

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroupAutomate -Location ChinaEast
```

使用 [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) 设置 VM 的管理员用户名和密码：

```powershell
$cred = Get-Credential
```

现在，可使用 [New-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvm) 创建 VM。 以下示例创建所需的虚拟网络组件、OS 配置，然后创建名为“myVM”的 VM：

```powershell
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork `
    -ResourceGroupName myResourceGroupAutomate `
    -Location ChinaEast `
    -Name myVnet `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$publicIP = New-AzureRmPublicIpAddress `
    -ResourceGroupName myResourceGroupAutomate `
    -Location ChinaEast `
    -AllocationMethod Static `
    -IdleTimeoutInMinutes 4 `
    -Name "myPublicIP"

# Create an inbound network security group rule for port 3389
$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig `
    -Name myNetworkSecurityGroupRuleRDP  `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1000 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389 `
    -Access Allow

# Create an inbound network security group rule for port 80
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig `
    -Name myNetworkSecurityGroupRuleWWW  `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1001 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 80 `
    -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName myResourceGroupAutomate `
    -Location ChinaEast `
    -Name myNetworkSecurityGroup `
    -SecurityRules $nsgRuleRDP,$nsgRuleWeb

# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface `
    -Name myNic `
    -ResourceGroupName myResourceGroupAutomate `
    -Location ChinaEast `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $publicIP.Id `
    -NetworkSecurityGroupId $nsg.Id

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName myVM -VMSize Standard_DS2 | `
Set-AzureRmVMOperatingSystem -Windows -ComputerName myVM -Credential $cred | `
Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer -Skus 2016-Datacenter -Version latest | `
Add-AzureRmVMNetworkInterface -Id $nic.Id

New-AzureRmVM -ResourceGroupName myResourceGroupAutomate -Location ChinaEast -VM $vmConfig
```

创建资源和 VM 需要几分钟的时间。

## 自动安装 IIS
<a id="automate-iis-install" class="xliff"></a>
使用 [Set-AzureRmVMExtension](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmextension) 安装自定义脚本扩展。 该扩展运行 `powershell Add-WindowsFeature Web-Server` 以安装 IIS Web 服务器，然后更新 Default.htm 页以显示 VM 的主机名：

```powershell
Set-AzureRmVMExtension -ResourceGroupName myResourceGroupAutomate `
    -ExtensionName IIS `
    -VMName myVM `
    -Publisher Microsoft.Compute `
    -ExtensionType CustomScriptExtension `
    -TypeHandlerVersion 1.4 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
    -Location ChinaEast
```

## 测试网站
<a id="test-web-site" class="xliff"></a>
使用 [Get-AzureRmPublicIPAddress](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermpublicipaddress) 获取负载均衡器的公共 IP 地址。 以下示例获取前面创建的“myPublicIP”的 IP 地址：

```powershell
Get-AzureRmPublicIPAddress `
    -ResourceGroupName myResourceGroupAutomate `
    -Name myPublicIP | select IpAddress
```

然后，可将公共 IP 地址输入 Web 浏览器中。 网站随即显示，其中包括负载均衡器将流量分发到的 VM 的主机名，如下例所示：

![运行 IIS 网站](./media/tutorial-automate-vm-deployment/running-iis-website.png)

## 后续步骤
<a id="next-steps" class="xliff"></a>

在本教程中，你在 VM 上自动执行了 IIS 安装。 你已了解如何：

> [!div class="checklist"]
> * 使用自定义脚本扩展安装 IIS
> * 创建使用自定义脚本扩展的 VM
> * 在应用扩展后查看正在运行的 IIS 站点

转到下一教程，了解如何创建自定义 VM 映像。

> [!div class="nextstepaction"]
> [创建自定义 VM 映像](./tutorial-custom-images.md)
