---
title: include 文件
description: include 文件
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: include
origin.date: 03/21/2018
ms.date: 03/04/2019
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: 1e8a33cf0869608b63221ca97648e4d9611652bd
ms.sourcegitcommit: 15a80d044339dab8bce43eb7be110ba01f630056
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/19/2019
ms.locfileid: "69578664"
---
对于当前 SKU（VpnGw1、VpnGw2 和 VPNGW3），如果希望调整网关 SKU 大小以升级到更强大的 SKU，可以使用 `Resize-AzVirtualNetworkGateway` PowerShell cmdlet。 也可以使用此 cmdlet 降级网关 SKU 大小。 如果使用的是基本网关 SKU，[请改用这些说明](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md#resize)来调整网关大小。

以下 PowerShell 示例演示如何将网关 SKU 的大小调整为 VpnGw2。

```powershell
$gw = Get-AzVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku VpnGw2
```

还可以通过转到虚拟网关的“配置”页并从下拉列表中选择其他 SKU，在 Azure 门户中调整网关大小  。
