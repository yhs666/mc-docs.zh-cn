---
title: Azure Functions 网络选项
description: 在 Azure Functions 中可用的所有网络选项的概述
services: functions
author: alexkarcher-msft
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
origin.date: 04/11/2019
ms.date: 10/28/2019
ms.author: v-junlch
ms.openlocfilehash: 87ccab35286e29ea297500f13a98fcefd8b2a6cd
ms.sourcegitcommit: 7d2ea8a08ee329913015bc5d2f375fc2620578ba
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2019
ms.locfileid: "73034445"
---
# <a name="azure-functions-networking-options"></a>Azure Functions 网络选项

本文介绍了可在各种 Azure Functions 托管选项中使用的网络功能。 以下所有网络选项都可以提供某种功能来访问资源，而无需使用可通过 Internet 路由的地址，或限制对函数应用的 Internet 访问。 

托管模型具有不同的可用网络隔离级别。 选择适当的级别有助于满足网络隔离要求。

可按多种方式托管函数应用：

* 有一组计划选项可在多租户基础结构上运行，并提供不同级别的虚拟网络连接和缩放选项：
    * [消耗计划](functions-scale.md#consumption-plan)：可以动态缩放以响应负载，但只能提供极少量的网络隔离选项。
    * Azure [应用服务计划](functions-scale.md#app-service-plan)：可按固定的规模运行。

## <a name="matrix-of-networking-features"></a>网络功能矩阵

|                |[消耗计划](functions-scale.md#consumption-plan) |[应用服务计划](functions-scale.md#app-service-plan)| 
|----------------|-----------|----------------|---------|-----------------------|  
|[入站 IP 限制和专用站点访问](#inbound-ip-restrictions)|✅是 |✅是|
|[虚拟网络集成](#virtual-network-integration)|❌否 |✅是（区域和网关）| 
|[虚拟网络触发器（非 HTTP）](#virtual-network-triggers-non-http)|❌否 |✅是| 
|[混合连接](#hybrid-connections)|❌否 |✅是| 
|[出站 IP 限制](#outbound-ip-restrictions)|❌否 |❌否| 


## <a name="inbound-ip-restrictions"></a>入站 IP 限制

可以使用 IP 限制来定义允许/拒绝访问应用的 IP 地址的优先级排序列表。 此列表可以包含 IPv4 和 IPv6 地址。 如果存在一个或多个条目，则列表末尾会存在一个隐式的“拒绝所有”。 IP 限制可与所有函数托管选项配合使用。

> [!NOTE]
> 如果进行了网络限制，则只能在虚拟网络内部使用门户编辑器，或者只能在将用于访问 Azure 门户的计算机的 IP 加入允许列表的情况下使用该编辑器。 但是，仍可以从任何计算机访问“平台功能”选项卡中的任何功能。 

有关详细信息，请参阅 [Azure 应用服务静态访问限制](../app-service/app-service-ip-restrictions.md)。

## <a name="private-site-access"></a>专用站点访问

专用站点访问指的是仅可从专用网络（例如 Azure 虚拟网络内）对应用进行访问。 
* 配置了**服务终结点**时，[消耗](functions-scale.md#consumption-plan)和[应用服务计划](functions-scale.md#app-service-plan)中提供专用站点访问。 
    * 可以在“平台功能”>“网络”>“配置访问限制”>“添加规则”下，针对每个应用配置服务终结点。 现在可以选择虚拟网络作为规则的“类型”。
    * 有关详细信息，请参阅[虚拟网络服务终结点](../virtual-network/virtual-network-service-endpoints-overview.md)
        * 请记住，如果使用服务终结点，即使已配置虚拟网络集成，函数也仍对 Internet 拥有完全的出站访问权限。

## <a name="virtual-network-integration"></a>虚拟网络集成

函数应用可以通过虚拟网络集成来访问虚拟网络内部的资源。 可在应用服务计划中使用此功能。 

可以使用虚拟网络集成来从应用访问虚拟网络中运行的数据库和 Web 服务。 使用虚拟网络集成就不需要公开 VM 上应用程序的公共终结点。 可以改用不可通过 Internet 路由的专用地址。

虚拟网络集成功能有两种形式

1. 区域虚拟网络集成允许与同一区域中的虚拟网络集成。 这种形式的功能需要在同一区域的虚拟网络中有一个子网。 此功能仍为预览版，但支持用于 Windows 应用生产工作负荷，下面是一些注意事项。
2. 需要网关的虚拟网络集成允许与远程区域中的虚拟网络或经典虚拟网络集成。 这种版本的功能需要将虚拟网关部署到 VNet 中。 这是基于点到站点 VPN 的功能，仅受 Windows 应用的支持。

一个应用一次只能使用一种形式的 VNet 集成功能。 那么，问题来了，你应该使用哪种功能？ 在许多情况下，可以使用任意一种。 但是，主要区别是：

| 问题  | 解决方案 | 
|----------|----------|
| 想要访问同一区域中的某个 RFC 1918 地址（10.0.0.0/8、172.16.0.0/12、192.168.0.0/16） | 区域 VNet 集成 |
| 想要访问经典 VNet 或另一区域的 VNet 中的资源 | 需要网关的 VNet 集成 |
| 想要跨 ExpressRoute 访问 RFC 1918 终结点 | 区域 VNet 集成 |
| 想要跨服务终结点访问资源 | 区域 VNet 集成 |

两种功能都不允许跨 ExpressRoute 访问非 RFC 1918 地址。 

使用区域 VNet 集成不能将 VNet 连接到本地，也不能配置服务终结点。 这是单独的网络配置。 区域 VNet 集成只是允许应用跨这些连接类型进行调用。

不管所用版本为何，VNet 集成可让函数应用访问虚拟网络中的资源，但不授予通过虚拟网络对函数应用进行专用站点访问的权限。 专用站点访问指的是仅可从专用网络（例如 Azure 虚拟网络内）对应用进行访问。 VNet 集成只是为了从应用对 VNet 进行出站调用。 

VNet 集成功能：

* 需要标准应用服务计划
* 支持 TCP 和 UDP
* 使用应用服务应用和函数应用

VNet 集成不支持某些功能，其中包括：

* 装载驱动器
* AD 集成 
* NetBios

Functions 中的虚拟网络集成使用与应用服务 Web 应用共享的基础结构。 若要详细阅读两类虚拟网络集成，请参阅：
* [区域 VNET 集成](../app-service/web-sites-integrate-with-vnet.md#regional-vnet-integration)
* [需要网关的 VNet 集成](../app-service/web-sites-integrate-with-vnet.md#gateway-required-vnet-integration)

## <a name="connecting-to-service-endpoint-secured-resources"></a>连接到服务终结点保护的资源

> [!note] 
> 对下游资源配置访问限制后，暂时可能需要多达 12 个小时才能使新的服务终结点对函数应用可用。 在此期间，资源将对应用完全不可用。

为了提供更高级别的安全性，可以使用服务终结点将许多 Azure 服务限制在一个虚拟网络中。 然后，必须将函数应用与该虚拟网络集成，才能访问资源。 支持虚拟网络集成的所有计划都支持此配置。

[在此处详细了解虚拟网络服务终结点。](../virtual-network/virtual-network-service-endpoints-overview.md)

### <a name="restricting-your-storage-account-to-a-virtual-network"></a>将存储帐户限制到虚拟网络
创建函数应用时，必须创建或链接到支持 Blob、队列和表存储的常规用途的 Azure 存储帐户。 目前不能对此帐户使用任何虚拟网络限制。 如果在用于函数应用的存储帐户上配置虚拟网络服务终结点，则会破坏应用。

[在此处详细了解存储帐户要求。](./functions-create-function-app-portal.md#storage-account-requirements)

## <a name="virtual-network-triggers-non-http"></a>虚拟网络触发器（非 HTTP）

目前，若要能够在虚拟网络中使用不同于 HTTP 的函数触发器，必须在应用服务计划中运行函数应用。

例如，如果要将 Azure Cosmos DB 配置为仅接受来自虚拟网络的流量，则需在与该虚拟网络进行虚拟网络集成的应用服务计划中部署函数应用，以便从该资源配置 Azure Cosmos DB 触发器。 

请检查[此列表中的所有非 HTTP 触发器](./functions-triggers-bindings.md#supported-bindings)，再次确认支持的内容。

## <a name="hybrid-connections"></a>混合连接

[混合连接](../service-bus-relay/relay-hybrid-connections-protocol.md)是可用于访问其他网络中的应用程序资源的一项 Azure 中继功能。 使用混合连接可以从应用访问应用程序终结点。 不能使用混合连接来访问应用程序。 在除消耗计划以外的所有计划中运行的函数都可以使用混合连接。

在 Azure Functions 中使用时，每个混合连接与单个 TCP 主机和端口组合相关联。 这意味着，混合连接终结点可以位于任何操作系统和任何应用程序上，前提是能够访问 TCP 侦听端口。 混合连接功能不知道、也不关心应用程序协议或者要访问的内容是什么。 它只提供网络访问。

若要了解详细信息，请参阅[混合连接的应用服务文档](../app-service/app-service-hybrid-connections.md)，该文档通过相同的配置步骤支持 Functions。


## <a name="next-steps"></a>后续步骤
若要详细了解网络和 Azure Functions ，请参阅以下链接： 

* [阅读 Azure Functions 网络常见问题解答](./functions-networking-faq.md)
* [详细了解虚拟网络与应用服务/Functions 的集成](../app-service/web-sites-integrate-with-vnet.md)
* [详细了解 Azure 中的虚拟网络](../virtual-network/virtual-networks-overview.md)
* [在不更改防火墙的情况下使用混合连接连接到单个本地资源](../app-service/app-service-hybrid-connections.md)

<!-- Update_Description: wording update -->