---
title: 创建支持外部重定向的应用程序网关 - Azure PowerShell | Microsoft Docs
description: 了解如何创建将 web 流量重定向到外部站点使用 Azure Powershell 的应用程序网关。
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 01/24/2018
ms.date: 06/06/2018
ms.author: v-junlch
ms.openlocfilehash: 0f68a59f17c05de5e17269fd4de4b0002de5b621
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52651224"
---
# <a name="create-an-application-gateway-with-external-redirection-using-azure-powershell"></a>使用 Azure PowerShell 创建支持外部重定向的应用程序网关

你可以使用 Azure Powershell 配置 [web 流量重定向](multiple-site-overview.md)创建时[应用程序网关](overview.md)。 在本教程中，可以配置侦听器和重定向到外部站点的应用程序网关在到达的 web 流量的规则。

在本文中，学习如何：

> [!div class="checklist"]
> * 设置网络
> * 创建侦听器和重定向规则
> * 创建应用程序网关

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial/?WT.mc_id=A261C142F)。

如果选择在本地安装并使用 PowerShell，则本教程需要 Azure PowerShell 模块版本 3.6 或更高版本。 若要查找版本，请运行 ` Get-Module -ListAvailable AzureRM`。 如果需要进行升级，请参阅 [Install Azure PowerShell module](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)（安装 Azure PowerShell 模块）。 如果在本地运行 PowerShell，则还需运行 `Login-AzureRmAccount -EnvironmentName AzureChinaCloud` 以创建与 Azure 的连接。

## <a name="create-a-resource-group"></a>创建资源组

资源组是在其中部署和管理 Azure 资源的逻辑容器。 使用 [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup) 创建 Azure 资源组。  

```azurepowershell
New-AzureRmResourceGroup -Name myResourceGroupAG -Location chinanorth
```

## <a name="create-network-resources"></a>创建网络资源

使用 [New-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) 创建子网配置 *myAGSubnet*。 使用 [New-AzureRmVirtualNetwork](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetwork) 和子网配置创建名为 *myVNet* 的虚拟网络。 最后使用 [New-AzureRmPublicIpAddress](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermpublicipaddress) 创建公共 IP 地址。 这些资源用于提供与应用程序网关及其关联资源的网络连接。

```azurepowershell
$agSubnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myAGSubnet `
  -AddressPrefix 10.0.1.0/24
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Location chinanorth `
  -Name myVNet `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $agSubnetConfig
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroupAG `
  -Location chinanorth `
  -Name myAGPublicIPAddress `
  -AllocationMethod Dynamic
```

## <a name="create-an-application-gateway"></a>创建应用程序网关

### <a name="create-the-ip-configurations-and-frontend-port"></a>创建 IP 配置和前端端口

使用 [New-AzureRmApplicationGatewayIPConfiguration](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermapplicationgatewayipconfiguration) 将前面创建的 *myAGSubnet* 关联到应用程序网关。 使用 [New-AzureRmApplicationGatewayFrontendIPConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermapplicationgatewayfrontendipconfig) 将公共 IP 地址分配给应用程序网关。 然后，可以创建使用 [New-AzureRmApplicationGatewayFrontendPort](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermapplicationgatewayfrontendport) 创建 HTTP 端口。

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

### <a name="create-the-backend-pool-and-settings"></a>创建后端池和设置

使用 [New-AzureRmApplicationGatewayBackendAddressPool](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermapplicationgatewaybackendaddresspool) 为应用程序网关创建名为 *defaultPool* 的后端池。 使用 [New-AzureRmApplicationGatewayBackendHttpSettings](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermapplicationgatewaybackendhttpsettings) 配置池的设置。

```azurepowershell
$defaultPool = New-AzureRmApplicationGatewayBackendAddressPool `
  -Name defaultPool 
$poolSettings = New-AzureRmApplicationGatewayBackendHttpSettings `
  -Name myPoolSettings `
  -Port 80 `
  -Protocol Http `
  -CookieBasedAffinity Enabled `
  -RequestTimeout 120
```

### <a name="create-the-listener-and-rule"></a>创建侦听器并添加规则

应用程序网关需要侦听器才能适当地将流量路由到后端池。 使用 [New-AzureRmApplicationGatewayHttpListener](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermapplicationgatewayhttplistener) 以及前面创建的前端配置和前端端口创建侦听器。 侦听器需要使用规则来了解哪个后端池使用传入流量。 使用 [New-AzureRmApplicationGatewayRequestRoutingRule](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermapplicationgatewayrequestroutingrule) 创建名为 *redirectRule* 的基本规则。

```azurepowershell
$defaultListener = New-AzureRmApplicationGatewayHttpListener `
  -Name defaultListener `
  -Protocol Http `
  -FrontendIPConfiguration $fipconfig `
  -FrontendPort $frontendport
$redirectConfig = New-AzureRmApplicationGatewayRedirectConfiguration `
  -Name myredirect `
  -RedirectType Temporary `
  -TargetUrl "http://bing.com"
$redirectRule = New-AzureRmApplicationGatewayRequestRoutingRule `
  -Name redirectRule `
  -RuleType Basic `
  -HttpListener $defaultListener `
  -RedirectConfiguration $redirectConfig
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
  -BackendAddressPools $defaultPool `
  -BackendHttpSettingsCollection $poolSettings `
  -FrontendIpConfigurations $fipconfig `
  -GatewayIpConfigurations $gipconfig `
  -FrontendPorts $frontendport `
  -HttpListeners $defaultListener `
  -RequestRoutingRules $redirectRule `
  -RedirectConfigurations $redirectConfig `
  -Sku $sku
```

## <a name="test-the-application-gateway"></a>测试应用程序网关

可以使用 [Get-AzureRmPublicIPAddress](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermpublicipaddress) 获取应用程序网关的公共 IP 地址。 复制该公共 IP 地址，并将其粘贴到浏览器的地址栏。

```azurepowershell
Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress
```

应该会看到 *bing.com* 出现在浏览器中。

## <a name="next-steps"></a>后续步骤

本文介绍了如何执行以下操作：

> [!div class="checklist"]
> * 设置网络
> * 创建侦听器和重定向规则
> * 创建应用程序网关

