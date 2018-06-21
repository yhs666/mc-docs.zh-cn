---
title: Azure Stack 集成系统的边界连接网络集成注意事项 | Microsoft Docs
description: 了解可以执行哪些操作来为多节点 Azure Stack 规划数据中心边界网络连接。
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 01/31/2018
ms.date: 03/01/2018
ms.author: v-junlch
ms.reviewer: wamota
ms.openlocfilehash: 1874efa97affc990db2219546912c9691069d60e
ms.sourcegitcommit: 34925f252c9d395020dc3697a205af52ac8188ce
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
ms.locfileid: "29731044"
---
# <a name="border-connectivity"></a>边界连接 
网络集成规划是成功进行 Azure Stack 集成系统部署、操作和管理的重要先决条件。 边界连接规划首先是要选择是否将动态路由与边界网关协议 (BGP) 配合使用。 这需要分配 16 位的 BGP 自治系统编号（公共或专用），或者使用静态路由（在这种情况下会将静态默认路由分配给边界设备）。

> [!IMPORTANT]
> 架顶式 (TOR) 交换机需要在物理接口上配置具有点到点 IP（/30 网络）的第 3 层上行链路。 不支持将第 2 层上行链路用于支持 Azure Stack 操作的 TOR 交换机。 

## <a name="bgp-routing"></a>BGP 路由
使用 BGP 等动态路由协议可以保证系统始终会注意到网络更改和便于管理。 

如下图所示，将使用前缀列表限制播发 TOR 交换机上的专用 IP 空间。 前缀列表将拒绝专用 IP 子网并将其作为路由映射应用于 TOR 与边界之间的连接。

Azure Stack 解决方案内运行的软件负载均衡器 (SLB) 将对等互连到 TOR 设备，以便它可以动态播发 VIP 地址。

若要确保用户流量立即以透明方式从故障中恢复，TOR 设备之间配置的 VPC 或 MLAG 允许对主机和 HSRP 或 VRRP 使用多底盘链接聚合以便为 IP 网络提供网络冗余。

![BGP 路由](./media/azure-stack-border-connectivity/bgp-routing.png)

## <a name="static-routing"></a>静态路由
使用静态路由将更固定的配置添加到边界设备和 TOR 设备。 这需要在进行任何更改之前进行全面分析。 配置错误导致的问题可能需要更多时间进行回退，具体取决于所做的更改。 它不是建议的路由方法，但受支持。

若要使用此方法将 Azure Stack 集成到网络环境，必须针对发送到外部网络、公共 VIP 的流量为边界设备配置指向 TOR 设备的静态路由。

必须为 TOR 设备配置将所有流量发送到边界设备的静态默认路由。 此规则的一个流量例外是，对于专用空间将使用应用于 TOR 到边界连接的访问控制列表阻止该流量。

其他所有内容应与第一种方法相同。 BGP 动态路由将仍在机架内使用，因为它对于 SLB 和其他组件来说是基本工具，无法禁用或删除。

![静态路由](./media/azure-stack-border-connectivity/static-routing.png)

## <a name="transparent-proxy"></a>透明代理
如果数据中心要求所有流量都使用代理，则必须配置“透明代理”以便根据策略处理来自机架的所有流量，并分离网络上不同区域之间的流量。

> [!IMPORTANT]
> Azure Stack 解决方案不支持普通 Web 代理。  

透明代理（也称为截获、内联或强制代理）将截获网络层的正常通信，而无需任何特殊的客户端配置。 客户端不需要注意代理是否存在。

![透明代理](./media/azure-stack-border-connectivity/transparent-proxy.png)

## <a name="next-steps"></a>后续步骤
[DNS 集成](azure-stack-integrate-dns.md)

