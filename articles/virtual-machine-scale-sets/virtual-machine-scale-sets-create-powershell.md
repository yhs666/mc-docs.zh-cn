---
title: "使用 Azure PowerShell 创建虚拟机规模集 | Microsoft Docs"
description: "了解如何使用 Azure PowerShell 快速创建虚拟机规模集"
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: get-started-article
origin.date: 12/19/2017
ms.date: 01/29/2018
ms.author: v-junlch
ms.openlocfilehash: d2b25873f0a77183bbe099644e48be8b9a63484c
ms.sourcegitcommit: 3629fd4a81f66a7d87a4daa00471042d1f79c8bb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2018
---
# <a name="create-a-virtual-machine-scale-set-with-azure-powershell"></a>使用 Azure PowerShell 创建虚拟机规模集
利用虚拟机规模集，可以部署和管理一组相同的、自动缩放的虚拟机。 可以手动缩放规模集中的 VM 数，也可以定义规则，以便根据资源使用情况（如 CPU 使用率、内存需求或网络流量）进行自动缩放。 在此入门文章中，可以使用 Azure PowerShell 创建虚拟机规模集。 也可使用 [Azure CLI 2.0](virtual-machine-scale-sets-create-cli.md) 或 [Azure 门户](virtual-machine-scale-sets-create-portal.md)创建规模集。

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。

如果选择在本地安装并使用 PowerShell，则本教程需要 Azure PowerShell 模块 4.4.1 或更高版本。 运行 ` Get-Module -ListAvailable AzureRM` 即可查找版本。 如果需要进行升级，请参阅 [Install Azure PowerShell module](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)（安装 Azure PowerShell 模块）。 如果在本地运行 PowerShell，则还需运行 `Login-AzureRmAccount` 以创建与 Azure 的连接。


## <a name="create-a-resource-group"></a>创建资源组
创建规模集之前，需使用 [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup) 创建一个资源组。 以下示例在“chinanorth”位置创建名为“myResourceGroup”的资源组：

```azurepowershell
New-AzureRmResourceGroup -ResourceGroupName "myResourceGroup" -Location "chinanorth"
```


## <a name="create-load-balancer"></a>创建负载均衡器
规模集中的 VM 实例连接到负载均衡器。 Azure 负载均衡器是位于第 4 层（TCP、UDP）的负载均衡器，通过在正常运行的 VM 之间分发传入流量提供高可用性。 负载均衡器运行状况探测器监视每个 VM 上的给定端口，仅将流量分发给正常运行的 VM。

创建虚拟网络以及具有公共 IP 地址、在端口 80 上分发 Web 流量的负载均衡器。 若要创建这些资源，请复制并粘贴以下 PowerShell cmdlet：

```azurepowershell
# Create a virtual network subnet
$subnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name "mySubnet" `
  -AddressPrefix 10.0.0.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName "myResourceGroup" `
  -Name "myVnet" `
  -Location "chinanorth" `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $subnet

# Create a public IP address
$publicIP = New-AzureRmPublicIpAddress `
  -ResourceGroupName "myResourceGroup" `
  -Location "chinanorth" `
  -AllocationMethod Static `
  -Name "myPublicIP"

# Create a frontend and backend IP pool
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig `
  -Name "myFrontEndPool" `
  -PublicIpAddress $publicIP
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "myBackEndPool"

# Create a Network Address Translation (NAT) pool
$inboundNATPool = New-AzureRmLoadBalancerInboundNatPoolConfig `
  -Name "myRDPRule" `
  -FrontendIpConfigurationId $frontendIP.Id `
  -Protocol TCP `
  -FrontendPortRangeStart 50001 `
  -FrontendPortRangeEnd 50010 `
  -BackendPort 3389

# Create the load balancer
$lb = New-AzureRmLoadBalancer `
  -ResourceGroupName "myResourceGroup" `
  -Name "myLoadBalancer" `
  -Location "chinanorth" `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool `
  -InboundNatPool $inboundNATPool

# Create a load balancer health probe on port 80
Add-AzureRmLoadBalancerProbeConfig -Name "myHealthProbe" `
  -LoadBalancer $lb `
  -Protocol TCP `
  -Port 80 `
  -IntervalInSeconds 15 `
  -ProbeCount 2

# Create a load balancer rule to distribute traffic on port 80
Add-AzureRmLoadBalancerRuleConfig `
  -Name "myLoadBalancerRule" `
  -LoadBalancer $lb `
  -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
  -BackendAddressPool $lb.BackendAddressPools[0] `
  -Protocol TCP `
  -FrontendPort 80 `
  -BackendPort 80

# Update the load balancer configuration
Set-AzureRmLoadBalancer -LoadBalancer $lb

# Create IP address configurations
$ipConfig = New-AzureRmVmssIpConfig `
  -Name "myIPConfig" `
  -LoadBalancerBackendAddressPoolsId $lb.BackendAddressPools[0].Id `
  -LoadBalancerInboundNatPoolsId $inboundNATPool.Id `
  -SubnetId $vnet.Subnets[0].Id
```


## <a name="create-a-scale-set"></a>创建规模集
现在，使用 [New-AzureRmVmss](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvmss) 创建一个虚拟机规模集。 以下示例创建名为 *myScaleSet* 且使用 *Windows Server 2016 Datacenter* 平台映像的规模集。 *vmssConfig* 对象在中国北部创建 2 个 VM 实例，使用在 *adminUsername* 和 *securePassword* 变量中指定的凭据。 提供自己的凭据，然后创建规模集，如下所示：

```azurepowershell
# Provide your own secure password for use with the VM instances
$securePassword = "P@ssword!"
$adminUsername = "azureuser"

# Create a config object
$vmssConfig = New-AzureRmVmssConfig `
    -Location "chinanorth" `
    -SkuCapacity 2 `
    -SkuName "Standard_DS2" `
    -UpgradePolicyMode Automatic

# Reference a virtual machine image from the gallery
Set-AzureRmVmssStorageProfile $vmssConfig `
  -ImageReferencePublisher "MicrosoftWindowsServer" `
  -ImageReferenceOffer "WindowsServer" `
  -ImageReferenceSku "2016-Datacenter" `
  -ImageReferenceVersion "latest"

# Set up information for authenticating with the virtual machine
Set-AzureRmVmssOsProfile $vmssConfig `
  -AdminUsername $adminUsername `
  -AdminPassword $securePassword `
  -ComputerNamePrefix "myVM"

# Attach the virtual network to the config object
Add-AzureRmVmssNetworkInterfaceConfiguration `
  -VirtualMachineScaleSet $vmssConfig `
  -Name "network-config" `
  -Primary $true `
  -IPConfiguration $ipConfig

# Create the scale set with the config object (this step might take a few minutes)
New-AzureRmVmss `
  -ResourceGroupName "myResourceGroup" `
  -Name "myScaleSet" `
  -VirtualMachineScaleSet $vmssConfig
```

创建和配置所有的规模集资源和 VM 需要几分钟时间。


## <a name="install-iis-webserver"></a>安装 IIS Web 服务器
若要测试规模集，请使用自定义脚本扩展下载并运行一个可在 VM 实例上安装 IIS 的脚本。 此扩展适用于部署后配置、软件安装或其他任何配置/管理任务。 有关详细信息，请参阅[自定义脚本扩展概述](../virtual-machines/windows/extensions-customscript.md)。

应用可安装 IIS 的自定义脚本扩展，如下所示：

```azurepowershell
# Define the script for your Custom Script Extension to run
$publicSettings = @{
    "fileUris" = (,"https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/automate-iis.ps1");
    "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File automate-iis.ps1"
}

# Get information about the scale set
$vmss = Get-AzureRmVmss `
            -ResourceGroupName "myResourceGroup" `
            -VMScaleSetName "myScaleSet"

# Use Custom Script Extension to install IIS and configure basic website
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss `
    -Name "customScript" `
    -Publisher "Microsoft.Compute" `
    -Type "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -Setting $publicSettings

# Update the scale set and apply the Custom Script Extension to the VM instances
Update-AzureRmVmss `
    -ResourceGroupName "myResourceGroup" `
    -Name "myScaleSet" `
    -VirtualMachineScaleSet $vmss
```


## <a name="test-your-web-server"></a>测试 Web 服务器
若要查看运行中的 Web 服务器，请使用 [Get-AzureRmPublicIpAddress](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermpublicipaddress) 获取负载均衡器的公共 IP 地址。 以下示例获取在 *myResourceGroup* 资源组中创建的 IP 地址：

```azurepowershell
Get-AzureRmPublicIpAddress -ResourceGroupName "myResourceGroup" | Select IpAddress
```

将负载均衡器的公共 IP 地址输入到 Web 浏览器中。 负载均衡器将流量分发到某个 VM 实例，如以下示例所示：

![运行 IIS 网站](./media/virtual-machine-scale-sets-create-powershell/running-iis-site.png)


## <a name="clean-up-resources"></a>清理资源
如果不再需要资源组、规模集和所有相关的资源，可以使用 [Remove-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/remove-azurermresourcegroup) 命令将其删除，如下所示：

```azurepowershell
Remove-AzureRmResourceGroup -Name "myResourceGroup"
```


## <a name="next-steps"></a>后续步骤
此快速入门文章介绍了如何创建基本的规模集，以及如何使用自定义脚本扩展在 VM 实例上安装基本的 IIS Web 服务器。 若要改进可伸缩性和自动化，请阅读以下操作指南文章，了解如何扩展规模集：

- [在虚拟机规模集上部署应用程序](virtual-machine-scale-sets-deploy-app.md)
- 通过 [Azure PowerShell](virtual-machine-scale-sets-autoscale-powershell.md) 或 [Azure CLI](virtual-machine-scale-sets-autoscale-cli.md) 自动进行缩放
