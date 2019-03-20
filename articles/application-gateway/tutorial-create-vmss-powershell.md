---
title: 使用虚拟机规模集创建应用程序网关 - Azure PowerShell
description: 了解如何通过 Azure PowerShell 使用虚拟机规模集创建应用程序网关。
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
origin.date: 02/26/2019
ms.date: 03/12/2019
ms.author: v-junlch
ms.openlocfilehash: 9abd33d2d5a933b2fca3d710371c8ddf9cc01940
ms.sourcegitcommit: d750a61a0e52a41cff5607149e33b6be189075d4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/12/2019
ms.locfileid: "57788706"
---
# <a name="create-an-application-gateway-and-virtual-machine-scale-set-using-azure-powershell"></a>使用 Azure PowerShell 创建应用程序网关和虚拟机规模集

可以使用 Azure PowerShell 创建使用[虚拟机规模集](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md)作为后端服务器的[应用程序网关](application-gateway-introduction.md)。 在此示例中，规模集包含两个添加到应用程序网关的默认后端池的虚拟机实例。

在本文中，学习如何：

> [!div class="checklist"]
> * 设置网络
> * 创建应用程序网关
> * 使用默认后端池创建虚拟机规模集

如果没有 Azure 订阅，请在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial/)。

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

如果选择在本地安装并使用 PowerShell，则本教程需要安装 Azure PowerShell 模块 1.0.0 或更高版本。 若要查找版本，请运行 `Get-Module -ListAvailable Az`。 如果需要进行升级，请参阅 [Install Azure PowerShell module](https://docs.microsoft.com/powershell/azure/install-az-ps)（安装 Azure PowerShell 模块）。 如果在本地运行 PowerShell，则还需运行 `Connect-AzAccount -Environment AzureChinaCloud` 来创建与 Azure 的连接。

## <a name="create-a-resource-group"></a>创建资源组

资源组是在其中部署和管理 Azure 资源的逻辑容器。 使用 [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) 创建 Azure 资源组。  

```azurepowershell
New-AzResourceGroup -Name myResourceGroupAG -Location chinanorth
```

## <a name="create-network-resources"></a>创建网络资源 

使用 [New-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetworksubnetconfig) 配置名为 *myBackendSubnet* 和 *myAGSubnet* 的子网。 使用 [New-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetwork) 和子网配置创建虚拟网络 *myVNet*。 最后使用 [New-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipaddress) 创建名为 *myAGPublicIPAddress* 的公共 IP 地址。 这些资源用于提供与应用程序网关及其关联资源的网络连接。

```azurepowershell
$backendSubnetConfig = New-AzVirtualNetworkSubnetConfig `
  -Name myBackendSubnet `
  -AddressPrefix 10.0.1.0/24
$agSubnetConfig = New-AzVirtualNetworkSubnetConfig `
  -Name myAGSubnet `
  -AddressPrefix 10.0.2.0/24
$vnet = New-AzVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Location chinanorth `
  -Name myVNet `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $backendSubnetConfig, $agSubnetConfig
$pip = New-AzPublicIpAddress `
  -ResourceGroupName myResourceGroupAG `
  -Location chinanorth `
  -Name myAGPublicIPAddress `
  -AllocationMethod Dynamic
```

## <a name="create-an-application-gateway"></a>创建应用程序网关

### <a name="create-the-ip-configurations-and-frontend-port"></a>创建 IP 配置和前端端口

使用 [New-AzApplicationGatewayIPConfiguration](https://docs.microsoft.com/powershell/module/az.network/new-azapplicationgatewayipconfiguration) 将前面创建的 *myAGSubnet* 关联到应用程序网关。 使用 [New-AzApplicationGatewayFrontendIPConfig](https://docs.microsoft.com/powershell/module/az.network/new-azapplicationgatewayfrontendipconfig) 将 *myAGPublicIPAddress* 分配给应用程序网关。

```azurepowershell
$vnet = Get-AzVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Name myVNet
$subnet=$vnet.Subnets[0]
$gipconfig = New-AzApplicationGatewayIPConfiguration `
  -Name myAGIPConfig `
  -Subnet $subnet
$fipconfig = New-AzApplicationGatewayFrontendIPConfig `
  -Name myAGFrontendIPConfig `
  -PublicIPAddress $pip
$frontendport = New-AzApplicationGatewayFrontendPort `
  -Name myFrontendPort `
  -Port 80
```

### <a name="create-the-backend-pool-and-settings"></a>创建后端池和设置

使用 [New-AzApplicationGatewayBackendAddressPool](https://docs.microsoft.com/powershell/module/az.network/new-azapplicationgatewaybackendaddresspool) 为应用程序网关创建名为 *appGatewayBackendPool* 的后端池。 使用 [New-AzApplicationGatewayBackendHttpSettings](https://docs.microsoft.com/powershell/module/az.network/new-azapplicationgatewaybackendhttpsettings) 配置后端地址池的设置。

```azurepowershell
$defaultPool = New-AzApplicationGatewayBackendAddressPool `
  -Name appGatewayBackendPool 
$poolSettings = New-AzApplicationGatewayBackendHttpSettings `
  -Name myPoolSettings `
  -Port 80 `
  -Protocol Http `
  -CookieBasedAffinity Enabled `
  -RequestTimeout 120
```

### <a name="create-the-default-listener-and-rule"></a>创建默认侦听器和规则

应用程序网关需要侦听器才能适当地将流量路由到后端池。 在此示例中，将一个创建基本侦听器以侦听根 URL 上的流量。 

使用 [New-AzApplicationGatewayHttpListener](https://docs.microsoft.com/powershell/module/az.network/new-azapplicationgatewayhttplistener) 以及前面创建的前端配置和前端端口创建名为 *mydefaultListener* 的侦听器。 侦听器需要使用规则来了解哪个后端池使用传入流量。 使用 [New-AzApplicationGatewayRequestRoutingRule](https://docs.microsoft.com/powershell/module/az.network/new-azapplicationgatewayrequestroutingrule) 创建一个名为 *rule1* 的基本规则。

```azurepowershell
$defaultlistener = New-AzApplicationGatewayHttpListener `
  -Name mydefaultListener `
  -Protocol Http `
  -FrontendIPConfiguration $fipconfig `
  -FrontendPort $frontendport
$frontendRule = New-AzApplicationGatewayRequestRoutingRule `
  -Name rule1 `
  -RuleType Basic `
  -HttpListener $defaultlistener `
  -BackendAddressPool $defaultPool `
  -BackendHttpSettings $poolSettings
```

### <a name="create-the-application-gateway"></a>创建应用程序网关

现在已创建所需的支持资源，请使用 [New-AzApplicationGatewaySku](https://docs.microsoft.com/powershell/module/az.network/new-azapplicationgatewaysku) 为应用程序网关指定参数，然后再使用 [New-AzApplicationGateway](https://docs.microsoft.com/powershell/module/az.network/new-azapplicationgateway) 创建它。

```azurepowershell
$sku = New-AzApplicationGatewaySku `
  -Name Standard_Medium `
  -Tier Standard `
  -Capacity 2
$appgw = New-AzApplicationGateway `
  -Name myAppGateway `
  -ResourceGroupName myResourceGroupAG `
  -Location chinanorth `
  -BackendAddressPools $defaultPool `
  -BackendHttpSettingsCollection $poolSettings `
  -FrontendIpConfigurations $fipconfig `
  -GatewayIpConfigurations $gipconfig `
  -FrontendPorts $frontendport `
  -HttpListeners $defaultlistener `
  -RequestRoutingRules $frontendRule `
  -Sku $sku
```

## <a name="create-a-virtual-machine-scale-set"></a>创建虚拟机规模集

在此示例中，将创建虚拟机规模集，以便为应用程序网关的后端池提供服务器。 配置 IP 设置时将规模集分配给后端池。

```azurepowershell
$vnet = Get-AzVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Name myVNet
$appgw = Get-AzApplicationGateway `
  -ResourceGroupName myResourceGroupAG `
  -Name myAppGateway
$backendPool = Get-AzApplicationGatewayBackendAddressPool `
  -Name appGatewayBackendPool `
  -ApplicationGateway $appgw
$ipConfig = New-AzVmssIpConfig `
  -Name myVmssIPConfig `
  -SubnetId $vnet.Subnets[1].Id `
  -ApplicationGatewayBackendAddressPoolsId $backendPool.Id
$vmssConfig = New-AzVmssConfig `
  -Location chinanorth `
  -SkuCapacity 2 `
  -SkuName Standard_DS2 `
  -UpgradePolicyMode Automatic

Set-AzVmssStorageProfile $vmssConfig `
  -OsDiskCreateOption "FromImage" -OsDiskCaching "None" `
  -ImageReferencePublisher MicrosoftWindowsServer `
  -ImageReferenceOffer WindowsServer `
  -ImageReferenceSku 2016-Datacenter `
  -ImageReferenceVersion latest
Set-AzVmssOsProfile $vmssConfig `
  -AdminUsername azureuser `
  -AdminPassword "Azure123456!" `
  -ComputerNamePrefix myvmss
Add-AzVmssNetworkInterfaceConfiguration `
  -VirtualMachineScaleSet $vmssConfig `
  -Name myVmssNetConfig `
  -Primary $true `
  -IPConfiguration $ipConfig
New-AzVmss `
  -ResourceGroupName myResourceGroupAG `
  -Name myvmss `
  -VirtualMachineScaleSet $vmssConfig
```

### <a name="install-iis"></a>安装 IIS

```azurepowershell
$publicSettings = @{ "fileUris" = (,"https://raw.githubusercontent.com/Azure/azure-docs-powershell-samples/master/application-gateway/iis/appgatewayurl.ps1"); 
  "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File appgatewayurl.ps1" }
$vmss = Get-AzVmss -ResourceGroupName myResourceGroupAG -VMScaleSetName myvmss
Add-AzVmssExtension -VirtualMachineScaleSet $vmss `
  -Name "customScript" `
  -Publisher "Microsoft.Compute" `
  -Type "CustomScriptExtension" `
  -TypeHandlerVersion 1.8 `
  -Setting $publicSettings
Update-AzVmss `
  -ResourceGroupName myResourceGroupAG `
  -Name myvmss `
  -VirtualMachineScaleSet $vmss
```

## <a name="test-the-application-gateway"></a>测试应用程序网关

可以使用 [Get-AzPublicIPAddress](https://docs.microsoft.com/powershell/module/az.network/get-azpublicipaddress) 获取应用程序网关的公共 IP 地址。 复制该公共 IP 地址，并将其粘贴到浏览器的地址栏。

```azurepowershell
Get-AzPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress
```

![在应用程序网关中测试基 URL](./media/tutorial-create-vmss-powershell/tutorial-iistest.png)

## <a name="next-steps"></a>后续步骤

在本教程中，你已学习了如何执行以下操作：

> [!div class="checklist"]
> * 设置网络
> * 创建应用程序网关
> * 使用默认后端池创建虚拟机规模集

若要详细了解应用程序网关及其关联的资源，请继续阅读操作指南文章。

<!-- Update_Description: code and link update -->