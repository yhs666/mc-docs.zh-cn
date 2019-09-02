---
title: include 文件
description: include 文件
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: include
origin.date: 03/21/2018
ms.date: 01/21/2019
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: b958e2ae67ce901b17a15c9a83b18bb923f1d9c7
ms.sourcegitcommit: 235c6c8a11af703474236c379aa6310e84ff03a3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/12/2019
ms.locfileid: "63844498"
---
需要为两件事情付费：虚拟网络网关的小时计算成本和来自虚拟网络网关的出口数据传输。 可在 [定价](https://www.azure.cn/pricing/details/vpn-gateway) 页上找到定价信息。

**虚拟网络网关计算成本**<br>每个虚拟网络网关都有每小时计算成本。 该价格取决于创建虚拟网络网关时指定的网关 SKU。 成本与网关本身以及流经网关的数据传输相关。 主动-主动设置的成本与主动-被动设置的成本相同。

**数据传输成本**<br>数据传输成本根据源虚拟网络网关的出口流量计算。

* 如果将流量发送到本地 VPN 设备，会以 Internet 出站数据传输费率收费。
* 如果要在不同区域的虚拟网络之间发送流量，定价因区域而异。
* 如果仅在同一区域的虚拟网络之间发送流量，不会有数据成本。 同一区域的 VNet 之间的流量是免费的。