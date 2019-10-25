---
title: include 文件
description: include 文件
services: virtual-wan
author: rockboyfor
ms.service: virtual-wan
ms.topic: include
origin.date: 08/06/2019
ms.date: 09/23/2019
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 03216adef36f2dd622b9bc8ec7aa7f42aaddde76
ms.sourcegitcommit: 1b4cb23c9bce2e9073e34eb9fb8b6765b9357d83
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2019
ms.locfileid: "72170804"
---
### <a name="what-is-the-difference-between-an-azure-virtual-network-gateway-vpn-gateway-and-an-azure-virtual-wan-vpngateway"></a>Azure 虚拟网络网关（VPN 网关）与 Azure 虚拟 WAN vpngateway 之间有什么差别？

虚拟 WAN 提供大规模站点到站点连接，在设计上考虑到了吞吐量、可伸缩性和易用性。  CPE 分支设备自动预配并连接到 Azure 虚拟 WAN。 这些设备由一个不断扩张的 SD-WAN 和 VPN 合作伙伴生态系统提供。 请参阅[首选合作伙伴列表](https://go.microsoft.com/fwlink/p/?linkid=2019615)。

<!--Not Available on ExpressRoute  for Virtual WAN connectivity is currently under Preview.-->

### <a name="what-is-a-branch-connection-to-azure-virtual-wan"></a>到 Azure 虚拟 WAN 的分支连接是什么？

从分支设备到 Azure 虚拟 WAN 的连接，包含两个主动/主动 IPsec 隧道。

### <a name="which-device-providers-virtual-wan-partners-are-supported-at-launch-time"></a>推出时支持哪些设备提供商（虚拟 WAN 合作伙伴）？

目前，许多合作伙伴都支持全自动虚拟 WAN 体验。 有关详细信息，请参阅[虚拟 WAN 合作伙伴](https://go.microsoft.com/fwlink/p/?linkid=2019615)。 

### <a name="what-are-the-virtual-wan-partner-automation-steps"></a>虚拟 WAN 合作伙伴自动化步骤有哪些？

有关合作伙伴自动化步骤，请参阅[虚拟 WAN 合作伙伴自动化](../articles/virtual-wan/virtual-wan-configure-automation-providers.md)。

### <a name="am-i-required-to-use-a-preferred-partner-device"></a>是否需要使用首选的合作伙伴设备？

否。 可以使用任何支持 VPN 且符合 Azure 对 IKEv2/IKEv1 IPsec 的支持要求的设备。

### <a name="how-do-virtual-wan-partners-automate-connectivity-with-azure-virtual-wan"></a>虚拟 WAN 合作伙伴如何自动与 Azure 虚拟 WAN 建立连接？

软件定义的连接解决方案通常使用控制器或设备预配中心来管理其分支设备。 控制器可以使用 Azure API 自动与 Azure 虚拟 WAN 建立连接。 有关详细信息，请参阅虚拟 WAN 合作伙伴自动化。

### <a name="does-virtual-wan-change-any-existing-connectivity-features"></a>虚拟 WAN 是否会更改任何现有的连接功能？   

不会对现有的 Azure 连接功能进行任何更改。

### <a name="are-there-new-resource-manager-resources-available-for-virtual-wan"></a>虚拟 WAN 是否有新的可用资源管理器资源？

是的，虚拟 WAN 引入了新的资源管理器资源。 有关详细信息，请参阅[概述](https://go.microsoft.com/fwlink/p/?LinkId=2004389)。

### <a name="how-many-vpn-devices-can-connect-to-a-single-hub"></a>允许多少 VPN 设备连接到单个中心？

每个虚拟中心最多支持 1,000 个连接。 每个连接包括采用主动-主动配置的两个隧道。 隧道在 Azure 虚拟中心 vpngateway 中终止。

### <a name="can-the-on-premises-vpn-device-connect-to-multiple-hubs"></a>本地 VPN 设备是否可以连接到多个中心？

是的。 开始时的流量流将从本地设备发送到最近的 Azure 边缘，然后才发送到虚拟中心。

<!--MOONCAKE: CORRECT ON Azure edge-->

### <a name="is-global-vnet-peering-supported-with-azure-virtual-wan"></a>Azure 虚拟 WAN 是否支持全局 VNet 对等互连？ 

 否。

### <a name="can-spoke-vnets-connected-to-a-virtual-hub-communicate-with-each-other"></a>连接到虚拟中心的分支 VNet 是否能够相互通信？

是的。 分支 VNet 可以通过虚拟网络对等互连直接通信。 但是，我们不支持通过中心进行可传递通信的 VNet。 有关详细信息，请参阅[虚拟网络对等互连](../articles/virtual-network/virtual-network-peering-overview.md)。

### <a name="can-i-deploy-and-use-my-favorite-network-virtual-appliance-in-an-nva-vnet-with-azure-virtual-wan"></a>是否可以在 Azure 虚拟 WAN 中部署和使用我偏爱的网络虚拟设备（在 NVA VNet 中）？

是的，可以将你偏爱的网络虚拟设备 (NVA) VNet 连接到 Azure Virtual WAN。 首先，使用中心虚拟网络连接将网络虚拟设备 VNet 连接到中心。 然后，使用指向虚拟设备的下一跃点创建一个虚拟中心路由。 可以将多个路由应用于虚拟中心路由表。 此外，连接到 NVA VNet 的任何辐射还必须连接到虚拟中心，以确保辐射 VNet 路由传播到本地系统。

### <a name="can-an-nva-vnet-have-a-virtual-network-gateway"></a>NVA VNet 是否可以包含虚拟网络网关？

否。 如果 NVA VNet 已连接到虚拟中心，则不能包含虚拟网络网关。 

### <a name="is-there-support-for-bgp"></a>是否支持 BGP？

是的，支持 BGP。 创建 VPN 站点时，可以在其中提供 BGP 参数。 这表示在 Azure 中为该站点创建的任何连接都将启用 BGP。 此外，如果 VNet 具有 NVA 且此 NVA VNet 已附加到虚拟 WAN 中心，为了确保适当地公布来自 NVA VNet 的路由，附加到 NVA VNet 的辐射必须禁用 BGP。 此外，请将这些辐射 VNet 连接到虚拟中心 VNet，以确保辐射 VNet 路由传播到本地系统。

### <a name="can-i-direct-traffic-using-udr-in-the-virtual-hub"></a>是否可以在虚拟中心使用 UDR 定向流量？

是的，可以使用虚拟中心路由表将流量定向到 VNet。

### <a name="is-there-any-licensing-or-pricing-information-for-virtual-wan"></a>虚拟 WAN 是否有任何许可或定价信息？

是的。 请参阅[定价](https://www.azure.cn/pricing/details/virtual-wan/)页面。

<!--Pending on https://www.azure.cn/pricing/details/virtual-wan/-->

<!--Not Available ### How do I calculate price of a hub?-->
<!--Not Available on [Pricing](https://www.azure.cn/pricing/details/virtual-wan/)-->

### <a name="how-do-new-partners-that-are-not-listed-in-your-launch-partner-list-get-onboarded"></a>没有在启动合作伙伴列表中列出的新合作伙伴如何加入？

联络 [Azure 支持部门](https://support.azure.cn/support/contact/)。 理想的合作伙伴具有可以预配 IKEv1 或 IKEv2 IPsec 连接的设备。

<!--Not Available on Send an email to azurevirtualwan@microsoft.com-->

### <a name="what-if-a-device-i-am-using-is-not-in-the-virtual-wan-partner-list-can-i-still-use-it-to-connect-to-azure-virtual-wan-vpn"></a>如果使用的设备不在虚拟 WAN 合作伙伴列表中，该怎么办？ 还可以用它来连接到 Azure 虚拟 WAN VPN 吗？

是的，只要设备支持 IPsec IKEv1 或 IKEv2 即可。 虚拟 WAN 合作伙伴自动执行设备到 Azure VPN 端点的连接。 这表示自动执行“分支信息上传”、“IPsec 和配置”以及“连接”等步骤。由于设备不是来自虚拟 WAN 合作伙伴生态系统，因此需要大量手动执行 Azure 配置和更新设备才能建立 IPsec 连接。 

### <a name="is-it-possible-to-construct-azure-virtual-wan-with-a-resource-manager-template"></a>是否可以使用资源管理器模板构造 Azure 虚拟 WAN？

可以使用 [Azure 快速入门模板](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Network)创建具有单个中心和单个 VPN 站点的单个虚拟 WAN 的简单配置。 虚拟 WAN 从根本上来说是一种 REST 或门户驱动的服务。

### <a name="is-branch-to-branch-connectivity-allowed-in-virtual-wan"></a>虚拟 WAN 中是否允许分支到分支连接？

是的，VPN 的分支到分支连接在虚拟 WAN 中可用。 而 VPN 站点到站点是 GA。

<!--Not Available on and VPN to ExpressRoute-->
<!--Not Available on , ExpressRoute is currently in Preview-->

### <a name="does-branch-to-branch-traffic-traverse-through-the-azure-virtual-wan"></a>分支到分支流量是否可以通过 Azure 虚拟 WAN？

是的。

### <a name="how-is-virtual-wan-different-from-the-existing-azure-virtual-network-gateway"></a>虚拟 WAN 与现有 Azure 虚拟网络网关有何不同？

虚拟网络网关 VPN 限制为 30 个隧道。 对于连接，应当为大型 VPN 使用虚拟 WAN。 在所有区域的中心内，可以以 20 Gbps 的速率最多连接到 1,000 个分支。 连接是从本地 VPN 设备到虚拟中心的主动-主动隧道。 每个区域中可以有一个中心，这意味着你可以跨中心连接到 1,000 多个分支。

<!--Not Available on except the West Central region, For the West Central region, 20 Gbps is available.-->


### <a name="how-is-virtual-wan-supporting-sd-wan-devices"></a>虚拟 WAN 如何支持 SD-WAN 设备？

虚拟 WAN 合作伙伴自动执行 Azure VPN 端点的 IPsec 连接。 如果虚拟 WAN 合作伙伴是 SD-WAN 提供商，则表示 SD-WAN 控制器管理到 Azure VPN 端点的自动化和 IPsec 连接。 如果 SD-WAN 设备需要自己的端点而不是 Azure VPN 来实现任何专有 SD-WAN 功能，则可以在 Azure VNet 中部署 SD-WAN 端点并与 Azure 虚拟 WAN 共存。

### <a name="does-this-virtual-wan-require-expressroute-from-each-site"></a>此虚拟 WAN 是否要求每个站点中都有 ExpressRoute？

否，虚拟 WAN 不要求每个站点中都有 ExpressRoute。 它通过 Internet 链接使用从设备到 Azure 虚拟 WAN 中心的标准 IPsec 站点到站点连接。 可以使用 ExpressRoute 线路将站点连接到提供商网络。

<!--Not Availble on For Sites that are connected using ExpressRoute in Virtual Hub (Under Preview), sites can have branch to branch traffic flow between VPN and ExpressRoute.-->

### <a name="is-there-a-network-throughput-limit-when-using-azure-virtual-wan"></a>使用 Azure 虚拟 WAN 时是否存在网络吞吐量限制？

分支数限制为每个中心/区域 1000 个连接，中心内总带宽为 20 Gbps。

### <a name="i-dont-see-the-20-gbps-setting-for-the-virtual-hub-in-the-portal-how-do-i-configure-that"></a>我在门户中看不到虚拟中心的 20 Gbps 设置。 我该如何配置它？

目前，可以使用 [Update-AzVpnGateway](https://docs.microsoft.com/powershell/module/az.network/update-azvpngateway) cmdlet 为 20 Gbps 配置网关缩放单元。 此设置在门户中将要提供的路线图上。

### <a name="how-many-vpn-connections-does-a-virtual-wan-hub-support"></a>一个虚拟 WAN 中心支持多少个 VPN 连接？

一个 Azure 虚拟 WAN 中心可以同时支持最多 1,000 个 S2S 连接和 10,000 个 P2S 连接。

### <a name="what-is-the-total-vpn-throughput-of-a-vpn-tunnel-and-a-connection"></a>一个 VPN 隧道和一个连接的总 VPN 吞吐量是多少？

一个中心的总 VPN 吞吐量最多为 20 Gbps，具体取决于所选缩放单元。 吞吐量由所有现有连接共享。

### <a name="does-virtual-wan-allow-the-on-premises-device-to-utilize-multiple-isps-in-parallel-or-is-it-always-a-single-vpn-tunnel"></a>虚拟 WAN 是否允许本地设备并行利用多个 ISP？亦或它始终为单个 VPN 隧道？

使用分支提供的链接与建立 WAN VPN 连接始终是主动-主动隧道（在同一中心/区域具有复原能力）。 此链接可以是本地分支的 ISP 链接。 虚拟 WAN 不提供任何用于并行设置多个 ISP 的特殊逻辑；在分支中跨 ISP 管理故障转移完全是以分支为中心的网络操作。 可以使用自己喜欢的 SD-WAN 解决方案在分支中进行路径选择。

### <a name="how-is-traffic-routed-on-the-azure-backbone"></a>流量在 Azure 主干网上是如何路由的？

流量遵循以下模式：分支设备->ISP->Microsoft Edge->Microsoft DC->Microsoft Edge（中心 VNet）->ISP->分支设备

### <a name="in-this-model-what-do-you-need-at-each-site-just-an-internet-connection"></a>在此模型中，需要在每个站点执行什么操作？ 只需要创建 Internet 连接？

是的。 支持 IPsec 的 Internet 连接和物理设备，最好是来自我们的集成[合作伙伴](https://go.microsoft.com/fwlink/p/?linkid=2019615)。 还可以从你偏爱的设备手动管理 Azure 的配置和连接。