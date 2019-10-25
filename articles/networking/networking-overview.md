---
title: Azure 网络 | Azure
description: 了解 Azure 中的网络服务及其功能。
services: networking
documentationcenter: na
author: KumudD
manager: twooley
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 07/17/2019
ms.date: 10/17/2019
ms.author: v-tawe
ms.openlocfilehash: c4953fa718a66723c08f1638cee6d1a989be397e
ms.sourcegitcommit: c21b37e8a5e7f833b374d8260b11e2fb2f451782
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/18/2019
ms.locfileid: "72583822"
---
# <a name="azure-networking"></a>Azure 网络

Azure 中的网络服务提供可以搭配使用或单独使用的各种网络功能。 请单击以下任一重要功能了解更多相关信息：
- [**连接服务**](#connect)：使用 Azure 中的以下任一网络服务或其组合连接 Azure 资源和本地资源 - 虚拟网络 (VNet)、虚拟 WAN、ExpressRoute、VPN 网关、Azure DNS。
- [**应用程序保护服务**](#protect)：使用 Azure 中的以下任一网络服务或其组合来保护应用程序 - 防火墙、网络安全组或虚拟网络终结点。
- [**应用程序分发服务**](#deliver)：使用 Azure 中的以下任一网络服务或其组合在 Azure 网络中分发应用程序 - 内容分发网络 (CDN)、流量管理器、应用程序网关或负载均衡器。
- [**网络监视**](#monitor) – 使用 Azure 中的以下任一网络服务或其组合来监视网络资源 - 网络观察程序、ExpressRoute Monitor、Azure Monitor。

## <a name="connect"></a>连接服务
 
本部分介绍用于在 Azure 资源之间提供连接、建立从本地网络到 Azure 资源的连接，以及在 Azure 中建立分支到分支连接的服务 - 虚拟网络、ExpressRoute、VPN 网关、DNS。

|服务|为何使用此类服务？|方案|
|---|---|---|
|[虚拟网络](#vnet)|可让 Azure 资源以安全方式彼此通信、与 Internet 通信，以及与本地网络通信。| <p>[筛选网络流量](../virtual-network/tutorial-filter-network-traffic.md)</p> <p>[路由网络流量](../virtual-network/tutorial-create-route-table-portal.md)</p> <p>[限制对资源的网络访问](../virtual-network/tutorial-restrict-network-access-to-resources.md)</p> <p>[连接虚拟网络](../virtual-network/tutorial-connect-virtual-networks-portal.md)</p>|
|[ExpressRoute](#expressroute)|通过连接服务提供商所提供的专用连接，将本地网络扩展到 Microsoft 云。|<p>[创建和修改 ExpressRoute 线路](../expressroute/expressroute-howto-circuit-portal-resource-manager.md)</p> <p>[创建和修改 ExpressRoute 线路的对等互连](../expressroute/expressroute-howto-routing-portal-resource-manager.md)</p> <p>[将 VNet 链接到 ExpressRoute 线路](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md)</p> <p>[配置和管理 ExpressRoute 线路的路由筛选器](../expressroute/how-to-routefilter-portal.md)</p>|
|[VPN 网关](#vpngateway)|通过公共 Internet 在 Azure 虚拟网络与本地位置之间发送加密流量。|<p>[站点到站点连接](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)</p> <p>[VNet 到 VNet 连接](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)</p> <p>[点到站点连接](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md)</p>|
|[虚拟 WAN](#virtualwan)|优化并自动化到 Azure 和通过 Azure 的分支连接。 Azure 区域充当可以选择将分支连接到的中心。|<p>[站点到站点连接](../virtual-wan/virtual-wan-site-to-site-portal.md)</p>|
|[Azure DNS](#dns)|托管使用 Microsoft Azure 基础结构提供名称解析的 DNS 域。|<p>[在 Azure DNS 中托管域](../dns/dns-delegate-domain-azure-dns.md)</p><p>[为 Web 应用创建 DNS 记录](../dns/dns-web-sites-custom-domain.md)</p>|
||||


### <a name="vnet"></a>虚拟网络

Azure 虚拟网络 (VNet) 是 Azure 中专用网络的基本构建块。 使用 VNet 可以：
- **在 Azure 资源之间通信**：可以将 VM 和多个其他类型的 Azure 资源部署到虚拟网络，如 Azure 应用服务环境、Azure Kubernetes 服务 (AKS) 和 Azure 虚拟机规模集。 若要查看可部署到虚拟网络的 Azure 资源的完整列表，请参阅[虚拟网络服务集成](../virtual-network/virtual-network-for-azure-services.md)。
- **相互通信**：可以互相连接虚拟网络，使虚拟网络中的资源能够通过虚拟网络对等互连相互进行通信。 连接的虚拟网络可以在相同或不同的 Azure 区域中。 有关详细信息，请参阅[虚拟网络对等互连](../virtual-network/virtual-network-peering-overview.md)。
- **与 Internet 通信**：默认情况下，VNet 中的所有资源都可以与 Internet 进行出站通信。 可以通过分配公共 IP 地址或公共负载均衡器来与资源进行入站通信。 还可以使用[公共 IP 地址](../virtual-network/virtual-network-public-ip-address.md)或公共[负载均衡器](../load-balancer/load-balancer-overview.md)来管理出站连接。
- **与本地网络通信**：可以使用 [VPN 网关](../vpn-gateway/vpn-gateway-about-vpngateways.md)或 [ExpressRoute](../expressroute/expressroute-introduction.md) 将本地计算机和网络连接到虚拟网络。

有关详细信息，请参阅[什么是 Azure 虚拟网络？](../virtual-network/virtual-networks-overview.md)

### <a name="expressroute"></a>ExpressRoute
使用 ExpressRoute 可通过连接服务提供商所提供的专用连接，将本地网络扩展到 Microsoft 云。 此连接是专用连接。 流量不经过 Internet。 使用 ExpressRoute 可与 Microsoft Azure、Office 365 和 Dynamics 365 等 Microsoft 云服务建立连接。  有关详细信息，请参阅[什么是 ExpressRoute？](../expressroute/expressroute-introduction.md)

![Azure ExpressRoute](./media/networking-overview/expressroute-connection-overview.png)

### <a name="vpngateway"></a>VPN 网关
VPN 网关可帮助你创建从本地位置到虚拟网络的加密跨界连接，或者在 VNet 之间创建加密连接。 VPN 网关连接可以使用不同的配置，例如站点到站点连接、点到站点连接，或 VNet 到 VNet 连接。
下图演示了与同一虚拟网络建立的多个站点到站点 VPN 连接。

![站点到站点 Azure VPN 网关连接](./media/networking-overview/vpngateway-multisite-connection-diagram.png)

有关不同类型的 VPN 连接的详细信息，请参阅 [VPN 网关](../vpn-gateway/vpn-gateway-about-vpngateways.md)。

### <a name="virtualwan"></a>虚拟 WAN
Azure Virtual WAN 是一种网络服务，提供到 Azure 并穿过该服务的经优化的自动分支连接。 Azure 区域充当可以选择将分支连接到的中心。 利用 Azure 主干网还可以连接分支并享用分支到 VNet 的连接。 Azure 虚拟 WAN 将许多 Azure 云连接服务（例如，站点到站点 VPN、ExpressRoute、点到站点用户 VPN）汇集到一个操作界面中。 通过使用虚拟网络连接建立与 Azure VNet 的连接。 有关详细信息，请参阅[什么是 Azure 虚拟 WAN？](../virtual-wan/virtual-wan-about.md)。

![虚拟 WAN 示意图](./media/networking-overview/virtualwan1.png)

### <a name="dns"></a>Azure DNS
Azure DNS 是 DNS 域的托管服务，它使用 Azure 基础结构提供名称解析。 通过在 Azure 中托管域，可以使用与其他 Azure 服务相同的凭据、API、工具和计费来管理 DNS 记录。 有关详细信息，请参阅[什么是 Azure DNS？](../dns/dns-overview.md)

## <a name="protect"></a>应用程序保护服务

本部分介绍 Azure 中可帮助你保护网络资源的网络服务 - Azure 防火墙、网络安全组和服务终结点。

|服务|为何使用此类服务？|方案|
|---|---|---|
|[Azure 防火墙](#firewall)|Azure 防火墙是托管的基于云的网络安全服务，可保护 Azure 虚拟网络资源。 它是一个服务形式的完全有状态防火墙，具有内置的高可用性和不受限制的云可伸缩性。|<p>[在 Vnet 中部署 Azure 防火墙](../firewall/tutorial-firewall-deploy-portal.md)</p> <p>[- 在混合网络中部署 Azure 防火墙](../firewall/tutorial-hybrid-ps.md)</p> <p>[使用 Azure 防火墙 DNAT 筛选入站流量](../firewall/tutorial-firewall-dnat.md)</p>|
|[网络安全组](#nsg)|在 VM/子网中对所有网络流量进行完全粒度的分布式终端节点控制|[使用网络安全组筛选网络流量](../virtual-network/tutorial-filter-network-traffic.md)|
|[虚拟网络服务终结点](#serviceendpoints)|使你可以将对某些 Azure 服务资源的网络访问限制到虚拟网络子网|[限制 PaaS 资源的网络访问](../virtual-network/tutorial-restrict-network-access-to-resources-powershell.md)|
|||
### <a name="nsg"></a>网络安全组
可以使用网络安全组来筛选 Azure 虚拟网络中出入 Azure 资源的网络流量。 有关详细信息，请参阅[安全性概述](../virtual-network/security-overview.md)。

### <a name="serviceendpoints"></a>服务终结点
虚拟网络 (VNet) 服务终结点可通过直接连接将 VNet 的虚拟网络专用地址空间和标识扩展到 Azure 服务。 使用终结点可以保护关键的 Azure 服务资源，只允许在客户自己的虚拟网络中对其进行访问。 从 VNet 发往 Azure 服务的流量始终保留在 Azure 主干网络中。 有关详细信息，请参阅[虚拟网络服务终结点](../virtual-network/virtual-network-service-endpoints-overview.md)。

![虚拟网络服务终结点](./media/networking-overview/vnet-service-endpoints-overview.png)

## <a name="deliver"></a>应用程序分发服务

本部分介绍 Azure 中可帮助分发应用程序的网络服务 - 流量管理器、应用程序网关和负载均衡器。

|服务|为何使用此类服务？|方案|
|---|---|---|
|[流量管理器](#trafficmanager)|基于 DNS 将流量分发到全球 Azure 区域中的服务，同时提供高可用性和响应度。|<p> [路由流量以降低延迟](../traffic-manager/tutorial-traffic-manager-improve-website-response.md)</p><p>[将流量路由到优先终结点](../traffic-manager/traffic-manager-configure-priority-routing-method.md)</p><p> [使用加权的终结点控制流量](../traffic-manager/tutorial-traffic-manager-weighted-endpoint-routing.md)</p><p>[基于终结点的地理位置路由流量](../traffic-manager/traffic-manager-configure-geographic-routing-method.md)</p> <p> [根据用户的子网路由流量](../traffic-manager/tutorial-traffic-manager-subnet-routing.md)</p>|
|[负载均衡器](#loadbalancer)|通过将流量路由到不同的可用性区域和你的 VNet 来提供区域负载均衡。 通过在资源中和资源之间路由流量来提供内部负载均衡，以构建区域性应用程序。|<p> [对传入 VM 的 Internet 流量进行负载均衡](../load-balancer/tutorial-load-balancer-standard-manage-portal.md)</p> <p>[对虚拟网络中 VM 之间的流量进行负载均衡](../load-balancer/tutorial-load-balancer-basic-internal-portal.md)<p>[通过端口转发将流量发送到特定 VM 上的特定端口](../load-balancer/tutorial-load-balancer-port-forwarding-portal.md)</p><p> [配置负载均衡和出站规则](../load-balancer/configure-load-balancer-outbound-cli.md)</p>|
|[应用程序网关](#applicationgateway)|Azure 应用程序网关是一种 Web 流量负载均衡器，可用于管理 Web 应用程序的流量。|<p>[使用 Azure 应用程序网关定向 Web 流量](../application-gateway/quick-create-portal.md)</p><p>[配置带有 SSL 终端的应用程序网关](../application-gateway/create-ssl-portal.md)</p><p>[创建支持基于 URL 路径进行重定向的应用程序网关](../application-gateway/create-url-route-portal.md) </p>|
|



### <a name="trafficmanager"></a>流量管理器

Azure 流量管理器是一种基于 DNS 的流量负载均衡器，可以在 Azure 区域内以最佳方式向服务分发流量，同时提供高可用性和响应度。 流量管理器提供一系列流量路由方法来分发流量，例如优先级、加权、性能、地理位置、多值或子网路由方法。 有关流量路由方法的详细信息，请参阅[流量管理器路由方法](../traffic-manager/traffic-manager-routing-methods.md)。

下图演示了流量管理器的基于终结点优先级的路由方法：

![Azure 流量管理器的“优先级”流量路由方法](./media/networking-overview/priority.png)

有关流量管理器的详细信息，请参阅[什么是 Azure 流量管理器？](../traffic-manager/traffic-manager-overview.md)

### <a name="loadbalancer"></a>负载均衡器
Azure 负载均衡器为所有 UDP 和 TCP 协议提供高性能、低延迟的第 4 层负载均衡。 它管理入站和出站连接。 可以配置公共和内部负载均衡终结点。 可以定义规则，以便将入站连接映射到后端池目标，并在其中包含 TCP 和 HTTP 运行状况探测选项来管理服务的可用性。 若要了解有关负载均衡器的详细信息，请参阅[负载均衡器概述](../load-balancer/load-balancer-overview.md)一文。

下图显示了利用外部和内部负载均衡器的面向 Internet 的多层应用程序：

![Azure 负载均衡器示例](./media/networking-overview/IC744147.png)


### <a name="applicationgateway"></a>应用程序网关
Azure 应用程序网关是一种 Web 流量负载均衡器，可用于管理 Web 应用程序的流量。 它是服务形式的应用程序传送控制器 (ADC)，借此为应用程序提供各种第 7 层负载均衡功能。 有关详细信息，请参阅[什么是 Azure 应用程序网关？](../application-gateway/overview.md)

下图演示了应用程序网关的基于 URL 路径的路由方法。

![应用程序网关示例](./media/networking-overview/figure1-720.png)

## <a name="monitor"></a>网络监视服务
本部分介绍 Azure 中可帮助监视网络资源的网络服务 - 网络观察程序、ExpressRoute Monitor、Azure Monitor 和虚拟网络 TAP。

|服务|为何使用此类服务？|方案|
|---|---|---|
|[网络观察程序](#networkwatcher)|帮助监视和排查连接问题，帮助诊断 VPN、NSG 和路由问题，捕获 VM 上的数据包，使用 Azure Functions 和逻辑应用自动触发诊断工具|<p>[诊断 VM 流量筛选器问题](../network-watcher/diagnose-vm-network-traffic-filtering-problem.md)</p><p>[诊断 VM 路由问题](../network-watcher/diagnose-vm-network-routing-problem.md)</p><p>[监视 VM 之间的通信](../network-watcher/connection-monitor.md)</p><p>[诊断网络之间的通信问题](../network-watcher/diagnose-communication-problem-between-networks.md)</p><p>[记录传入和传出 VM 的网络流量](../network-watcher/network-watcher-nsg-flow-logging-portal.md)</p>|
|[ExpressRoute 监视器](#expressroutemonitor)|提供网络性能、可用性和利用率的实时监视，帮助自动发现网络拓扑，提供更快的故障隔离，检测暂时性网络问题，帮助分析历史网络性能特征，支持多订阅|<p>[ExpressRoute 监视、指标和警报](../expressroute/expressroute-monitoring-metrics-alerts.md)</p>|
|

### <a name="networkwatcher"></a>网络观察程序
Azure 网络观察程序提供所需的工具用于监视、诊断 Azure 虚拟网络中的资源、查看其指标，以及为其启用或禁用日志。 有关详细信息，请参阅[什么是网络观察程序？](../network-watcher/network-watcher-monitoring-overview.md?toc=%2fazure%2fnetworking%2ftoc.json)
### <a name="expressroutemonitor"></a>ExpressRoute Monitor
若要了解如何查看 ExpressRoute 线路指标、诊断日志和警报，请参阅 [ExpressRoute 监视、指标和警报](../expressroute/expressroute-monitoring-metrics-alerts.md?toc=%2fazure%2fnetworking%2ftoc.json)。

## <a name="next-steps"></a>后续步骤

- 完成[创建首个虚拟网络](../virtual-network/quick-create-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)一文中的步骤，创建自己的首个 VNet，并将几个 VM 连接到此网络。
- 完成[配置点到站点连接](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)一文中的步骤，将计算机连接到 VNet。
- 完成[创建面向 Internet 的负载均衡器](../load-balancer/load-balancer-get-started-internet-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)一文中的步骤，对发往公共服务器的 Internet 流量进行负载均衡。
 
 
   
