---
title: Azure Stack 集成系统的边界连接和网络集成 | Microsoft Docs
description: 了解如何在 Azure Stack 集成系统中规划数据中心边界网络连接。
services: azure-stack
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 10/02/2019
ms.date: 11/18/2019
ms.author: v-jay
ms.reviewer: wamota
ms.lastreviewed: 08/30/2018
ms.openlocfilehash: 21cedaa0b239b9c5a86b89a7a1c8d7c4fecbeb44
ms.sourcegitcommit: 7dfb76297ac195e57bd8d444df89c0877888fdb8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2019
ms.locfileid: "74020111"
---
# <a name="border-connectivity"></a>边界连接 
网络集成规划是成功进行 Azure Stack 集成系统部署、操作和管理的重要先决条件。 边界连接规划从选择是否要将动态路由与边界网关协议 (BGP) 一起使用开始。 这需要分配 16 位的 BGP 自治系统编号（公共或专用），或者使用静态路由（在这种情况下会将静态默认路由分配给边界设备）。

> [!IMPORTANT]
> 架顶式 (TOR) 交换机需要在物理接口上配置具有点到点 IP（/30 网络）的第 3 层上行链路。 不支持具有支持 Azure Stack 操作的 TOR 交换机的第 2 层上行链路。

## <a name="bgp-routing"></a>BGP 路由
使用 BGP 等动态路由协议可以保证系统始终会注意到网络更改和便于管理。 为了增强安全性，可以针对 TOR 和边界之间的 BGP 对等互连设置密码。

如下图所示，将使用前缀列表阻止播发 TOR 交换机上的专用 IP 空间。 前缀列表将拒绝播发专用网络，它会作为路由映射应用于 TOR 与边界之间的连接。

Azure Stack 解决方案内运行的软件负载均衡器 (SLB) 将对等互连到 TOR 设备，以便它可以动态播发 VIP 地址。

若要确保用户流量立即以透明方式从故障中恢复，TOR 设备之间配置的 VPC 或 MLAG 允许对主机和 HSRP 或 VRRP 使用多底盘链接聚合以便为 IP 网络提供网络冗余。

![BGP 路由](media/azure-stack-border-connectivity/bgp-routing.png)

## <a name="static-routing"></a>静态路由
静态路由需要额外配置边界设备。 它需要更多的手动干预和管理，以及在任何更改之前进行彻底的分析。 配置错误导致的问题可能需要更多时间进行回退，具体取决于所做的更改。 不建议使用此路由方法，但支持此方法。

若要使用静态路由将 Azure Stack 集成到网络环境，必须连接边界和 TOR 设备之间的所有四个物理链路。 由于静态路由的工作方式，无法保证高可用性。

对于发送到 Azure Stack 内部任何网络的流量，边界设备必须配置有静态路由，指向 TOR 和边界之间的四个 P2P IP 集中的每个 P2P IP 集，但操作仅需要*外部*或公共 VIP 网络。 初始部署需要到 BMC  网络和外部  网络的静态路由。 操作员可以选择在边界中保留静态路由以访问位于 BMC  和基础结构  网络上的管理资源。 添加指向*交换机基础结构*和*交换机管理*网络的静态路由是可选的。

TOR 设备配置有将所有流量发送到边界设备的静态默认路由。 默认规则的一个流量例外是，对于专用空间，将使用应用于 TOR 到边界连接的访问控制列表阻止该流量。

静态路由仅适用于 TOR 与边界交换机之间的上行链路。 机架内使用的是 BGP 动态路由，因为它对于 SLB 和其他组件来说是基本工具，无法禁用或删除。

![静态路由](media/azure-stack-border-connectivity/static-routing.png)

<sup>\*</sup> 在部署后，BMC 网络是可选的。

<sup>\*\*</sup> 交换机基础结构网络是可选的，因为整个网络可以包含在交换机管理网络中。

<sup>\*\*\*</sup> 交换机管理网络是必需的，可以与交换机基础结构网络分开添加。

## <a name="transparent-proxy"></a>透明代理
如果数据中心要求所有流量都使用代理，则必须配置“透明代理”  以便根据策略处理来自机架的所有流量，并分离网络上不同区域之间的流量。

> [!IMPORTANT]
> Azure Stack 解决方案不支持普通 Web 代理。  

透明代理（也称为截获、内联或强制代理）将截获网络层的正常通信，而无需任何特殊的客户端配置。 客户端不需要知道代理是否存在。

![透明代理](media/azure-stack-border-connectivity/transparent-proxy.png)

## <a name="next-steps"></a>后续步骤
[DNS 集成](azure-stack-integrate-dns.md)
