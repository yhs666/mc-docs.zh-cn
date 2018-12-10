---
 title: include 文件 description: include 文件 services: vpn-gateway author: cherylmc ms.service: vpn-gateway ms.topic: include origin.date: 03/21/2018 ms.date: 05/08/2018 ms.author: v-junlch ms.custom: include file
---

对于当前 SKU（VpnGw1、VpnGw2 和 VPNGW3），如果希望调整网关 SKU 大小以升级到更强大的 SKU，可以使用 `Resize-AzureRmVirtualNetworkGateway` PowerShell cmdlet。 也可以使用此 cmdlet 降级网关 SKU 大小。 如果使用的是基本网关 SKU，[请改用这些说明](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md#resize)来调整网关大小。

以下 PowerShell 示例演示如何将网关 SKU 的大小调整为 VpnGw2。

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku VpnGw2
```

还可以通过转到虚拟网关的“配置”页并从下拉列表中选择其他 SKU，在 Azure 门户中调整网关大小。

<!-- ms.date: 05/08/2018 -->