---
title: Azure 负载均衡器概述 | Azure
description: Azure 负载均衡器功能、体系结构和实现概述。 了解负载均衡器工作原理，在云中对其进行利用。
services: load-balancer
documentationcenter: na
author: rockboyfor
manager: digimobile
editor: ''
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 03/21/2018
ms.date: 04/30/2018
ms.author: v-yeche
ms.openlocfilehash: ff5b4413875349ed468b5bef7351b4c3053195fc
ms.sourcegitcommit: beee57ca976e21faa450dd749473f457e299bbfd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/10/2018
---
# <a name="azure-load-balancer-overview"></a>Azure 负载均衡器概述

使用 Azure 负载均衡器可以缩放应用程序，并为服务提供高可用性。 负载均衡器支持入站和出站场景、提供低延迟和高吞吐量，以及为所有 TCP 和 UDP 应用程序纵向扩展到数以百万计的流。   

负载均衡器根据规则和运行状况探测，将抵达负载均衡器前端的新入站流量分配到后端池实例。  

此外，公共负载均衡器还可将虚拟网络中虚拟机的专用 IP 地址转换为公共 IP 地址，从而为这些虚拟机提供出站连接。

<!-- Not Available on Azure Load Balancer is available in two different SKUs: Basic and Standard.  There are differences in scale, features, and pricing.  Any scenario possible with Basic Load Balancer can also be created with Standard Load Balancer, although the approach might differ slightly.  As you learn about Load Balancer, it is important to familiarize yourself with the fundamentals and SKU-specific differences.-->

## <a name="why-use-load-balancer"></a>为何使用负载均衡器？ 

Azure 负载均衡器可用于：

* 对传入到虚拟机的 Internet 流量进行负载均衡。 此配置称为[公共负载均衡器](#publicloadbalancer)。
* 对虚拟网络中虚拟机之间的流量进行负载均衡。 还可以在混合场景中从本地网络访问负载均衡器前端。 这两种场景都使用称作[内部负载均衡器](#internalloadbalancer)的配置。
* 使用入站 NAT 规则通过端口转发将流量转发到特定虚拟机上的特定端口。
* 使用公共负载均衡器为虚拟网络中的虚拟机提供[出站连接](load-balancer-outbound-connections.md)。

>[!NOTE]
> Azure 为方案提供了一套完全托管的负载均衡解决方案。  若要寻求 TLS 终止（“SSL 卸载”）或每个 HTTP/HTTPS 请求的应用层处理，请查看[应用程序网关](../application-gateway/application-gateway-introduction.md)。  若要寻求全局 DNS 负载均衡，请查看[流量管理器](../traffic-manager/traffic-manager-overview.md)。  端到端场景可从结合所需的解决方案中受益。

## <a name="what-is-load-balancer"></a>什么是负载均衡器？

负载均衡器资源可以是公共负载均衡器或内部负载均衡器。 负载均衡器资源的功能表示为前端、规则、运行状况探测和后端池定义。  可通过从虚拟机指定后端池，将虚拟机放入后端池。

负载均衡器资源是一些对象，可在其中表述 Azure 应如何设定其多租户基础结构，以实现想要创建的场景。  负载均衡器资源与实际基础结构之间不存在直接的关系，创建负载均衡器不会创建实例，容量始终可用。 

## <a name="fundamental-load-balancer-features"></a>基本的负载均衡器功能

负载均衡器为 TCP 和 UDP 应用程序提供以下基本功能：

* **负载均衡**

    使用 Azure 负载均衡器可以创建负载均衡规则，以便将抵达前端的流量分配到后端池实例。  负载均衡器使用基于哈希的算法来分配入站流量，并相应地重写发往后端池实例的流量的标头。 当运行状况探测指示后端终结点正常时，可以使用一个服务器来接收新流量。

    默认情况下，负载均衡器使用 5 元组哈希（包括源 IP 地址、源端口、目标 IP 地址、目标端口和 IP 协议编号）将流量映射到可用服务器。  可以启用给定规则的 2 元组或 3 元组哈希，来与特定的源 IP 地址创建关联。  同一数据包流量的所有数据包将会抵达负载均衡前端后面的同一实例。  当客户端从同一源 IP 发起新流量时，源端口将会更改。 生成的 5 元组可能导致流量定向到不同的后端终结点。

    有关详细信息，请参阅[负载均衡器分配模式](load-balancer-distribution-mode.md)。 下图显示了基于哈希的分发：

    ![基于哈希的分发](./media/load-balancer-overview/load-balancer-distribution.png)

    *图 - 基于哈希的分配*

* **端口转发**

    使用 Azure 负载均衡器可以创建入站 NAT 规则，以便通过端口转发，将来自特定前端 IP 地址的特定端口的流量转发到虚拟网络中特定后端实例的特定端口。 也可以通过与负载均衡相同的基于哈希的分配来实现此目的。  此功能的常见应用场景是与虚拟网络中的单个虚拟机实例建立远程桌面协议 (RDP) 或安全外壳 (SSH) 会话。  可将多个内部终结点映射到相同前端 IP 地址上的不同端口。 使用这些端口可以通过 Internet 远程管理虚拟机，而无需额外配置 Jumpbox。

* **应用程序不可知性和透明性**

    负载均衡器不直接与 TCP、UDP 或应用层交互，可支持任何基于 TCP 或 UDP 的应用场景。  例如，尽管负载均衡器不会终止 TLS 本身，但你可以使用负载均衡器构建并横向扩展 TLS 应用程序，并在虚拟机本身上终止 TLS 连接。 负载均衡器不会终止流，协议握手始终在客户端与哈希选择的后端池实例之间直接发生。 例如，TCP 握手就始终在客户端与选定的后端虚拟机之间发生。  对前端请求做出的响应是从后端虚拟机生成的响应。  负载均衡器的出站网络性能仅受限于所选的虚拟机 SKU，如果永远达不到空闲超时，则流就能长时间保持活动状态。

* **自动重新配置**

    增加或减少实例时，Azure 负载均衡器会立即自行重新配置。 在后端池中添加或删除虚拟机会重新配置负载均衡器，而无需针对负载均衡器资源执行附加的操作。

* **运行状况探测**

    Azure 负载均衡器使用定义的运行状况探测来确定后端池中实例的运行状况。 当探测无法响应时，负载均衡器会停止向状况不良的实例发送新连接。 现有连接不受影响，会一直保留到应用程序终止了流、发生空闲超时或虚拟机关闭为止。

    支持三种类型的探测：

    - **HTTP 自定义探测：**可以使用此探测来创建自己的自定义逻辑，以确定后端池实例的运行状况。 负载均衡器将定期探测你的终结点（默认情况下，每隔 15 秒探测 1 次）。 如果实例在超时期限内（默认为 31 秒）内使用 HTTP 200 做出响应，则认为该实例正常。 非 HTTP 200 的任何状态会导致此探测失败。  若要实现自己的逻辑以便从负载均衡器轮换中删除实例，此探测也很有用。 例如，可以将实例配置为在实例的 CPU 使用率超出 90% 时返回非 200 状态。   此探测将替代默认来宾代理探测。

    - **TCP 自定义探测：** 此探测依赖于在定义的探测端口上成功建立 TCP 会话。  只要虚拟机上指定的侦听器存在，此探测就会成功。 如果连接被拒绝，此探测将会失败。 此探测将替代默认来宾代理探测。

    - **来宾代理探测（仅适用于平台即服务虚拟机）：**负载均衡器也可以利用虚拟机中的来宾代理。 仅当实例处于就绪状态时，来宾代理才会侦听并以“HTTP 200 正常”做出响应。 如果代理没有使用“HTTP 200 正常”进行响应，则负载均衡器会将实例标记为无响应，并停止向该实例发送流量。 负载均衡器继续尝试访问实例。 如果来宾代理使用 HTTP 200 进行了响应，则负载均衡器会再次向该实例发送流量。  来宾代理探测是最终手段，如果支持 HTTP 或 TCP 自定义探测配置，则不应使用来宾代理探测。 

* **出站连接（源 NAT）**

    从虚拟网络中的专用 IP 地址发往 Internet 上的公共 IP 地址的所有出站流量可以转换为负载均衡器的前端 IP 地址。 通过负载均衡规则将公共前端绑定到后端虚拟机后，Azure 会将出站连接设定为自动转换成公共前端的 IP 地址。 这也称为源 NAT (SNAT)。 SNAT 的重要优势在于：

    + 可以轻松地对服务进行升级和灾难恢复操作，因为前端可以动态映射到服务的其他实例。
    + 简化了访问控制列表 (ACL) 管理。 以前端 IP 表示的 ACL 不会随着服务的缩放或重新部署而更改。

    有关此功能的详细讨论，请参阅[出站连接](load-balancer-outbound-connections.md)一文。

<!-- Not Available on Standard Load Balancer has additional SKU-specific abilities beyond these fundamentals.  Review the remainder of this article for details.-->

<a name="skus"></a>
<!-- Not Available on ##  Load Balancer SKU comparison-->

## <a name="concepts"></a>概念

### <a name = "publicloadbalancer"></a>公共负载均衡器

公共负载均衡器将传入流量的公用 IP 地址和端口号映射到虚拟机的专用 IP 地址和端口号，对于来自虚拟机的响应流量，则进行反向的映射。 借助负载均衡规则，可在多个虚拟机或服务之间分配特定类型的流量。 例如，可将 Web 请求流量负载分配到多个 Web 服务器。

下图显示了公用和专用 TCP 端口 80 的 Web 流量的负载均衡终结点，由三个虚拟机共享。 三台虚拟机位于负载均衡集中。

![公共负载均衡器示例](./media/load-balancer-overview/IC727496.png)

*图：使用公共负载均衡器对 Web 流量进行负载均衡*

当 Internet 客户端将网页请求发送到 TCP 端口 80 上的 Web 应用的公共 IP 地址时，Azure 负载均衡器会在负载均衡集中的三个虚拟机之间分配请求。 有关负载均衡器算法的详细信息，请参阅[负载均衡器概述页](load-balancer-overview.md#load-balancer-features)。

默认情况下，Azure 负载均衡器在多个虚拟机实例之间平均分发网络流量。 还可以配置会话关联。 有关详细信息，请参阅[负载均衡器分配模式](load-balancer-distribution-mode.md)。

### <a name = "internalloadbalancer"></a>内部负载均衡器。

内部负载均衡器仅将流量定向到虚拟网络中的资源，或定向到使用 VPN 访问 Azure 基础结构的资源。 在此方面，内部负载均衡器不同于公共负载均衡器。 Azure 基础结构会限制对虚拟网络的负载均衡前端 IP 地址的访问。 前端 IP 地址和虚拟网络不会直接在 Internet 终结点上公开。 内部业务线应用程序可在 Azure 中运行，并可从 Azure 内或从本地资源访问这些应用程序。

内部负载均衡器支持以下类型的负载均衡：

* 在虚拟网络中：从虚拟网络中的 VM 负载均衡到驻留在同一虚拟网络中的一组 VM。
* 对于跨界虚拟网络：从本地计算机负载均衡到驻留在同一虚拟网络中的一组 VM。 
* 对于多层应用程序：对面向 Internet 的多层应用程序进行负载均衡，其中的后端层不面向 Internet。 后端层需要针对面向 Internet 的层发出的流量进行负载均衡（参阅图 2）。
* 对于业务线应用程序：使托管在 Azure 中的业务线应用程序实现负载均衡，而无需其他负载均衡器硬件或软件。 此方案将本地服务器包含在一组流量已实现负载均衡的计算机中。

![内部负载均衡器示例](./media/load-balancer-overview/IC744147.png)

*图 - 使用公共和内部负载均衡器对多层应用程序进行负载均衡*

## <a name="pricing"></a>定价
<!-- Not Available on Standard Load Balancer is a charged product based on number of load balancing rules configured and all inbound and outbound data processed. For Standard Load Balancer pricing information, visit the [Load Balancer Pricing](https://www.azure.cn/pricing/details/load-balancer/) page.-->

基本负载均衡器是免费提供的。

<!-- Not Available on ## SLA-->
## <a name="next-steps"></a>后续步骤

<!-- Not Available on - Review [Standard Load Balancer in more detail](load-balancer-standard-overview.md)-->
<!-- Not Available on - Learn about using [Standard Load Balancer and Availability Zones](load-balancer-standard-availability-zones.md)-->
- 了解如何[对出站连接使用负载均衡器](load-balancer-outbound-connections.md)
<!-- Not Available on - Learn about [Load Balancer HA Ports](load-balancer-ha-ports-overview.md)-->
- 了解如何[对多个前端使用负载均衡器](load-balancer-multivip-overview.md)
- 了解 [VNet 服务终结点](../virtual-network/virtual-network-service-endpoints-overview.md)
- 了解如何创建[基本的公共负载均衡器](load-balancer-get-started-internet-portal.md)
<!--Update_Description: update meta properties, update link, wording update -->