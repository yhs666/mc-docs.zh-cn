---
title: Azure Functions 网络选项
description: 在 Azure Functions 中可用的所有网络选项的概述
services: functions
author: alexkarcher-msft
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
origin.date: 04/11/2019
ms.date: 07/18/2019
ms.author: v-junlch
ms.openlocfilehash: f87fdaefa36f9d3fb3ac587331459bf3b0ca9cb8
ms.sourcegitcommit: c61b10764d533c32d56bcfcb4286ed0fb2bdbfea
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/19/2019
ms.locfileid: "68331870"
---
# <a name="azure-functions-networking-options"></a>Azure Functions 网络选项

本文介绍了可在各种 Azure Functions 托管选项中使用的网络功能。 以下所有网络选项都可以提供某种功能来访问资源，而无需使用可通过 Internet 路由的地址，或限制对函数应用的 Internet 访问。 

托管模型具有不同的可用网络隔离级别。 选择适当的级别有助于满足网络隔离要求。

可按多种方式托管函数应用：

* 有一组计划选项可在多租户基础结构上运行，并提供不同级别的虚拟网络连接和缩放选项：
    * [消耗计划](functions-scale.md#consumption-plan)：可以动态缩放以响应负载，但只能提供极少量的网络隔离选项。
    * Azure [应用服务计划](functions-scale.md#app-service-plan)：可按固定的规模运行。

## <a name="matrix-of-networking-features"></a>网络功能矩阵

|                |[消耗计划](functions-scale.md#consumption-plan)|[应用服务计划](functions-scale.md#app-service-plan)|
|----------------|-----------|----------------|---------|-----------------------|  
|[入站 IP 限制](#inbound-ip-restrictions)|✅是|✅是|
|[虚拟网络集成](#virtual-network-integration)|❌否| ✅是|
|[预览版虚拟网络集成（Azure ExpressRoute 和出站服务终结点）](#preview-version-of-virtual-network-integration)|❌否|✅是|
|[混合连接](#hybrid-connections)|❌否|✅是|
|[专用站点访问](#private-site-access)|❌否|✅是|

## <a name="inbound-ip-restrictions"></a>入站 IP 限制

可以使用 IP 限制来定义允许/拒绝访问应用的 IP 地址的优先级排序列表。 此列表可以包含 IPv4 和 IPv6 地址。 如果存在一个或多个条目，则列表末尾会存在一个隐式的“拒绝所有”。 IP 限制可与所有函数托管选项配合使用。

> [!NOTE]
> 若要使用 Azure 门户编辑器，门户必须能够直接访问正在运行的函数应用。 此外，用于访问门户的设备的 IP 必须已加入白名单。 实施网络限制后，仍可以访问“平台功能”选项卡中的任何功能。 

有关详细信息，请参阅 [Azure 应用服务静态访问限制](../app-service/app-service-ip-restrictions.md)。

## <a name="virtual-network-integration"></a>虚拟网络集成

函数应用可以通过虚拟网络集成来访问虚拟网络内部的资源。 可在应用服务计划中使用此功能。 

虚拟网络集成可让函数应用访问虚拟网络中的资源，但不授予通过虚拟网络对函数应用进行[专用站点访问](#private-site-access)的权限。

可以使用虚拟网络集成来从应用访问虚拟网络中运行的数据库和 Web 服务。 使用虚拟网络集成就不需要公开 VM 上应用程序的公共终结点。 可以改用不可通过 Internet 路由的专用地址。

虚拟网络集成的正式版依赖于使用 VPN 网关将函数应用连接到虚拟网络。 它可以在应用服务计划中托管的函数中使用。 若要了解如何配置此功能，请参阅[将应用与 Azure 虚拟网络集成](../app-service/web-sites-integrate-with-vnet.md)。

### <a name="preview-version-of-virtual-network-integration"></a>虚拟网络集成预览版

新版的虚拟网络集成功能目前为预览版。 此版本不依赖于点到站点 VPN。 此版本支持通过 ExpressRoute 或服务终结点访问资源。 可以在扩展到 PremiumV2 的应用服务计划中使用此版本。

下面是此版本的一些特征：

* 无需网关即可使用它。
* 可以通过 ExpressRoute 连接来访问资源，除了与连接 ExpressRoute 的虚拟网络集成以外，不需要任何其他配置。
* 可以通过正在运行的 Functions 使用服务终结点保护的资源。 为此，请在用于虚拟网络集成的子网上启用服务终结点。
* 不能将触发器配置为使用服务终结点保护的资源。 
* 函数应用和虚拟网络必须位于同一区域。
* 新功能需要使用通过 Azure 资源管理器部署的虚拟网络中某个未使用的子网。
* 预览版功能不支持生产工作负荷。
* 该功能尚不支持路由表和全球对等互连。
* 对每个潜在的函数应用实例使用一个地址。 由于分配后无法更改子网大小，因此请使用可轻松支持最大规模大小的子网。 

## <a name="hybrid-connections"></a>混合连接

[混合连接](../service-bus-relay/relay-hybrid-connections-protocol.md)是可用于访问其他网络中的应用程序资源的一项 Azure 中继功能。 使用混合连接可以从应用访问应用程序终结点。 不能使用混合连接来访问应用程序。 在[应用服务计划](functions-scale.md#app-service-plan)中运行的函数可以使用混合连接。

在 Azure Functions 中使用时，每个混合连接与单个 TCP 主机和端口组合相关联。 这意味着，混合连接终结点可以位于任何操作系统和任何应用程序上，前提是能够访问 TCP 侦听端口。 混合连接功能不知道、也不关心应用程序协议或者要访问的内容是什么。 它只提供网络访问。

有关详细信息，请参阅支持应用服务计划中的 Functions 的[混合连接的应用服务文档](../app-service/app-service-hybrid-connections.md)。

## <a name="private-site-access"></a>专用站点访问

专用站点访问指的是仅可从专用网络（例如 Azure 虚拟网络内）对应用进行访问。 
* 如果配置了**服务终结点**，则可以在应用服务计划中使用专用站点访问。有关详细信息，请参阅[虚拟网络服务终结点](../virtual-network/virtual-network-service-endpoints-overview.md)
    * 请记住，如果使用服务终结点，即使已配置 VNET 集成，函数也仍对 Internet 拥有完全的出站访问权限。

可通过多种方法访问其他托管选项中的虚拟网络资源。

## <a name="next-steps"></a>后续步骤
若要详细了解网络和 Azure Functions ，请参阅以下链接： 

* [阅读 Azure Functions 网络常见问题解答](./functions-networking-faq.md)
* [详细了解虚拟网络与应用服务/Functions 的集成](../app-service/web-sites-integrate-with-vnet.md)
* [详细了解 Azure 中的虚拟网络](../virtual-network/virtual-networks-overview.md)
* [在不更改防火墙的情况下使用混合连接连接到单个本地资源](../app-service/app-service-hybrid-connections.md)

<!-- Update_Description: wording update -->