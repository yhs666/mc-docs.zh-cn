---
title: 快速入门 - 使用 Azure 应用程序网关定向 Web 流量 - Azure PowerShell | Microsoft Docs
description: 了解如何使用 Azure PowerShell 创建 Azure 应用程序网关，用以将 Web 流量重定向到后端池中的虚拟机。
services: application-gateway
author: vhorne
manager: jpconnock
editor: ''
tags: azure-resource-manager
ms.service: application-gateway
ms.devlang: azurepowershell
ms.topic: quickstart
ms.workload: infrastructure-services
origin.date: 01/25/2018
ms.date: 06/04/2018
ms.author: v-junlch
ms.custom: mvc
ms.openlocfilehash: afd2a33b6509c35f6d6c514eafb9d0c964d96c37
ms.sourcegitcommit: 4fe9905d17a8df9f2270543a5a0ce1762a5830c9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/08/2018
ms.locfileid: "34855785"
---
# <a name="quickstart-direct-web-traffic-with-azure-application-gateway---azure-powershell"></a>快速入门：使用 Azure 应用程序网关定向 Web 流量 - Azure PowerShell

使用 Azure 应用程序网关，可以通过为端口分配侦听器、创建规则以及向后端池添加资源，来将应用程序 Web 流量定向到特定资源。

本快速入门展示了如何使用 Azure 门户快速创建后端池中有两台虚拟机的应用程序网关。 然后，对它进行测试以确保它正常工作。

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial/)。

如果选择在本地安装并使用 PowerShell，则本教程需要 Azure PowerShell 模块版本 3.6 或更高版本。 若要查找版本，请运行 `Get-Module -ListAvailable AzureRM`。 如果需要进行升级，请参阅 [Install Azure PowerShell module](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)（安装 Azure PowerShell 模块）。 如果在本地运行 PowerShell，则还需运行 `Login-AzureRmAccount -EnvironmentName AzureChinaCloud` 以创建与 Azure 的连接。

## <a name="create-a-resource-group"></a>创建资源组

需要始终在资源组中创建资源。 使用 [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup) 创建 Azure 资源组。 

```azurepowershell
New-AzureRmResourceGroup -Name myResourceGroupAG -Location chinanorth
```

## <a name="create-network-resources"></a>创建网络资源 

需要为应用程序网关创建虚拟网络才能与其他资源进行通信。 在本示例中创建了两个子网：一个用于应用程序网关，另一个用于后端服务器。 

使用 [New-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) 创建子网配置。 使用 [New-AzureRmVirtualNetwork](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetwork) 和子网配置创建虚拟网络。 最后使用 [New-AzureRmPublicIpAddress](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermpublicipaddress) 创建公共 IP 地址。

```azurepowershell
$backendSubnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myAGSubnet `
  -AddressPrefix 10.0.1.0/24
$agSubnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myBackendSubnet `
  -AddressPrefix 10.0.2.0/24
New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Location chinanorth `
  -Name myVNet `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $backendSubnetConfig, $agSubnetConfig
New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroupAG `
  -Location chinanorth `
  -Name myAGPublicIPAddress `
  -AllocationMethod Dynamic
```
## <a name="create-backend-servers"></a>创建后端服务器

在此示例中，将创建两个虚拟机以用作应用程序网关的后端服务器。 

还可以在虚拟机上安装 IIS，以验证是否已成功创建应用程序网关。

### <a name="create-two-virtual-machines"></a>创建两个虚拟机

使用 [New-AzureRmNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermnetworkinterface) 创建网络接口。 使用 [New-AzureRmVMConfig](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvmconfig) 创建虚拟机配置。 运行以下命令时，会提示你输入凭据。 输入*azureuser* 作为用户名，输入 *Azure123456!*  密码。 使用 [New-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvm) 创建虚拟机。

```azurepowershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName myResourceGroupAG -Name myVNet
$cred = Get-Credential
for ($i=1; $i -le 2; $i++)
{
  $nic = New-AzureRmNetworkInterface `
    -Name myNic$i `
    -ResourceGroupName myResourceGroupAG `
    -Location ChinaNorth `
    -SubnetId $vnet.Subnets[1].Id
  $vm = New-AzureRmVMConfig `
    -VMName myVM$i `
    -VMSize Standard_DS2
  $vm = Set-AzureRmVMOperatingSystem `
    -VM $vm `
    -Windows `
    -ComputerName myVM$i `
    -Credential $cred
  $vm = Set-AzureRmVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
  $vm = Add-AzureRmVMNetworkInterface `
    -VM $vm `
    -Id $nic.Id
  $vm = Set-AzureRmVMBootDiagnostics `
    -VM $vm `
    -Disable
  New-AzureRmVM -ResourceGroupName myResourceGroupAG -Location ChinaNorth -VM $vm
  Set-AzureRmVMExtension `
    -ResourceGroupName myResourceGroupAG `
    -ExtensionName IIS `
    -VMName myVM$i `
    -Publisher Microsoft.Compute `
    -ExtensionType CustomScriptExtension `
    -TypeHandlerVersion 1.4 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
    -Location ChinaNorth
}
```

## <a name="create-an-application-gateway"></a>创建应用程序网关

### <a name="create-the-ip-configurations-and-frontend-port"></a>创建 IP 配置和前端端口

使用 [New-AzureRmApplicationGatewayIPConfiguration](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermapplicationgatewayipconfiguration) 创建配置，以将前面创建的子网与应用程序网关相关联。 使用 [New-AzureRmApplicationGatewayFrontendIPConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermapplicationgatewayfrontendipconfig) 创建配置，以将前面也创建的公共 IP 地址分配给应用程序网关。 使用 [New-AzureRmApplicationGatewayFrontendPort](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermapplicationgatewayfrontendport) 分配用于访问应用程序网关的端口 80。

```azurepowershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName myResourceGroupAG -Name myVNet
$pip = Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress 
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

### <a name="create-the-backend-pool"></a>创建后端池

使用 [New-AzureRmApplicationGatewayBackendAddressPool](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermapplicationgatewaybackendaddresspool) 为应用程序网关创建后端池。 使用 [New-AzureRmApplicationGatewayBackendHttpSettings](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermapplicationgatewaybackendhttpsettings) 配置后端池的设置。

```azurepowershell
$address1 = Get-AzureRmNetworkInterface -ResourceGroupName myResourceGroupAG -Name myNic1
$address2 = Get-AzureRmNetworkInterface -ResourceGroupName myResourceGroupAG -Name myNic2
$backendPool = New-AzureRmApplicationGatewayBackendAddressPool `
  -Name myAGBackendPool `
  -BackendIPAddresses $address1.ipconfigurations[0].privateipaddress, $address2.ipconfigurations[0].privateipaddress
$poolSettings = New-AzureRmApplicationGatewayBackendHttpSettings `
  -Name myPoolSettings `
  -Port 80 `
  -Protocol Http `
  -CookieBasedAffinity Enabled `
  -RequestTimeout 120
```

### <a name="create-the-listener-and-add-a-rule"></a>创建侦听器并添加规则

应用程序网关需要侦听器才能适当地将流量路由到后端池。 使用 [New-AzureRmApplicationGatewayHttpListener](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermapplicationgatewayhttplistener) 以及前面创建的前端配置和前端端口创建侦听器。 侦听器需要使用规则来了解哪个后端池使用传入流量。 使用 [New-AzureRmApplicationGatewayRequestRoutingRule](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermapplicationgatewayrequestroutingrule) 创建一个名为 *rule1* 的规则。

```azurepowershell
$defaultlistener = New-AzureRmApplicationGatewayHttpListener `
  -Name myAGListener `
  -Protocol Http `
  -FrontendIPConfiguration $fipconfig `
  -FrontendPort $frontendport
$frontendRule = New-AzureRmApplicationGatewayRequestRoutingRule `
  -Name rule1 `
  -RuleType Basic `
  -HttpListener $defaultlistener `
  -BackendAddressPool $backendPool `
  -BackendHttpSettings $poolSettings
```

### <a name="create-the-application-gateway"></a>创建应用程序网关

现在已创建所需的支持资源，请使用 [New-AzureRmApplicationGatewaySku](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermapplicationgatewaysku) 为应用程序网关指定参数，然后再使用 [New-AzureRmApplicationGateway](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermapplicationgateway) 创建它。

```azurepowershell
$sku = New-AzureRmApplicationGatewaySku `
  -Name Standard_Medium `
  -Tier Standard `
  -Capacity 2
New-AzureRmApplicationGateway `
  -Name myAppGateway `
  -ResourceGroupName myResourceGroupAG `
  -Location chinanorth `
  -BackendAddressPools $backendPool `
  -BackendHttpSettingsCollection $poolSettings `
  -FrontendIpConfigurations $fipconfig `
  -GatewayIpConfigurations $gipconfig `
  -FrontendPorts $frontendport `
  -HttpListeners $defaultlistener `
  -RequestRoutingRules $frontendRule `
  -Sku $sku
```

## <a name="test-the-application-gateway"></a>测试应用程序网关

创建应用程序网关不需要安装 IIS，但本快速入门中安装了它，用来验证应用程序网关是否已成功创建。 使用 [Get-AzureRmPublicIPAddress](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermpublicipaddress) 获取应用程序网关的公共 IP 地址。 复制该公共 IP 地址，并将其粘贴到浏览器的地址栏。

```azurepowershell
Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress
```

![测试应用程序网关](./media/quick-create-powershell/application-gateway-iistest.png)

刷新浏览器时，应该会看到显示了另一 VM 的名称。

## <a name="clean-up-resources"></a>清理资源

首先，探究随应用程序网关创建的资源，当不再需要这些资源时，可以使用 [Remove-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/remove-azurermresourcegroup) 命令来删除资源组、应用程序网关和所有相关资源。

```azurepowershell
Remove-AzureRmResourceGroup -Name myResourceGroupAG
```

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [通过 Azure PowerShell 使用应用程序网关管理 Web 流量](./tutorial-manage-web-traffic-powershell.md)


