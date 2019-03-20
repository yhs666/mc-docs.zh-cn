---
title: 快速入门 - 使用 Azure 应用程序网关定向 Web 流量 - Azure PowerShell | Microsoft Docs
description: 了解如何使用 Azure PowerShell 创建 Azure 应用程序网关，用以将 Web 流量重定向到后端池中的虚拟机。
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: quickstart
origin.date: 01/11/2019
ms.date: 03/12/2019
ms.author: v-junlch
ms.custom: mvc
ms.openlocfilehash: 4d81b5455971cfd9ca1a6a03680ee1ef99d6df33
ms.sourcegitcommit: d750a61a0e52a41cff5607149e33b6be189075d4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/12/2019
ms.locfileid: "57788748"
---
# <a name="quickstart-direct-web-traffic-with-azure-application-gateway---azure-powershell"></a>快速入门：使用 Azure 应用程序网关定向 Web 流量 - Azure PowerShell

本快速入门介绍如何使用 Azure 门户快速创建应用程序网关。  创建应用程序网关后，可对其进行测试，以确保正常工作。 使用 Azure 应用程序网关可为端口分配侦听器、创建规则以及向后端池添加资源，以便将应用程序 Web 流量定向到特定资源。 为方便演示，本文使用了一种简单的设置，其中包括一个公共前端 IP、一个用于在此应用程序网关上托管单个站点的基本侦听器、两个用于后端池的虚拟机，以及一个基本请求路由规则。

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial/)。

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>先决条件

### <a name="azure-powershell-module"></a>Azure PowerShell 模块

如果选择在本地安装并使用 Azure PowerShell，则本教程需要安装 Azure PowerShell 模块 1.0.0 或更高版本。

1. 若要查找版本，请运行 `Get-Module -ListAvailable Az`。 如果需要进行升级，请参阅 [Install Azure PowerShell module](https://docs.microsoft.com/powershell/azure/install-az-ps)（安装 Azure PowerShell 模块）。 
2. 若要创建与 Azure 的连接，请运行 `Login-AzAccount -Environment AzureChinaCloud`。

### <a name="resource-group"></a>资源组

在 Azure 中，可将相关的资源分配到资源组。 可以使用现有资源组，也可以创建新组。 本示例使用 [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/Az.resources/new-Azresourcegroup) cmdlet 创建新的资源组，如下所示： 

```azurepowershell
New-AzResourceGroup -Name myResourceGroupAG -Location chinanorth
```

### <a name="required-network-resources"></a>所需的网络资源

Azure 需要一个虚拟网络才能在创建的资源之间通信。  应用程序网关子网只能包含应用程序网关。 不允许其他资源。  可为应用程序网关创建新的子网，或者使用现有的子网。 本示例将创建两个子网：一个用于应用程序网关，另一个用于后端服务器。 可根据用例将应用程序网关的前端 IP 配置为公共或专用 IP。 本示例选择了公共前端 IP。

1. 通过调用 [New-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/Az.network/new-Azvirtualnetworksubnetconfig) 创建子网配置。
2. 通过调用 [New-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/Az.network/new-Azvirtualnetwork) 创建带有子网配置的虚拟网络。 
3. 通过调用 [New-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/Az.network/new-Azpublicipaddress) 创建公共 IP 地址。 

```azurepowershell
$agSubnetConfig = New-AzVirtualNetworkSubnetConfig `
  -Name myAGSubnet `
  -AddressPrefix 10.0.1.0/24
$backendSubnetConfig = New-AzVirtualNetworkSubnetConfig `
  -Name myBackendSubnet `
  -AddressPrefix 10.0.2.0/24
New-AzVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Location chinanorth `
  -Name myVNet `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $agSubnetConfig, $backendSubnetConfig
New-AzPublicIpAddress `
  -ResourceGroupName myResourceGroupAG `
  -Location chinanorth `
  -Name myAGPublicIPAddress `
  -AllocationMethod Dynamic
```
### <a name="backend-servers"></a>后端服务器

后端可以包含 NIC、虚拟机规模集、公共 IP、内部 IP、完全限定的域名 (FQDN) 和多租户后端（例如 Azure 应用服务）。 在此示例中，将创建两个虚拟机，供 Azure 用作应用程序网关的后端服务器。 还可以在虚拟机上安装 IIS，以验证 Azure 是否已成功创建应用程序网关。

#### <a name="create-two-virtual-machines"></a>创建两个虚拟机

1. 使用 [New-AzNetworkInterface](https://docs.microsoft.com/powershell/module/Az.network/new-Aznetworkinterface) 创建网络接口。 
2. 使用 [New-AzVMConfig](https://docs.microsoft.com/powershell/module/Az.compute/new-Azvmconfig) 创建虚拟机配置。
3. 使用 [New-AzVM](https://docs.microsoft.com/powershell/module/Az.compute/new-Azvm) 创建虚拟机。

运行以下代码示例来创建虚拟机时，Azure 会提示你输入凭据。 输入*azureuser* 作为用户名，输入 *Azure123456!*  作为密码：
    
```azurepowershell
$vnet   = Get-AzVirtualNetwork -ResourceGroupName myResourceGroupAG -Name myVNet
$subnet = Get-AzVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name myBackendSubnet
$cred = Get-Credential
for ($i=1; $i -le 2; $i++)
{
  $nic = New-AzNetworkInterface `
    -Name myNic$i `
    -ResourceGroupName myResourceGroupAG `
    -Location ChinaNorth `
    -SubnetId $subnet.Id
  $vm = New-AzVMConfig `
    -VMName myVM$i `
    -VMSize Standard_DS2_v2
  Set-AzVMOperatingSystem `
    -VM $vm `
    -Windows `
    -ComputerName myVM$i `
    -Credential $cred
  Set-AzVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
  Add-AzVMNetworkInterface `
    -VM $vm `
    -Id $nic.Id
  Set-AzVMBootDiagnostics `
    -VM $vm `
    -Disable
  New-AzVM -ResourceGroupName myResourceGroupAG -Location ChinaNorth -VM $vm
  Set-AzVMExtension `
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

1. 使用 [New-AzApplicationGatewayIPConfiguration](https://docs.microsoft.com/powershell/module/Az.network/new-Azapplicationgatewayipconfiguration) 创建配置，将创建的子网与应用程序网关相关联。 
2. 使用 [New-AzApplicationGatewayFrontendIPConfig](https://docs.microsoft.com/powershell/module/Az.network/new-Azapplicationgatewayfrontendipconfig) 创建配置，以将前面创建的公共 IP 地址分配给应用程序网关。 
3. 使用 [New-AzApplicationGatewayFrontendPort](https://docs.microsoft.com/powershell/module/Az.network/new-Azapplicationgatewayfrontendport) 分配用于访问应用程序网关的端口 80。

```azurepowershell
$vnet   = Get-AzVirtualNetwork -ResourceGroupName myResourceGroupAG -Name myVNet
$subnet = Get-AzVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name myAGSubnet
$pip    = Get-AzPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress 
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

### <a name="create-the-backend-pool"></a>创建后端池

1. 使用 [New-AzApplicationGatewayBackendAddressPool](https://docs.microsoft.com/powershell/module/Az.network/new-Azapplicationgatewaybackendaddresspool) 为应用程序网关创建后端池。 
2. 使用 [New-AzApplicationGatewayBackendHttpSettings](https://docs.microsoft.com/powershell/module/Az.network/new-Azapplicationgatewaybackendhttpsettings) 配置后端池的设置。

```azurepowershell
$address1 = Get-AzNetworkInterface -ResourceGroupName myResourceGroupAG -Name myNic1
$address2 = Get-AzNetworkInterface -ResourceGroupName myResourceGroupAG -Name myNic2
$backendPool = New-AzApplicationGatewayBackendAddressPool `
  -Name myAGBackendPool `
  -BackendIPAddresses $address1.ipconfigurations[0].privateipaddress, $address2.ipconfigurations[0].privateipaddress
$poolSettings = New-AzApplicationGatewayBackendHttpSettings `
  -Name myPoolSettings `
  -Port 80 `
  -Protocol Http `
  -CookieBasedAffinity Enabled `
  -RequestTimeout 120
```

### <a name="create-the-listener-and-add-a-rule"></a>创建侦听器并添加规则

Azure 需要一个侦听器才能使应用程序网关以适当方式将流量路由到后端池。 Azure 还需要一项规则，使侦听器了解将哪个后端池用于传入流量。 

1. 使用 [New-AzApplicationGatewayHttpListener](https://docs.microsoft.com/powershell/module/Az.network/new-Azapplicationgatewayhttplistener) 以及前面创建的前端配置和前端端口创建侦听器。 
2. 使用 [New-AzApplicationGatewayRequestRoutingRule](https://docs.microsoft.com/powershell/module/Az.network/new-Azapplicationgatewayrequestroutingrule) 创建一个名为 *rule1* 的规则。 

```azurepowershell
$defaultlistener = New-AzApplicationGatewayHttpListener `
  -Name myAGListener `
  -Protocol Http `
  -FrontendIPConfiguration $fipconfig `
  -FrontendPort $frontendport
$frontendRule = New-AzApplicationGatewayRequestRoutingRule `
  -Name rule1 `
  -RuleType Basic `
  -HttpListener $defaultlistener `
  -BackendAddressPool $backendPool `
  -BackendHttpSettings $poolSettings
```

### <a name="create-the-application-gateway"></a>创建应用程序网关

创建所需的支持资源以后，即可创建应用程序网关：

1. 使用 [New-AzApplicationGatewaySku](https://docs.microsoft.com/powershell/module/Az.network/new-Azapplicationgatewaysku) 指定应用程序网关的参数。
2. 使用 [New-AzApplicationGateway](https://docs.microsoft.com/powershell/module/Az.network/new-Azapplicationgateway) 创建应用程序网关。

```azurepowershell
$sku = New-AzApplicationGatewaySku `
  -Name Standard_Medium `
  -Tier Standard `
  -Capacity 2
New-AzApplicationGateway `
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

虽然不需 IIS 即可创建应用程序网关，但本快速入门中安装了它，用来验证 Azure 是否已成功创建应用程序网关。 使用 IIS 测试应用程序网关：

1. 运行 [Get-AzPublicIPAddress](https://docs.microsoft.com/powershell/module/Az.network/get-Azpublicipaddress) 获取应用程序网关的公共 IP 地址。 
2. 复制该公共 IP 地址，并将其粘贴到浏览器的地址栏。 刷新浏览器时，应该会看到虚拟机的名称。 有效的响应中会确认已成功创建应用程序网关，并且它可以成功连接到后端。

```azurepowershell
Get-AzPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress
```

![测试应用程序网关](./media/quick-create-powershell/application-gateway-iistest.png)


## <a name="clean-up-resources"></a>清理资源

如果不再需要通过应用程序网关创建的资源，请删除资源组。 删除资源组时，也会删除应用程序网关和及其所有的相关资源。 

若要删除资源组，请调用 [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/Az.resources/remove-Azresourcegroup) cmdlet，如下所示：

```azurepowershell
Remove-AzResourceGroup -Name myResourceGroupAG
```

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [通过 Azure PowerShell 使用应用程序网关管理 Web 流量](./tutorial-manage-web-traffic-powershell.md)


<!-- Update_Description: code and links update -->