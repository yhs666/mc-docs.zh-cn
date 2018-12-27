---
title: 创建支持 HTTP 到 HTTPS 重定向的应用程序网关 - Azure PowerShell | Microsoft Docs
description: 了解如何使用 Azure PowerShell 创建支持从 HTTP 到 HTTPS 重定向流量的应用程序网关。
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
tags: azure-resource-manager
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
origin.date: 01/23/2018
ms.date: 08/08/2018
ms.author: v-junlch
ms.openlocfilehash: 40088013b8519b9d6595e1cd212a30ee970cb6d1
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52651345"
---
# <a name="create-an-application-gateway-with-http-to-https-redirection-using-azure-powershell"></a>使用 Azure PowerShell 创建支持 HTTP 到 HTTPS 重定向的应用程序网关

可以通过 Azure PowerShell 使用 SSL 终端的证书创建[应用程序网关](application-gateway-introduction.md)。 路由规则用于将 HTTP 流量重定向到应用程序网关中的 HTTPS 端口。 在此示例中，还会为包含两个虚拟机实例的应用程序网关的后端池创建一个[虚拟机规模集](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md)。 

在本文中，学习如何：

> [!div class="checklist"]
> * 创建自签名证书
> * 设置网络
> * 使用证书创建应用程序网关
> * 添加侦听器和重定向规则
> * 使用默认后端池创建虚拟机规模集

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial/)。

本教程需要 Azure PowerShell 模块 3.6 或更高版本。 可以运行 `Get-Module -ListAvailable AzureRM` 来查找版本。 如果需要进行升级，请参阅 [Install Azure PowerShell module](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)（安装 Azure PowerShell 模块）。 若要运行本教程中的命令，还需要运行 `Connect-AzureRmAccount -Environment AzureChinaCloud` 以创建与 Azure 的连接。

## <a name="create-a-self-signed-certificate"></a>创建自签名证书

为供生产使用，应导入由受信任的提供程序签名的有效证书。 对于本教程，请使用 [New-SelfSignedCertificate](https://docs.microsoft.com/powershell/module/pkiclient/new-selfsignedcertificate) 创建自签名证书。 可以结合返回的指纹使用 [Export-PfxCertificate](https://docs.microsoft.com/powershell/module/pkiclient/export-pfxcertificate)，从证书导出 pfx 文件。

```powershell
New-SelfSignedCertificate `
  -certstorelocation cert:\localmachine\my `
  -dnsname www.contoso.com
```

应会显示如下结果所示的内容：

```
PSParentPath: Microsoft.PowerShell.Security\Certificate::LocalMachine\my

Thumbprint                                Subject
----------                                -------
E1E81C23B3AD33F9B4D1717B20AB65DBB91AC630  CN=www.contoso.com
```

使用指纹创建 pfx 文件：

```powershell
$pwd = ConvertTo-SecureString -String "Azure123456!" -Force -AsPlainText
Export-PfxCertificate `
  -cert cert:\localMachine\my\E1E81C23B3AD33F9B4D1717B20AB65DBB91AC630 `
  -FilePath c:\appgwcert.pfx `
  -Password $pwd
```

## <a name="create-a-resource-group"></a>创建资源组

资源组是在其中部署和管理 Azure 资源的逻辑容器。 使用 [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup) 创建名为 *myResourceGroupAG* 的 Azure 资源组。 

```powershell
New-AzureRmResourceGroup -Name myResourceGroupAG -Location chinanorth
```

## <a name="create-network-resources"></a>创建网络资源

使用 [New-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) 创建 *myBackendSubnet* 和 *myAGSubnet* 的子网配置。 使用 [New-AzureRmVirtualNetwork](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetwork) 和子网配置创建名为 *myVNet* 的虚拟网络。 最后使用 [New-AzureRmPublicIpAddress](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermpublicipaddress) 创建名为 *myAGPublicIPAddress* 的公共 IP 地址。 这些资源用于提供与应用程序网关及其关联资源的网络连接。

```powershell
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

使用 [New-AzureRmApplicationGatewayIPConfiguration](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermapplicationgatewayipconfiguration) 将前面创建的 *myAGSubnet* 关联到应用程序网关。 使用 [New-AzureRmApplicationGatewayFrontendIPConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermapplicationgatewayfrontendipconfig) 将 *myAGPublicIPAddress* 分配给应用程序网关。 然后，可以使用 [New-AzureRmApplicationGatewayFrontendPort](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermapplicationgatewayfrontendport) 创建 HTTPS 端口。

```powershell
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
$frontendPort = New-AzureRmApplicationGatewayFrontendPort `
  -Name myFrontendPort `
  -Port 443
```

### <a name="create-the-backend-pool-and-settings"></a>创建后端池和设置

使用 [New-AzureRmApplicationGatewayBackendAddressPool](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermapplicationgatewaybackendaddresspool) 为应用程序网关创建名为 *appGatewayBackendPool* 的后端池。 使用 [New-AzureRmApplicationGatewayBackendHttpSettings](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermapplicationgatewaybackendhttpsettings) 配置后端池的设置。

```powershell
$defaultPool = New-AzureRmApplicationGatewayBackendAddressPool `
  -Name appGatewayBackendPool 
$poolSettings = New-AzureRmApplicationGatewayBackendHttpSettings `
  -Name myPoolSettings `
  -Port 80 `
  -Protocol Http `
  -CookieBasedAffinity Enabled `
  -RequestTimeout 120
```

### <a name="create-the-default-listener-and-rule"></a>创建默认侦听器和规则

应用程序网关需要侦听器才能适当地将流量路由到后端池。 在此示例中，将一个创建基本侦听器以侦听根 URL 上的 HTTPS 流量。 

使用 [New-AzureRmApplicationGatewaySslCertificate](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermapplicationgatewaysslcertificate) 创建证书对象，然后结合前端配置、前端端口和前面创建的证书使用 [New-AzureRmApplicationGatewayHttpListener](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermapplicationgatewayhttplistener) 创建名为 *appGatewayHttpListener* 的侦听器。 侦听器需要使用规则来了解哪个后端池使用传入流量。 使用 [New-AzureRmApplicationGatewayRequestRoutingRule](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermapplicationgatewayrequestroutingrule) 创建一个名为 *rule1* 的基本规则。

```powershell
$pwd = ConvertTo-SecureString `
  -String "Azure123456!" `
  -Force `
  -AsPlainText
$cert = New-AzureRmApplicationGatewaySslCertificate `
  -Name "appgwcert" `
  -CertificateFile "c:\appgwcert.pfx" `
  -Password $pwd
$defaultListener = New-AzureRmApplicationGatewayHttpListener `
  -Name appGatewayHttpListener `
  -Protocol Https `
  -FrontendIPConfiguration $fipconfig `
  -FrontendPort $frontendPort `
  -SslCertificate $cert
$frontendRule = New-AzureRmApplicationGatewayRequestRoutingRule `
  -Name rule1 `
  -RuleType Basic `
  -HttpListener $defaultListener `
  -BackendAddressPool $defaultPool `
  -BackendHttpSettings $poolSettings
```

### <a name="create-the-application-gateway"></a>创建应用程序网关

现在已创建所需的支持资源，请使用 [New-AzureRmApplicationGatewaySku](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermapplicationgatewaysku) 为名为 *myAppGateway* 的应用程序网关指定参数，然后结合证书使用 [New-AzureRmApplicationGateway](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermapplicationgateway) 创建网关。

```powershell
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
  -FrontendPorts $frontendPort `
  -HttpListeners $defaultListener `
  -RequestRoutingRules $frontendRule `
  -Sku $sku `
  -SslCertificates $cert
```

## <a name="add-a-listener-and-redirection-rule"></a>添加侦听器和重定向规则

### <a name="add-the-http-port"></a>添加 HTTP 端口

使用 [Add-AzureRmApplicationGatewayFrontendPort](https://docs.microsoft.com/powershell/module/azurerm.network/add-azurermapplicationgatewayfrontendport) 向应用程序网关添加 HTTP 端口。

```powershell
$appgw = Get-AzureRmApplicationGateway `
  -Name myAppGateway `
  -ResourceGroupName myResourceGroupAG
Add-AzureRmApplicationGatewayFrontendPort `
  -Name httpPort  `
  -Port 80 `
  -ApplicationGateway $appgw
```

### <a name="add-the-http-listener"></a>添加 HTTP 侦听器

使用 [Add-AzureRmApplicationGatewayHttpListener](https://docs.microsoft.com/powershell/module/azurerm.network/add-azurermapplicationgatewayhttplistener) 向应用程序网关添加名为 *myListener* 的 HTTP 侦听器。

```powershell
$fipconfig = Get-AzureRmApplicationGatewayFrontendIPConfig `
  -Name myAGFrontendIPConfig `
  -ApplicationGateway $appgw
$fp = Get-AzureRmApplicationGatewayFrontendPort `
  -Name httpPort `
  -ApplicationGateway $appgw
Add-AzureRmApplicationGatewayHttpListener `
  -Name myListener `
  -Protocol Http `
  -FrontendPort $fp `
  -FrontendIPConfiguration $fipconfig `
  -ApplicationGateway $appgw
```

### <a name="add-the-redirection-configuration"></a>添加重定向配置

使用 [Add-AzureRmApplicationGatewayRedirectConfiguration](https://docs.microsoft.com/powershell/module/azurerm.network/add-azurermapplicationgatewayredirectconfiguration) 将 HTTP 到 HTTPS 重定向配置添加到应用程序网关。

```powershell
$defaultListener = Get-AzureRmApplicationGatewayHttpListener `
  -Name appGatewayHttpListener `
  -ApplicationGateway $appgw
Add-AzureRmApplicationGatewayRedirectConfiguration -Name httpToHttps `
  -RedirectType Permanent `
  -TargetListener $defaultListener `
  -IncludePath $true `
  -IncludeQueryString $true `
  -ApplicationGateway $appgw
```

### <a name="add-the-routing-rule"></a>添加路由规则

使用 [Add-AzureRmApplicationGatewayRequestRoutingRule](https://docs.microsoft.com/powershell/module/azurerm.network/add-azurermapplicationgatewayrequestroutingrule) 将具有重定向配置的路由规则添加到应用程序网关。

```powershell
$myListener = Get-AzureRmApplicationGatewayHttpListener `
  -Name myListener `
  -ApplicationGateway $appgw
$redirectConfig = Get-AzureRmApplicationGatewayRedirectConfiguration `
  -Name httpToHttps `
  -ApplicationGateway $appgw
Add-AzureRmApplicationGatewayRequestRoutingRule `
  -Name rule2 `
  -RuleType Basic `
  -HttpListener $myListener `
  -RedirectConfiguration $redirectConfig `
  -ApplicationGateway $appgw
Set-AzureRmApplicationGateway -ApplicationGateway $appgw
```

## <a name="create-a-virtual-machine-scale-set"></a>创建虚拟机规模集

在此示例中，将创建虚拟机规模集，以便为应用程序网关的后端池提供服务器。 配置 IP 设置时将规模集分配给后端池。

```powershell
$vnet = Get-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Name myVNet
$appgw = Get-AzureRmApplicationGateway `
  -ResourceGroupName myResourceGroupAG `
  -Name myAppGateway
$backendPool = Get-AzureRmApplicationGatewayBackendAddressPool `
  -Name appGatewayBackendPool `
  -ApplicationGateway $appgw
$ipConfig = New-AzureRmVmssIpConfig `
  -Name myVmssIPConfig `
  -SubnetId $vnet.Subnets[1].Id `
  -ApplicationGatewayBackendAddressPoolsId $backendPool.Id
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
  -ComputerNamePrefix myvmss
Add-AzureRmVmssNetworkInterfaceConfiguration `
  -VirtualMachineScaleSet $vmssConfig `
  -Name myVmssNetConfig `
  -Primary $true `
  -IPConfiguration $ipConfig
New-AzureRmVmss `
  -ResourceGroupName myResourceGroupAG `
  -Name myvmss `
  -VirtualMachineScaleSet $vmssConfig
```

### <a name="install-iis"></a>安装 IIS

```powershell
$publicSettings = @{ "fileUris" = (,"https://raw.githubusercontent.com/Azure/azure-docs-powershell-samples/master/application-gateway/iis/appgatewayurl.ps1"); 
  "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File appgatewayurl.ps1" }
$vmss = Get-AzureRmVmss -ResourceGroupName myResourceGroupAG -VMScaleSetName myvmss
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss `
  -Name "customScript" `
  -Publisher "Microsoft.Compute" `
  -Type "CustomScriptExtension" `
  -TypeHandlerVersion 1.8 `
  -Setting $publicSettings
Update-AzureRmVmss `
  -ResourceGroupName myResourceGroupAG `
  -Name myvmss `
  -VirtualMachineScaleSet $vmss
```

## <a name="test-the-application-gateway"></a>测试应用程序网关

可以使用 [Get-AzureRmPublicIPAddress](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermpublicipaddress) 获取应用程序网关的公共 IP 地址。 复制该公共 IP 地址，并将其粘贴到浏览器的地址栏。 例如： http://52.170.203.149

```powershell
Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress
```

![安全警告](./media/tutorial-http-redirect-powershell/application-gateway-secure.png)

若要接受有关使用自签名证书的安全警告，请依次选择“详细信息”和“继续转到网页”。 随即显示受保护的 IIS 网站，如下例所示：

![在应用程序网关中测试基 URL](./media/tutorial-http-redirect-powershell/application-gateway-iistest.png)

## <a name="next-steps"></a>后续步骤

在本教程中，你已学习了如何执行以下操作：

> [!div class="checklist"]
> * 创建自签名证书
> * 设置网络
> * 使用证书创建应用程序网关
> * 添加侦听器和重定向规则
> * 使用默认后端池创建虚拟机规模集

> [!div class="nextstepaction"]
> [详细了解应用程序网关的作用](application-gateway-introduction.md)

<!-- Update_Description: code update -->