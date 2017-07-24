---
title: "将 Azure VPN 网关连接到多个基于策略的本地 VPN 设备：Azure Resource Manager：PowerShell | Azure"
description: "本文逐步讲解如何使用 Azure Resource Manager 和 PowerShell 将 Azure 基于路由的 VPN 网关配置到多个基于策略的 VPN 设备。"
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 05/27/2017
ms.date: 07/17/2017
ms.author: v-dazen
ms.openlocfilehash: a115e7116e637a3d06facab618b4dd3ae106d411
ms.sourcegitcommit: f2f4389152bed7e17371546ddbe1e52c21c0686a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/14/2017
---
# <a name="connect-azure-vpn-gateways-to-multiple-on-premises-policy-based-vpn-devices-using-powershell"></a>使用 PowerShell 将 Azure VPN 网关连接到多个基于策略的本地 VPN 设备

本文逐步讲解如何利用 S2S VPN 连接上的自定义 IPsec/IKE 策略，将 Azure 基于路由的 VPN 网关配置到多个基于策略的本地 VPN 设备。

## <a name="about-policy-based-and-route-based-vpn-gateways"></a>关于基于策略和基于路由的 VPN 网关

基于策略与基于路由的 VPN 设备的差异体现在如何在连接上设置 IPsec 流量选择器：

* **基于策略**的 VPN 设备使用两个网络的前缀组合来定义如何通过 IPsec 隧道加密/解密流量。 它通常构建在执行数据包筛选的防火墙设备的基础之上。 IPsec 隧道加密和解密将添加到数据包筛选及处理引擎。
* **基于路由**的 VPN 设备使用任意到任意（通配）流量选择器，允许路由/转发表将流量定向到不同的 IPsec 隧道。 它通常构建在其中每个 IPsec 隧道建模为网络接口或 VTI（虚拟隧道接口）的路由器平台的基础之上。

下图突出显示了这两种模型：

### <a name="policy-based-vpn-example"></a>基于策略的 VPN 示例
![policybased](./media/vpn-gateway-connect-multiple-policybased-rm-ps/policybasedmultisite.png)

### <a name="route-based-vpn-example"></a>基于路由的 VPN 示例
![routebased](./media/vpn-gateway-connect-multiple-policybased-rm-ps/routebasedmultisite.png)

### <a name="azure-support-for-policy-based-vpn"></a>Azure 对基于策略的 VPN 的支持
目前，Azure 支持两种模式的 VPN 网关：基于路由的 VPN 网关和基于策略的 VPN 网关。 它们构建在不同内部平台的基础之上，因此在规范上有所差别：

|                          | **PolicyBased VPN 网关** | **RouteBased VPN 网关**               |
| ---                      | ---                         | ---                                      |
| **Azure 网关 SKU**    | 基本                       | 基本、标准、高性能         |
| **IKE 版本**          | IKEv1                       | IKEv2                                    |
| **最大S2S 连接** | **1**                       | 基本/标准：10<br> 高性能：30 |
|                          |                             |                                          |

使用自定义 IPsec/IKE 策略时，现在可以通过选项“**PolicyBasedTrafficSelectors**”将 Azure 基于路由的 VPN 网关配置为使用基于前缀的流量选择器，以便连接到基于策略的本地 VPN 设备。 使用此功能可从 Azure 虚拟网络和 VPN 网关连接到多个基于策略的本地 VPN/防火墙设备，消除只能在当前 Azure 基于策略的 VPN 网关中建立单一连接的限制。

> [!IMPORTANT]
> 1. 若要启用此连接，基于策略的本地 VPN 设备必须支持使用 **IKEv2** 连接到 Azure 基于路由的 VPN 网关。 请查阅 VPN 设备规范。
> 2. 使用此机制通过基于策略的 VPN 设备连接的本地网络只能连接到 Azure 虚拟网络；**无法通过相同的 Azure VPN 网关将数据传输到其他本地网络或虚拟网络**。
> 3. 配置选项是自定义 IPsec/IKE 连接策略的一部分。 如果启用基于策略的流量选择器选项，则必须指定完整的策略（IPsec/IKE 加密和完整性算法、密钥强度和 SA 生存期）。

下图显示了使用基于策略的选项时，通过 Azure VPN 网关的传输路由为何不起作用。

![policybasedtransit](./media/vpn-gateway-connect-multiple-policybased-rm-ps/policybasedtransit.png)

如图所示，Azure VPN 网关将使用从虚拟网络到每个本地网络前缀而不是跨连接前缀的流量选择器。 例如，本地站点 2、站点 3 和站点 4 各自都可与 VNet1 通信，但无法通过 Azure VPN 网关相互连接。 图中显示，在这种配置下，无法在 Azure VPN 网关中使用跨连接流量选择器。

## <a name="configure-policy-based-traffic-selectors-on-a-connection"></a>在连接上配置基于策略的流量选择器

本文中的说明沿用[为 S2S 或 VNet 到 VNet 连接配置 IPsec/IKE 策略](vpn-gateway-ipsecikepolicy-rm-powershell.md)中所述的相同示例来建立 S2S VPN 连接，如下图所示：

![s2s-policy](./media/vpn-gateway-connect-multiple-policybased-rm-ps/s2spolicypb.png)

启用此连接的工作流如下：
1. 为跨界连接创建虚拟网络、VPN 网关和本地网络网关
2. 创建 IPsec/IKE 策略
3. 创建 S2S 或 VNet 到 VNet 连接时应用该策略，并在连接上**启用基于策略的流量选择器**
4. 如果已创建连接，则可以在现有连接上应用或更新策略

## <a name="enable-policy-based-traffic-selectors-on-a-connection"></a>在连接上启用基于策略的流量选择器

在学习本部分之前，请确保已完成[“配置 IPsec/IKE 策略”一文的“第 3 部分”](vpn-gateway-ipsecikepolicy-rm-powershell.md)。 以下示例使用相同的参数和步骤。

### <a name="step-1---create-the-virtual-network-vpn-gateway-and-local-network-gateway"></a>步骤 1 - 创建虚拟网络、VPN 网关和本地网络网关

#### <a name="1-declare-your-variables--connect-to-your-subscription"></a>1.声明变量并连接到订阅
对于本练习，我们首先要声明变量。 请务必在配置生产环境时，使用自己的值来替换该值。

```powershell
$Sub1          = "<YourSubscriptionName>"
$RG1           = "TestPolicyRG1"
$Location1     = "China East"
$VNetName1     = "TestVNet1"
$FESubName1    = "FrontEnd"
$BESubName1    = "Backend"
$GWSubName1    = "GatewaySubnet"
$VNetPrefix11  = "10.11.0.0/16"
$VNetPrefix12  = "10.12.0.0/16"
$FESubPrefix1  = "10.11.0.0/24"
$BESubPrefix1  = "10.12.0.0/24"
$GWSubPrefix1  = "10.12.255.0/27"
$DNS1          = "8.8.8.8"
$GWName1       = "VNet1GW"
$GW1IPName1    = "VNet1GWIP1"
$GW1IPconf1    = "gw1ipconf1"
$Connection16  = "VNet1toSite6"

$LNGName6      = "Site6"
$LNGPrefix61   = "10.61.0.0/16"
$LNGPrefix62   = "10.62.0.0/16"
$LNGIP6        = "131.107.72.22"
```
确保切换到 PowerShell 模式，以便使用Resource Manager cmdlet。 有关详细信息，请参阅[将 Windows PowerShell 与 Resource Manager 配合使用](../powershell-azure-resource-manager.md)。

打开 PowerShell 控制台并连接到你的帐户。 使用下面的示例来帮助你连接：

```powershell
Login-AzureRmAccount -EnvironmentName AzureChinaCloud
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="2-create-the-virtual-network-vpn-gateway-and-local-network-gateway"></a>2.创建虚拟网络、VPN 网关和本地网络网关
以下示例创建包含三个子网的虚拟网络 TestVNet1，并创建 VPN 网关。 替换值时，请务必始终将网关子网特意命名为 GatewaySubnet。 如果命名为其他名称，网关创建将会失败。

```powershell
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
$besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

$gw1pip1    = New-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
$vnet1      = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gw1ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf1 -Subnet $subnet1 -PublicIpAddress $gw1pip1

New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gw1ipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance

New-AzureRmLocalNetworkGateway -Name $LNGName6 -ResourceGroupName $RG1 -Location $Location1 -GatewayIpAddress $LNGIP6 -AddressPrefix $LNGPrefix61,$LNGPrefix62
```

### <a name="step-2---creat-a-s2s-vpn-connection-with-an-ipsecike-policy"></a>步骤 2 - 使用 IPsec/IKE 策略创建 S2S VPN 连接

#### <a name="1-create-an-ipsecike-policy"></a>1.创建 IPsec/IKE 策略

> [!IMPORTANT]
> 需要创建一个 IPsec/IKE 策略才能在连接上启用“UsePolicyBasedTrafficSelectors”选项。

以下示例脚本使用以下算法和参数创建 IPsec/IKE 策略：
* IKEv2：AES256、SHA384、DHGroup24
* IPsec：AES256、SHA256、PFS24，SA 生存期为 3600 秒，大小为 2048KB

```powershell
$ipsecpolicy6 = New-AzureRmIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption AES256 -IpsecIntegrity SHA256 -PfsGroup PFS24 -SALifeTimeSeconds 3600 -SADataSizeKilobytes 2048
```

#### <a name="2-create-the-s2s-vpn-connection-with-policy-based-traffic-selectors-and-ipsecike-policy"></a>2.使用基于策略的流量选择器和 IPsec/IKE 策略创建 S2S VPN 连接
创建 S2S VPN 连接并应用前面创建的 IPsec/IKE 策略。 请注意用于在连接上启用基于策略的流量选择器的附加参数“-UsePolicyBasedTrafficSelectors $True”。

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng6 = Get-AzureRmLocalNetworkGateway  -Name $LNGName6 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng6 -Location $Location1 -ConnectionType IPsec -UsePolicyBasedTrafficSelectors $True -IpsecPolicies $ipsecpolicy6 -SharedKey 'AzureA1b2C3'
```

完成这些步骤后，S2S VPN 连接将使用上面定义的 IPsec/IKE 策略，并在连接上启用基于策略的流量选择器。 可以重复相同的步骤，通过相同的 Azure VPN 网关将更多连接添加到其他基于策略的本地 VPN 设备。

## <a name="update-policy-based-traffic-selectors-for-a-connection"></a>更新连接的基于策略的流量选择器
最后一个部分介绍如何更新现有 S2S VPN 连接的基于策略的流量选择器选项。

### <a name="1-get-the-connection"></a>1.获取连接
获取连接资源。

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1
```

### <a name="2-check-the-policy-based-traffic-selectors-option"></a>2.检查基于策略的流量选择器选项
以下代码行将显示是否对连接使用了基于策略的流量选择器：

```powershell
$connection6.UsePolicyBasedTrafficSelectors
```

如果在连接上配置了基于策略的流量选择器，该代码行将返回“True”；否则返回“False”。

### <a name="3-update-the-policy-based-traffic-selectors-on-a-connection"></a>3.在连接上更新基于策略的流量选择器
获取连接资源后，可以启用或禁用该选项。

#### <a name="disable-usepolicybasedtrafficselectors"></a>禁用 UsePolicyBasedTrafficSelectors
以下示例禁用基于策略的流量选择器选项，但将 IPsec/IKE 策略保持不变：

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -UsePolicyBasedTrafficSelectors $False
```

#### <a name="enable-usepolicybasedtrafficselectors"></a>启用 UsePolicyBasedTrafficSelectors
以下示例启用基于策略的流量选择器选项，但将 IPsec/IKE 策略保持不变：

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -UsePolicyBasedTrafficSelectors $True
```

## <a name="next-steps"></a>后续步骤
连接完成后，即可将虚拟机添加到虚拟网络。 请参阅[创建虚拟机](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)以获取相关步骤。

另请参阅[为 S2S VPN 或 VNet 到 VNet 连接配置 IPsec/IKE 策略](vpn-gateway-ipsecikepolicy-rm-powershell.md)，了解有关自定义 IPsec/IKE 策略的详细信息。