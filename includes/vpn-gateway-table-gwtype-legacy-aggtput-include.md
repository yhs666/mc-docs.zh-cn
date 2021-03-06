---
title: include 文件
description: include 文件
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: include
origin.date: 03/21/2018
ms.date: 12/24/2018
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: 239488fac5b04f8bc0b37ee8e7b08aaa053b6a61
ms.sourcegitcommit: 0a5a7daaf864ef787197f2b8e62539786b6835b3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/20/2018
ms.locfileid: "53711676"
---
下表显示网关类型和预计的网关 SKU 聚合吞吐量。 此表适用于 Resource Manager 部署模型和经典部署模型。 

定价因网关 SKU 而异。 有关详细信息，请参阅 [VPN 网关定价](https://www.azure.cn/pricing/details/vpn-gateway)。

请注意，UltraPerformance 网关 SKU 未在此表中表示。 有关 UltraPerformance SKU 的信息，请参阅 [ExpressRoute](../articles/expressroute/expressroute-about-virtual-network-gateways.md) 文档。

|  | **VPN 网关吞吐量 (1)** | **VPN 网关最大 IPsec 隧道数 (2)** | **ExpressRoute 网关吞吐量** | **VPN 网关和 ExpressRoute 共存** |
| --- | --- | --- | --- | --- |
| **基本 SKU (3)(5)(6)** |100 Mbps |10 个 |500 Mbps (6) |否 |
| **标准 SKU (4)(5)** |100 Mbps |10 个 |1000 Mbps |是 |
| **高性能 SKU (4)** |200 Mbps |30 |2000 Mbps |是 |


(1) VPN 吞吐量是根据同一 Azure 区域 VNet 之间的度量进行的粗略估计。 不能保证该吞吐量通过 Internet 跨界连接。 这是可能的最大吞吐量度量。

(2) 隧道数量是指 RouteBased VPN。 PolicyBased VPN 只能支持一个站点到站点 VPN 隧道。

(3) 基本 SKU 不支持 BGP。

(4) 此 SKU 不支持 PolicyBased VPN。 仅基本 SKU 支持它们。

(5) 此 SKU 不支持主动-主动 S2S VPN 网关连接。 只有 HighPerformance SKU 才支持主动-主动连接。

(6) 用于 ExpressRoute 的基本 SKU 已弃用。

<!-- ms.date: 09/01/2017 -->