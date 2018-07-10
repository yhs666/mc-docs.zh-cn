---
title: 创建托管多个站点的应用程序网关 - Azure PowerShell | Microsoft Docs
description: 了解如何使用 Azure Powershell 创建托管多个站点的应用程序网关。
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 01/26/2018
ms.date: 07/02/2018
ms.author: v-junlch
ms.openlocfilehash: 76f3c5e78a6a7c614b1988494e95ada61ed28acb
ms.sourcegitcommit: f0bfa3f8dca94099a2181492952e6a575fbdbcc8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/02/2018
ms.locfileid: "37142663"
---
# <a name="create-an-application-gateway-with-multiple-site-hosting-using-azure-powershell"></a>使用 Azure PowerShell 创建托管多个站点的应用程序网关

创建[应用程序网关](application-gateway-introduction.md)时可以使用 Azure Powershell 配置[多个网站的托管](application-gateway-multi-site-overview.md)。 本教程中使用虚拟机规模集创建后端池。 然后，基于所拥有的域配置侦听器和规则，以确保 Web 流量可到达池中的相应服务器。 本教程假定你拥有多个域，并使用示例 *www.contoso.com* 和 *www.fabrikam.com*。

在本文中，学习如何：

> [!div class="checklist"]
> * 设置网络
> * 创建应用程序网关
> * 创建侦听器和路由规则
> * 使用后端池创建虚拟机规模集
> * 在域中创建 CNAME 记录

![多站点路由示例](./media/application-gateway-create-multisite-azureresourcemanager-powershell/scenario.png)

如果没有 Azure 订阅，请在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial/)。

如果选择在本地安装并使用 PowerShell，则本教程需要 Azure PowerShell 模块版本 3.6 或更高版本。 若要查找版本，请运行 ` Get-Module -ListAvailable AzureRM`。 如果需要进行升级，请参阅 [Install Azure PowerShell module](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)（安装 Azure PowerShell 模块）。 如果在本地运行 PowerShell，则还需运行 `Connect-AzureRmAccount -Environment AzureChinaCloud` 以创建与 Azure 的连接。

## <a name="create-a-resource-group"></a>创建资源组

资源组是在其中部署和管理 Azure 资源的逻辑容器。 使用 [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup) 创建 Azure 资源组。  

```azurepowershell
New-AzureRmResourceGroup -Name myResourceGroupAG -Location chinanorth
```

## <a name="create-network-resources"></a>创建网络资源

使用 [New-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) 配置名为 *myBackendSubnet* 和 *myAGSubnet* 的子网。 使用 [New-AzureRmVirtualNetwork](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetwork) 和子网配置创建名为 *myVNet* 的虚拟网络。 最后使用 [New-AzureRmPublicIpAddress](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermpublicipaddress) 创建名为 *myAGPublicIPAddress* 的公共 IP 地址。 这些资源用于提供与应用程序网关及其关联资源的网络连接。

```azurepowershell
$backendSubnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myBackendSubnet `
  -AddressPrefix 10.0.1.0/24
$agSubnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myAGSubnet `
  -AddressPrefix 10.0.2.0/24
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Location chinanorth `
  -Name myVNet `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $backendSubnetConfig, $agSubnetConfig
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroupAG `
  -Location chinanorth `
  -Name myAGPublicIPAddress `
  -AllocationMethod Dynamic
```

## <a name="create-an-application-gateway"></a>创建应用程序网关

### <a name="create-the-ip-configurations-and-frontend-port"></a>创建 IP 配置和前端端口

使用 [New-AzureRmApplicationGatewayIPConfiguration](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermapplicationgatewayipconfiguration) 将前面创建的 *myAGSubnet* 关联到应用程序网关。 使用 [New-AzureRmApplicationGatewayFrontendIPConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermapplicationgatewayfrontendipconfig) 将 *myAGPublicIPAddress* 分配给应用程序网关。

```azurepowershell
$vnet = Get-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Name myVNet
$subnet=$vnet.Subnets[0]
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration `
  -Name myAGIPConfig `
  -Subnet $subnet
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig `
  -Name myAGFrontendIPConfig `
  -PublicIPAddress $pip
$frontendport = New-AzureRmApplicationGatewayFrontendPort `
  -Name myFrontendPort `
  -Port 80
```

### <a name="create-the-backend-pools-and-settings"></a>创建后端池和设置

使用 [New-AzureRmApplicationGatewayBackendAddressPool](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermapplicationgatewaybackendaddresspool) 为应用程序网关创建名为 *contosoPool* 和 *fabrikamPool* 的后端池。 使用 [New-AzureRmApplicationGatewayBackendHttpSettings](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermapplicationgatewaybackendhttpsettings) 配置池的设置。

```azurepowershell
$contosoPool = New-AzureRmApplicationGatewayBackendAddressPool `
  -Name contosoPool 
$fabrikamPool = New-AzureRmApplicationGatewayBackendAddressPool `
  -Name fabrikamPool 
$poolSettings = New-AzureRmApplicationGatewayBackendHttpSettings `
  -Name myPoolSettings `
  -Port 80 `
  -Protocol Http `
  -CookieBasedAffinity Enabled `
  -RequestTimeout 120
```

### <a name="create-the-listeners-and-rules"></a>创建侦听器和规则

应用程序网关需要侦听器才能适当地将流量路由到后端池。 在本教程中，将为两个域的每一个域创建侦听器。 在此示例中，将为域 *www.contoso.com* 和 *www.fabrikam.com* 创建侦听器。

使用 [New-AzureRmApplicationGatewayHttpListener](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermapplicationgatewayhttplistener) 以及前面创建的前端配置和前端端口创建名为 *contosoListener* 和 *fabrikamListener* 的侦听器。 侦听器需要使用规则来了解哪个后端池用于传入流量。 使用 [New-AzureRmApplicationGatewayRequestRoutingRule](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermapplicationgatewayrequestroutingrule) 创建名为 *contosoRule* 和 *fabrikamRule* 的基本规则。

```azurepowershell
$contosolistener = New-AzureRmApplicationGatewayHttpListener `
  -Name contosoListener `
  -Protocol Http `
  -FrontendIPConfiguration $fipconfig `
  -FrontendPort $frontendport `
  -HostName "www.contoso.com"
$fabrikamlistener = New-AzureRmApplicationGatewayHttpListener `
  -Name fabrikamListener `
  -Protocol Http `
  -FrontendIPConfiguration $fipconfig `
  -FrontendPort $frontendport `
  -HostName "www.fabrikam.com"
$contosoRule = New-AzureRmApplicationGatewayRequestRoutingRule `
  -Name contosoRule `
  -RuleType Basic `
  -HttpListener $contosoListener `
  -BackendAddressPool $contosoPool `
  -BackendHttpSettings $poolSettings
$fabrikamRule = New-AzureRmApplicationGatewayRequestRoutingRule `
  -Name fabrikamRule `
  -RuleType Basic `
  -HttpListener $fabrikamListener `
  -BackendAddressPool $fabrikamPool `
  -BackendHttpSettings $poolSettings
```

### <a name="create-the-application-gateway"></a>创建应用程序网关

现在已创建所需的支持资源，请使用 [New-AzureRmApplicationGatewaySku](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermapplicationgatewaysku) 为名为 *myAppGateway* 的应用程序网关指定参数，然后再使用 [New-AzureRmApplicationGateway](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermapplicationgateway) 创建它。

```azurepowershell
$sku = New-AzureRmApplicationGatewaySku `
  -Name Standard_Medium `
  -Tier Standard `
  -Capacity 2
$appgw = New-AzureRmApplicationGateway `
  -Name myAppGateway `
  -ResourceGroupName myResourceGroupAG `
  -Location chinanorth `
  -BackendAddressPools $contosoPool, $fabrikamPool `
  -BackendHttpSettingsCollection $poolSettings `
  -FrontendIpConfigurations $fipconfig `
  -GatewayIpConfigurations $gipconfig `
  -FrontendPorts $frontendport `
  -HttpListeners $contosoListener, $fabrikamListener `
  -RequestRoutingRules $contosoRule, $fabrikamRule `
  -Sku $sku
```

## <a name="create-virtual-machine-scale-sets"></a>创建虚拟机规模集

在此示例中，将创建两个虚拟机规模集以支持所创建的两个后端池。 创建的规模集分别名为 *myvmss1* 和 *myvmss2*。 每个规模集包含两个在其上安装了 IIS 的虚拟机实例。 配置 IP 设置时将规模集分配给后端池。

```azurepowershell
$vnet = Get-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Name myVNet
$appgw = Get-AzureRmApplicationGateway `
  -ResourceGroupName myResourceGroupAG `
  -Name myAppGateway
$contosoPool = Get-AzureRmApplicationGatewayBackendAddressPool `
  -Name contosoPool `
  -ApplicationGateway $appgw
$fabrikamPool = Get-AzureRmApplicationGatewayBackendAddressPool `
  -Name fabrikamPool `
  -ApplicationGateway $appgw
for ($i=1; $i -le 2; $i++)
{
  if ($i -eq 1) 
  {
    $poolId = $contosoPool.Id
  }
  if ($i -eq 2)
  {
    $poolId = $fabrikamPool.Id
  }
  $ipConfig = New-AzureRmVmssIpConfig `
    -Name myVmssIPConfig$i `
    -SubnetId $vnet.Subnets[1].Id `
    -ApplicationGatewayBackendAddressPoolsId $poolId
  $vmssConfig = New-AzureRmVmssConfig `
    -Location chinanorth `
    -SkuCapacity 2 `
    -SkuName Standard_DS2 `
    -UpgradePolicyMode Automatic
  Set-AzureRmVmssStorageProfile $vmssConfig `
    -OsDiskCreateOption "FromImage" `
    -ImageReferencePublisher MicrosoftWindowsServer `
    -ImageReferenceOffer WindowsServer `
    -ImageReferenceSku 2016-Datacenter `
    -ImageReferenceVersion latest
  Set-AzureRmVmssOsProfile $vmssConfig `
    -AdminUsername azureuser `
    -AdminPassword "Azure123456!" `
    -ComputerNamePrefix myvmss$i
  Add-AzureRmVmssNetworkInterfaceConfiguration `
    -VirtualMachineScaleSet $vmssConfig `
    -Name myVmssNetConfig$i `
    -Primary $true `
    -IPConfiguration $ipConfig
  New-AzureRmVmss `
    -ResourceGroupName myResourceGroupAG `
    -Name myvmss$i `
    -VirtualMachineScaleSet $vmssConfig
}
```

### <a name="install-iis"></a>安装 IIS

```azurepowershell
$publicSettings = @{ "fileUris" = (,"https://raw.githubusercontent.com/davidmu1/samplescripts/master/appgatewayurl.ps1"); 
  "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File appgatewayurl.ps1" }

for ($i=1; $i -le 2; $i++)
{
  $vmss = Get-AzureRmVmss `
    -ResourceGroupName myResourceGroupAG `
    -VMScaleSetName myvmss$i
  Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss `
    -Name "customScript" `
    -Publisher "Microsoft.Compute" `
    -Type "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -Setting $publicSettings
  Update-AzureRmVmss `
    -ResourceGroupName myResourceGroupAG `
    -Name myvmss$i `
    -VirtualMachineScaleSet $vmss
}
```

## <a name="create-cname-record-in-your-domain"></a>在域中创建 CNAME 记录

使用其公共 IP 地址创建应用程序网关后，可以获取 DNS 地址并使用它在域中创建 CNAME 记录。 可以使用 [Get-AzureRmPublicIPAddress](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermpublicipaddress) 获取应用程序网关的 DNS 地址。 复制 DNSSettings 的 *fqdn* 值并使用它作为所创建的 CNAME 记录的值。 不建议使用 A 记录，因为重新启动应用程序网关后 VIP 可能会变化。

```azurepowershell
Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress
```

## <a name="test-the-application-gateway"></a>测试应用程序网关

在浏览器的地址栏中输入域名。 例如，http://www.contoso.com。

![在应用程序网关中测试 contoso 站点](./media/application-gateway-create-multisite-azureresourcemanager-powershell/application-gateway-iistest.png)

将地址更改为其他域，应看到类似下例所示的内容：

![在应用程序网关中测试 fabrikam 站点](./media/application-gateway-create-multisite-azureresourcemanager-powershell/application-gateway-iistest2.png)

## <a name="next-steps"></a>后续步骤

本文介绍了如何执行以下操作：

> [!div class="checklist"]
> * 设置网络
> * 创建应用程序网关
> * 创建侦听器和路由规则
> * 使用后端池创建虚拟机规模集
> * 在域中创建 CNAME 记录

> [!div class="nextstepaction"]
> [详细了解应用程序网关的作用](application-gateway-introduction.md)

