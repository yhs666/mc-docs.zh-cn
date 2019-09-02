---
title: 旧式 Azure 虚拟网络 VPN 网关 SKU | Microsoft Docs
description: 如何使用旧式虚拟网络网关 SKU；“基本”、“标准”和“高性能”。
services: vpn-gateway
documentationcenter: na
author: WenJason
manager: digimobile
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 06/10/2019
ms.date: 08/05/2019
ms.author: v-jay
ms.openlocfilehash: db73b512d86fb331919b203a4496e69127bca617
ms.sourcegitcommit: 193f49f19c361ac6f49c59045c34da5797ed60ac
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/02/2019
ms.locfileid: "68732370"
---
# <a name="working-with-virtual-network-gateway-skus-legacy-skus"></a>使用虚拟网关 SKU（传统 SKU）

本文包含有关传统（旧）虚拟网关 SKU 的信息。 传统 SKU 仍可用于已创建的 VPN 网关的两种部署模型。 经典 VPN 网关继续使用传统 SKU，不管是对于现有网关还是对于新网关。 新建资源管理器 VPN 网关时，使用新的网关 SKU。 有关新 SKU 的信息，请参阅[关于 VPN 网关](vpn-gateway-about-vpngateways.md)。

## <a name="gwsku"></a>网关 SKU

[!INCLUDE [Legacy gateway SKUs](../../includes/vpn-gateway-gwsku-legacy-include.md)]

可以在 [虚拟网络网关](https://www.azure.cn/pricing/details/vpn-gateway)  部分（位于“虚拟网络网关定价”页上）中查看旧版网关定价。

## <a name="agg"></a>按 SKU 列出的估计聚合吞吐量

[!INCLUDE [Aggregated throughput by legacy SKU](../../includes/vpn-gateway-table-gwtype-legacy-aggtput-include.md)]

## <a name="config"></a>按 SKU 和 VPN 类型划分的受支持配置

[!INCLUDE [Table requirements for old SKUs](../../includes/vpn-gateway-table-requirements-legacy-sku-include.md)]

## <a name="resize"></a>重设网关大小

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

可以在同一 SKU 系列内将网关大小重设到某个网关 SKU。 例如，如果具有标准 SKU，则可重设大小为高性能 SKU。 但不能在旧式 SKU 和新式 SKU 系列之间重设 VPN 网关大小。 例如，不能从标准 SKU 调整为 VpnGw2 SKU 或从标准 SKU 调整为 VpnGw1。

若要重设经典部署模型的网关大小，请使用以下命令：

```powershell
Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance
```

若要使用 PowerShell 重设资源管理器部署模型的网关大小，请使用以下命令：

```powershell
$gw = Get-AzVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance
```
还可以在 Azure 门户中重设网关大小。

## <a name="change"></a>更改到新式网关 SKU

[!INCLUDE [Change to the new SKUs](../../includes/vpn-gateway-gwsku-change-legacy-sku-include.md)]

## <a name="next-steps"></a>后续步骤

有关新式网关 SKU 的详细信息，请参阅[网关 SKU](vpn-gateway-about-vpngateways.md#gwsku)。

有关配置设置的详细信息，请参阅[关于 VPN 网关配置设置](vpn-gateway-about-vpn-gateway-settings.md)。

<!--Update_Description: wording update-->