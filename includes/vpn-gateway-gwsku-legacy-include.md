---
author: WenJason
ms.service: vpn-gateway
ms.topic: include
origin.date: 11/09/2018
ms.date: 12/24/2018
ms.author: v-jay
ms.openlocfilehash: b256d8def8bf4934c3a079203c7b8ace10c334a2
ms.sourcegitcommit: 0a5a7daaf864ef787197f2b8e62539786b6835b3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/20/2018
ms.locfileid: "53711702"
---
旧版（老版）VPN 网关 SKU 为：

* 基本
* 标准
* HighPerformance

VPN 网关不使用 UltraPerformance 网关 SKU。 有关 UltraPerformance SKU 的信息，请参阅 [ExpressRoute](../articles/expressroute/expressroute-about-virtual-network-gateways.md) 文档。

使用旧版 SKU 时，请考虑以下方面：

* 如果想要使用 PolicyBased VPN 类型，必须使用基本 SKU。 任何其他 SKU 均不支持 PolicyBased VPN（之前称为静态路由）。
* 基本 SKU 不支持 BGP。
* 基本 SKU 不支持 ExpressRoute-VPN 网关共存配置。
* 主动-主动 S2S VPN 网关连接只能在 HighPerformance SKU 上配置。