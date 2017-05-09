---
title: "ExpressRoute 路由要求 | Azure"
description: "本页提供有关为 ExpressRoute 线路配置和管理路由的详细要求。"
documentationCenter: na
services: expressroute
authors: osamazia
manager: ganesr
editor: 
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/03/2017
ms.author: osamam
ms.translationtype: Human Translation
ms.sourcegitcommit: a114d832e9c5320e9a109c9020fcaa2f2fdd43a9
ms.openlocfilehash: c389522d8e4fa2f4ac2cf0133606353a6d8fb1c1
ms.contentlocale: zh-cn
ms.lasthandoff: 04/14/2017

---

# <a name="expressroute-routing-requirements"></a>ExpressRoute 路由要求  

若要使用 ExpressRoute 连接到 Microsoft 云服务，需要设置并管理路由。 某些连接服务提供商以托管服务形式提供路由的设置和管理。 请咨询连接服务提供商，以确定他们是否提供此类服务。 如果不提供，则必须遵守以下要求。 

有关需要在设置后才能建立连接的路由会话的说明，请参阅[线路和路由域](./expressroute-circuit-peerings.md)一文。

> [!NOTE]
> Microsoft 不支持任何高可用性配置的路由器冗余协议（如 HSRP 和 VRRP）。 我们依赖每个对等互连的一组冗余 BGP 会话来获得高可用性。
> 
> 

## <a name="ip-addresses-used-for-peerings"></a>用于对等互连的 IP 地址
需要保留一些 IP 地址块用于配置网络与 Microsoft Enterprise Edge (MSEE) 路由器之间的路由。 本部分提供了要求列表，并介绍有关如何获取和使用这些 IP 地址的规则。

### <a name="ip-addresses-used-for-azure-private-peering"></a>用于 Azure 专用对等互连的 IP 地址
可以使用专用 IP 地址或公共 IP 地址配置对等互连。 用于配置路由的地址范围不得与用于在 Azure 中创建虚拟网络的地址范围重叠。 

 - 必须为路由接口保留一个 /29 子网或两个 /30 子网。
 - 用于路由的子网可以是专用 IP 地址或公共 IP 地址。
 - 子网不得与客户保留用于 Microsoft 云的范围冲突。
 - 如果使用 /29 子网，它将拆分成两个 /30 子网。 
     - 第一个 /30 子网用于主链路，第二个 /30 子网用于辅助链路。
     - 对于每个 /30 子网，必须在路由器上使用 /30 子网的第一个 IP 地址。 Microsoft 使用 /30 子网的第二个 IP 地址设置 BGP 会话。
     - [可用性 SLA](https://azure.microsoft.com/support/legal/sla/) 只有在设置两个 BGP 会话后才有效。  

#### <a name="example-for-private-peering"></a>专用对等互连示例

如果选择使用 a.b.c.d/29 设置对等互连，它将拆分成两个 /30 子网。 在以下示例中，可以了解 a.b.c.d/29 子网的用法。 

a.b.c.d/29 拆分成 a.b.c.d/30 和 a.b.c.d+4/30，并通过预配 API 传递到 Microsoft。 将使用 a.b.c.d+1 作为主要 PE 的 VRF IP，而 Microsoft 将使用 a.b.c.d+2 作为主要 MSEE 的 VRF IP。 将使用 b.c.d+5 作为辅助 PE 的 VRF IP，而 Microsoft 将使用 a.b.c.d+6 作为辅助 MSEE 的 VRF IP。

假设选择 192.168.100.128/29 设置专用对等互连。 192.168.100.128/29 包括从 192.168.100.128 到 192.168.100.135 的地址，其中：

- 192.168.100.128/30 分配给 link1（提供商使用 192.168.100.129，Microsoft 使用 192.168.100.130）。
- 192.168.100.132/30 分配给 link2（提供商使用 192.168.100.133，Microsoft 使用 192.168.100.134）。

### <a name="ip-addresses-used-for-azure-public-and-microsoft-peering"></a>用于 Azure 公共网络与 Microsoft 对等互连的 IP 地址
必须使用自己的公共 IP 地址设置 BGP 会话。 Microsoft 必须能够通过路由 Internet 注册表和 Internet 路由注册表验证 IP 地址的所有权。 

- 必须使用唯一的 /29 子网或两个 /30 子网为每条 ExpressRoute 线路（如果有多个）的每个对等互连设置 BGP 对等互连。 
- 如果使用 /29 子网，它将拆分成两个 /30 子网。 
    - 第一个 /30 子网用于主链路，第二个 /30 子网用于辅助链路。
    - 对于每个 /30 子网，必须在路由器上使用 /30 子网的第一个 IP 地址。 Microsoft 使用 /30 子网的第二个 IP 地址设置 BGP 会话。
    - [可用性 SLA](https://www.azure.cn/support/legal/sla/) 只有在设置两个 BGP 会话后才有效。

## <a name="public-ip-address-requirement"></a>公共 IP 地址要求
### <a name="private-peering"></a>专用对等互连
可选择使用用于专用对等互连的公共或专用 IPv4 地址。 我们提供端到端流量隔离，因此在进行专用对等互连时不可能与其他客户的地址重叠。 这些地址不会播发到 Internet。 

### <a name="public-peering"></a>公共对等互连
Azure 公共对等互连路径使用户能够通过其公共 IP 地址连接到 Azure 中托管的所有服务。 这些服务包括 [ExpessRoute 常见问题](expressroute-faqs.md) 中列出的服务，以及由 ISV 托管在 Microsoft Azure 上的所有服务。 始终从用户网络向 Microsoft 网络发起与公共对等互连中 Microsoft Azure 服务的连接。 必须为定向到 Microsoft 网络的流量使用公共 IP 地址。

### <a name="microsoft-peering"></a>Microsoft 对等互连
Microsoft 对等互连路径使用户能够连接到不支持通过 Azure 公共对等互连路径访问的 Microsoft 云服务。 服务列表包括 Office 365 服务，例如 Exchange Online、SharePoint Online、Skype for Business 和 CRM Online。 Microsoft 在 Microsoft 对等互连上支持双向连接。 定向到 Microsoft 云服务的流量必须使用有效的公共 IPv4 地址才能进入 Microsoft 网络。

确保 IP 地址和 AS 编号已在下列其中一个注册表中注册。

- [ARIN](https://www.arin.net/)
- [APNIC](https://www.apnic.net/)
- [AFRINIC](https://www.afrinic.net/)
- [LACNIC](http://www.lacnic.net/)
- [RIPENCC](https://www.ripe.net/)
- [RADB](http://www.radb.net/)
- [ALTDB](http://altdb.net/)

> [!IMPORTANT]
> 通过 ExpressRoute 播发到 Microsoft 的公共 IP 地址不得播发到 Internet。 这会中断其他 Microsoft 服务的连接。 但是，用户网络中与 Microsoft 内的 O365 终结点通信的服务器使用的公共 IP 地址可通过 ExpressRoute 播发。 
> 
> 

## <a name="dynamic-route-exchange"></a>动态路由交换

路由交换将通过 eBGP 协议进行。 在 MSEE 与路由器之间建立 EBGP 会话。 不要求对 BGP 会话进行身份验证。 如果需要，可以配置 MD5 哈希。 有关配置 BGP 会话的信息，请参阅[配置路由](./expressroute-howto-routing-classic.md)及[线路预配工作流和线路状态](./expressroute-workflows.md)。

## <a name="autonomous-system-numbers"></a>自治系统编号

Microsoft 将为 Azure 公共对等互连和 Azure 专用对等互连使用 AS 12076，同时保留从 65515 到 65520 的 ASN 供内部使用。 支持 16 和 32 位 AS 编号。

数据传输对称没有相关要求。 转发与返回路径可以遍历不同的路由器对。 相同的路由必须从所属的多个线路对的任何一端播发。 路由指标不需要完全相同。

## <a name="route-aggregation-and-prefix-limits"></a>路由聚合与前缀限制
支持通过 Azure 专用对等互连播发最多 4000 个前缀。 如果启用 ExpressRoute 高级外接程序，则最多可增加 10,000 个前缀。 接受为每个 BGP 会话最多使用 200 个前缀建立 Azure 公共和 Microsoft 对等互连。 

如果前缀数目超过此限制，将丢弃 BGP 会话。 只接受专用对等互连链路上的默认路由。 提供商必须从 Azure 公共和 Microsoft 对等互连路径中筛选默认路由和专用 IP 地址 (RFC 1918)。 

## <a name="transit-routing-and-cross-region-routing"></a>传输路由和跨区域路由
ExpressRoute 不能配置为传输路由器。 必须依赖连接服务提供商的传输路由服务。

## <a name="advertising-default-routes"></a>播发默认路由
只有 Azure 专用对等互连会话允许默认路由。 在这种情况下，可将所有流量从关联的虚拟网络路由到用户网络。 在专用对等互连中播发默认路由会导致阻止来自 Azure 的 Internet 路径。 必须依赖企业网络边缘，为 Azure 中托管的服务往返路由 Internet 的流量。 

 若要启用与其他 Azure 服务和基础结构服务的连接，必须确保已准备下列其中一项：

 - 已启用 Azure 公共对等互连，以将流量路由到公共终结点
 - 使用用户定义的路由可为需要 Internet 连接的每个子网建立 Internet 连接。

**注意：** 播发默认路由会中断 Windows 和其他 VM 许可证激活。 请按照 [此处](http://blogs.msdn.com/b/mast/archive/2015/05/20/use-azure-custom-routes-to-enable-kms-activation-with-forced-tunneling.aspx) 的说明来解决此问题。

## <a name="support-for-bgp-communities"></a>BGP 社区支持
本部分概述如何配合 ExpressRoute 使用 BGP 社区。 Microsoft 将播发公共和 Microsoft 对等互连路径中的路由，并将路由标记为适当的社区值。 下面将介绍这种方案的理由和有关社区值的详细信息。 但是，Microsoft 不遵循向 Microsoft 播发的路由的任何标记社区值。

如果要在某个地缘政治区域内的任何一个对等互连位置通过 ExpressRoute 连接到 Microsoft，必须能够访问该地缘政治边界内所有区域中的所有 Microsoft 云服务。 

例如，如果在阿姆斯特丹通过 ExpressRoute 连接到 Microsoft，则能够访问在北欧和西欧托管的所有 Microsoft 云服务。 

有关地缘政治地区、关联的 Azure 区域和对应的 ExpressRoute 对等互连位置的详细列表，请参阅 [ExpressRoute 合作伙伴和对等位置](./expressroute-locations.md) 。

可以针对每个地缘政治区域购买多个 ExpressRoute 线路。 如果拥有多个连接，则可以从异地冗余中获得明显的高可用性优势。 如果拥有多条 ExpressRoute 线路，将在公共对等互连和 Microsoft 对等互连路径上收到从 Microsoft 播发的同一组前缀。 这意味着可以使用多个路径从用户网络接入 Microsoft。 这可能会导致在网络中做出欠佳的路由决策。 因此，你可能会在不同的服务上遇到欠佳的连接体验。 可以依赖社区值做出适当的路由决策，[向用户提供最佳路由](./expressroute-optimize-routing.md)。

| **Microsoft Azure 区域** | **BGP 社区值** |
| --- | --- |
| **北美** | |
| 美国东部 |12076:51004 |
| 美国东部 2 |12076:51005 |
| 美国西部 |12076:51006 |
| 美国西部 2 |12076:51026 |
| 美国中西部 |12076:51027 |
| 美国中北部 |12076:51007 |
| 美国中南部 |12076:51008 |
| 美国中部 |12076:51009 |
| 加拿大中部 |12076:51020 |
| 加拿大东部 |12076:51021 |
| **南美洲** | |
| 巴西南部 |12076:51014 |
| **欧洲** | |
| 北欧 |12076:51003 |
| 西欧 |12076:51002 |
| 英国南部 | 12076:51024 |
| 英国西部 | 12076:51025 |
| **亚太区** | |
| 东亚 |12076:51010 |
| 东南亚 |12076:51011 |
| **日本** | |
| 日本东部 |12076:51012 |
| 日本西部 |12076:51013 |
| **澳大利亚** | |
| 澳大利亚东部 |12076:51015 |
| 澳大利亚东南部 |12076:51016 |
| **印度** | |
| 印度南部 |12076:51019 |
| 印度西部 |12076:51018 |
| 印度中部 |12076:51017 |
| **韩国** | |
| 韩国南部 |12076:51028 |
| 韩国中部 |12076:51029 |

所有 Microsoft 播发的路由都标有适当的社区值。 

>[!IMPORTANT]
> 全局前缀将使用相应的社区值进行标记，并且仅当已启用 ExpressRoute 高级外接程序时才播发。

除了上述各项，Microsoft 也将根据其所属的服务标记前缀。 这仅适用于 Microsoft 对等互连。 下表提供了服务与 BGP 社区值之间的映射。

| **服务** | **BGP 社区值** |
| --- | --- |
| Exchange Online |12076:5010 |
| SharePoint Online |12076:5020 |
| Skype for Business Online |12076:5030 |
| CRM Online |12076:5040 |
| 其他 Office 365 Online 服务 |12076:5100 |

>[!NOTE]
> Microsoft 不遵循你在播发到 Microsoft 的路由上设置的任何 BGP 社区值。

### <a name="bgp-community-support-in-national-clouds-preview"></a>区域云中的 BGP 社区支持（预览版）

| **国家云 Azure 区域**| **BGP 社区值** |
| --- | --- |
| **美国政府** |  |
| 美国亚利桑那州政府 | 12076:51106 |
| 美国政府爱荷华州 | 12076:51109 |
| 美国政府弗吉尼亚州 | 12076:51105 |
| 美国德克萨斯州政府 | 12076:51108 |
| 美国 DoD 中部 | 12076:51209 |
| 美国 DoD 东部 | 12076:51205 |

| **国家云中的服务** | **BGP 社区值** |
| --- | --- |
| **美国政府** |  |
| Exchange Online |12076:5110 |
| SharePoint Online |12076:5120 |
| Skype for Business Online |12076:5130 |
| CRM Online |12076:5140 |
| 其他 Office 365 Online 服务 |12076:5200 |

## <a name="next-steps"></a>后续步骤

- 配置 ExpressRoute 连接。

    - [创建经典部署模型的 ExpressRoute 线路](./expressroute-howto-circuit-classic.md)或[使用 Azure Resource Manager 创建和修改 ExpressRoute 线路](./expressroute-howto-circuit-arm.md)
    - [为经典部署模型配置路由](./expressroute-howto-routing-classic.md)或[为 Resource Manager 部署模型配置路由](./expressroute-howto-routing-arm.md)
    - [将经典 VNet 链接到 ExpressRoute 线路](./expressroute-howto-linkvnet-classic.md)或[将 Resource Manager VNet 链接到 ExpressRoute 线路](./expressroute-howto-linkvnet-arm.md)
