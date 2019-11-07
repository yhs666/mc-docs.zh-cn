---
title: 使用 Azure PowerShell 配置 Web 应用程序防火墙 v2 自定义规则
description: 了解如何使用 Azure PowerShell 配置 Web 应用程序防火墙 v2 自定义规则
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
origin.date: 06/18/2019
ms.date: 10/23/2019
ms.author: v-junlch
ms.openlocfilehash: 6638282a7a7618eb880be4fd61a953bf76cd6198
ms.sourcegitcommit: 24b69c0a22092c64c6c3db183bb0655a23340420
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2019
ms.locfileid: "72798487"
---
# <a name="configure-web-application-firewall-v2-custom-rules-by-using-azure-powershell"></a>使用 Azure PowerShell 配置 Web 应用程序防火墙 v2 自定义规则

<!--- If you make any changes to the PowerShell in this article, also make the change in the corresponding Sample file: azure-docs-powershell-samples/application-gateway/waf-rules/waf-custom-rules.ps1 --->

使用自定义规则，可以创建自己的规则，这些规则对通过 Web 应用程序防火墙 (WAF) 传递的每个请求进行评估。 这些规则的优先级高于托管规则集中的其他规则。 为了允许完全自定义，自定义规则具有一个操作（允许或阻止）、一个匹配条件和一个运算符。

本文创建一个使用自定义规则的 Azure 应用程序网关 WAF v2。 如果请求标头包含用户代理 *evilbot*，该自定义规则会阻止流量。

若要查看更多自定义规则示例，请参阅[创建和使用自定义 Web 应用程序防火墙规则](create-custom-waf-rules.md)。

若要在一个可复制、粘贴和运行的连续脚本中运行本文中的 Azure PowerShell 代码，请参阅 [Azure 应用程序网关 PowerShell 示例](powershell-samples.md)。

## <a name="prerequisites"></a>先决条件
* 如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。

* 需要 Azure PowerShell 模块。 如果选择在本地安装并使用 Azure PowerShell，则此脚本需要 Azure PowerShell 模块版本 2.1.0 或更高版本。 请执行以下操作：

  1. 若要查找版本，请运行 `Get-Module -ListAvailable Az`。 如果需要进行升级，请参阅 [Install Azure PowerShell module](https://docs.microsoft.com/powershell/azure/install-az-ps)（安装 Azure PowerShell 模块）。
  2. 若要创建与 Azure 的连接，请运行 `Connect-AzAccount -Environment AzureChinaCloud`。

## <a name="example-script"></a>示例脚本

### <a name="set-up-variables"></a>设置变量

运行以下代码：

```azurepowershell
$rgname = "CustomRulesTest"

$location = "China North"

$appgwName = "WAFCustomRules"
```

### <a name="create-a-resource-group"></a>创建资源组

运行以下代码：

```azurepowershell
$resourceGroup = New-AzResourceGroup -Name $rgname -Location $location
```

### <a name="create-a-virtual-network"></a>创建虚拟网络

运行以下代码：

```azurepowershell
$sub1 = New-AzVirtualNetworkSubnetConfig -Name "appgwSubnet" -AddressPrefix "10.0.0.0/24"

$sub2 = New-AzVirtualNetworkSubnetConfig -Name "backendSubnet" -AddressPrefix "10.0.1.0/24"

$vnet = New-AzvirtualNetwork -Name "Vnet1" -ResourceGroupName $rgname -Location $location `
  -AddressPrefix "10.0.0.0/16" -Subnet @($sub1, $sub2)
```

### <a name="create-a-static-public-vip"></a>创建静态公共 VIP

运行以下代码：

```azurepowershell
$publicip = New-AzPublicIpAddress -ResourceGroupName $rgname -name "AppGwIP" `
  -location $location -AllocationMethod Static -Sku Standard
```

### <a name="create-a-pool-and-front-end-port"></a>创建池和前端端口

运行以下代码：

```azurepowershell
$gwSubnet = Get-AzVirtualNetworkSubnetConfig -Name "appgwSubnet" -VirtualNetwork $vnet

$gipconfig = New-AzApplicationGatewayIPConfiguration -Name "AppGwIpConfig" -Subnet $gwSubnet

$fipconfig01 = New-AzApplicationGatewayFrontendIPConfig -Name "fipconfig" -PublicIPAddress $publicip

$pool = New-AzApplicationGatewayBackendAddressPool -Name "pool1" `
  -BackendIPAddresses testbackend1.chinanorth.chinacloudapp.cn, testbackend2.chinanorth.chinacloudapp.cn

$fp01 = New-AzApplicationGatewayFrontendPort -Name "port1" -Port 80
```

### <a name="create-a-listener-http-setting-rule-and-autoscale"></a>创建侦听器、HTTP 设置、规则和自动缩放

运行以下代码：

```azurepowershell
$listener01 = New-AzApplicationGatewayHttpListener -Name "listener1" -Protocol Http `
  -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01

$poolSetting01 = New-AzApplicationGatewayBackendHttpSettings -Name "setting1" -Port 80 `
  -Protocol Http -CookieBasedAffinity Disabled

$rule01 = New-AzApplicationGatewayRequestRoutingRule -Name "rule1" -RuleType basic `
  -BackendHttpSettings $poolSetting01 -HttpListener $listener01 -BackendAddressPool $pool

$autoscaleConfig = New-AzApplicationGatewayAutoscaleConfiguration -MinCapacity 3

$sku = New-AzApplicationGatewaySku -Name WAF_v2 -Tier WAF_v2
```

### <a name="create-the-custom-rule-and-apply-it-to-waf-policy"></a>创建自定义规则并将其应用于 WAF 策略

运行以下代码：

```azurepowershell
$variable = New-AzApplicationGatewayFirewallMatchVariable -VariableName RequestHeaders -Selector User-Agent

$condition = New-AzApplicationGatewayFirewallCondition -MatchVariable $variable -Operator Contains -MatchValue "evilbot" -Transform Lowercase -NegationCondition $False  

$rule = New-AzApplicationGatewayFirewallCustomRule -Name blockEvilBot -Priority 2 -RuleType MatchRule -MatchCondition $condition -Action Block

$wafPolicy = New-AzApplicationGatewayFirewallPolicy -Name wafPolicy -ResourceGroup $rgname -Location $location -CustomRule $rule

$wafConfig = New-AzApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode "Prevention"
```

### <a name="create-an-application-gateway"></a>创建应用程序网关

运行以下代码：

```azurepowershell
$appgw = New-AzApplicationGateway -Name $appgwName -ResourceGroupName $rgname `
   -Location $location -BackendAddressPools $pool `
   -BackendHttpSettingsCollection  $poolSetting01 `
   -GatewayIpConfigurations $gipconfig -FrontendIpConfigurations $fipconfig01 `
   -FrontendPorts $fp01 -HttpListeners $listener01 `
   -RequestRoutingRules $rule01 -Sku $sku -AutoscaleConfiguration $autoscaleConfig `
   -WebApplicationFirewallConfig $wafConfig `
   -FirewallPolicy $wafPolicy
```

## <a name="next-steps"></a>后续步骤

[详细了解 Web 应用程序防火墙](waf-overview.md)

<!-- Update_Description: wording update -->