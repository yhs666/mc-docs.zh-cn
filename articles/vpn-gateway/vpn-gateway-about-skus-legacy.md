---
title: "传统 Azure 虚拟网关 SKU | Microsoft Docs"
description: "旧版虚拟网关 SKU。"
services: vpn-gateway
documentationcenter: na
author: alexchen2016
manager: digimobile
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 08/01/2017
ms.date: 08/31/2017
ms.author: v-junlch
ms.openlocfilehash: 5374dddda3e31a7bffcbac6afaab97d6cb15ebcb
ms.sourcegitcommit: b69abfec4a5baf598ddb25f640beaa9dd1fdf5a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/01/2017
---
# <a name="working-with-virtual-network-gateway-skus-legacy-skus"></a>使用虚拟网关 SKU（传统 SKU）

本文包含有关传统（旧）虚拟网关 SKU 的信息。 传统 SKU 仍可用于已创建的 VPN 网关的两种部署模型。 经典 VPN 网关继续使用传统 SKU，不管是对于现有网关还是对于新网关。 新建资源管理器 VPN 网关时，使用新的网关 SKU。 有关新 SKU 的信息，请参阅[关于 VPN 网关](vpn-gateway-about-vpngateways.md)。

## <a name="gwsku"></a>网关 SKU

[!INCLUDE [Legacy gateway SKUs](../../includes/vpn-gateway-gwsku-legacy-include.md)]

## <a name="agg"></a>按 SKU 列出的估计聚合吞吐量

[!INCLUDE [Aggregated throughput by legacy SKU](../../includes/vpn-gateway-table-gwtype-legacy-aggtput-include.md)]

## <a name="config"></a>按 SKU 和 VPN 类型划分的受支持配置

[!INCLUDE [Table requirements for old SKUs](../../includes/vpn-gateway-table-requirements-legacy-sku-include.md)]

## <a name="resize"></a>重设网关大小（更改网关 SKU）

可以在同一 SKU 系列内重设网关 SKU 大小。 例如，如果具有标准 SKU，则可重设大小为高性能 SKU。 不能在旧的 SKU 和新的 SKU 系列之间重设 VPN 网关大小。 例如，不能从标准 SKU 调整为 VpnGw2 SKU。 

若要重设经典部署模型的网关 SKU 大小，请使用以下命令：

```powershell
Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance
```

若要重设资源管理器部署模型的网关 SKU 大小，请使用以下命令：

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance
```

## <a name="migrate"></a>迁移到新式网关 SKU

如果使用的是资源管理器部署模型，则可迁移到新式网关 SKU。 如果使用的是经典部署模型，则无法迁移到新式 SKU，必须继续使用旧式 SKU。

[!INCLUDE [Migrate SKU](../../includes/vpn-gateway-migrate-legacy-sku-include.md)]

## <a name="next-steps"></a>后续步骤

有关新式网关 SKU 的详细信息，请参阅[网关 SKU](vpn-gateway-about-vpngateways.md#gwsku)。

有关配置设置的详细信息，请参阅[关于 VPN 网关配置设置](vpn-gateway-about-vpn-gateway-settings.md)。

