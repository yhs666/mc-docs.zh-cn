---
title: Azure Functions 网络选项
description: 在 Azure Functions 中可用的所有网络选项的概述
services: functions
author: alexkarcher-msft
manager: jehollan
ms.service: azure-functions
ms.topic: conceptual
origin.date: 01/14/2019
ms.date: 04/26/2019
ms.author: v-junlch
ms.openlocfilehash: 0b1413a6ded221cf13d42ba74e84520b7d015886
ms.sourcegitcommit: 9642fa6b5991ee593a326b0e5c4f4f4910f50742
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2019
ms.locfileid: "64855452"
---
# <a name="azure-functions-networking-options"></a>Azure Functions 网络选项

本文档介绍了可在各种 Azure Functions 托管选项中使用的网络功能套件。 以下所有网络选项都可以提供某种功能来访问资源，而无需使用可通过 Internet 路由的地址，或限制对函数应用的 Internet 访问。 所有托管模型都具有不同的可用网络隔离级别，选择适当的级别可以满足网络隔离要求。

可按多种不同的方式托管函数应用。
    1. 消耗计划：可以动态缩放以响应负载，但只能提供极少量的网络隔离选项。
    1. 应用服务计划：可按固定的规模运行。

## <a name="networking-feature-matrix"></a>网络功能矩阵

|                |[消耗计划](functions-scale.md#consumption-plan)|[应用服务计划](functions-scale.md#app-service-plan)|
|----------------|-----------|----------------|---------|-----------------------|  
|[**入门 IP 限制**](#inbound-ip-restrictions)|✅是|✅是|
|[**VNET 集成**](#vnet-integration)|❌否|✅是|
|[**预览版 VNET 集成（Express Route 和服务终结点）**](#preview-vnet-integration)|❌否|⚠ 是|
|[**混合连接**](#hybrid-connections)|❌否|✅是|
|[**专用站点访问**](#private-site-access)|❌否|❌否|

⚠ 预览版功能，不适合在生产环境中使用

## <a name="inbound-ip-restrictions"></a>入门 IP 限制

通过 IP 限制，可定义允许访问应用的 IP 地址的允许/拒绝列表（按优先级排序）。 此允许列表可能包含 IPv4 和 IPv6 地址。 如果存在一个或多个条目，则在列表末尾会存在一个隐式的“拒绝所有”。 IP 限制功能可与所有函数托管选项配合使用。

> ![重要] 若要使用 Azure 门户编辑器，门户必须能够直接访问正在运行的函数应用，并且用于访问门户的设备的 IP 必须已加入白名单。 实施网络限制后，仍可以访问“平台功能”选项卡中的任何功能。

[在此处了解更多信息](/app-service/app-service-ip-restrictions)

## <a name="vnet-integration"></a>VNET 集成

函数应用可以通过 VNET 集成来访问 VNET 内部的资源。 可在应用服务计划中使用 VNET 集成。  

VNet 集成可让函数应用访问虚拟网络中的资源，但不授予通过虚拟网络对函数应用进行[专用站点访问](#private-site-access)的权限。

VNet 集成通常用于实现应用对 VNet 中的数据库和运行 Web 服务的访问。 使用 VNet 集成时，不需要公开 VM 中应用程序的公共终结点，可以改用无法通过 Internet 路由的专用地址。

VNET 集成的正式版依赖于使用 VPN 网关将函数应用连接到虚拟网络。 它可以在应用服务计划中托管的函数中使用。 若要了解如何配置此功能，请参阅[相同功能的应用服务文档](../app-service/web-sites-integrate-with-vnet.md#enabling-vnet-integration)。

### <a name="preview-vnet-integration"></a>预览版 VNET 集成

VNet 集成功能目前提供处于预览阶段的新版本。 该版本不依赖于点到站点 VPN，且同样支持对 ExpressRoute 和服务终结点上的资源进行访问。 此功能可在扩展到 PremiumV2 的应用服务计划中使用。

VNet 集成的新版本（目前为预览版）提供以下优势：

* 不需网关即可使用新的 VNet 集成功能
* 可以通过 ExpressRoute 连接来访问资源，除了与连接 ExpressRoute 的 VNet 集成以外，不需要任何其他配置。
* 可以通过正在运行的 Functions 使用服务终结点保护的资源。 若要实现这一点，请在用于 VNet 集成的子网上启用服务终结点。
* 无法通过新的 VNet 集成功能将触发器配置为使用服务终结点保护的资源。 
* 函数应用和 VNet 必须位于同一区域。
* 新功能要求在资源管理器 VNet 中有一个未使用的子网。
* 处于预览期的 VNet 集成新版本不支持生产工作负荷。
* 新的 VNet 集成功能尚不支持使用路由表和全球对等互联。
* 对每个潜在的函数应用实例使用一个地址。 由于子网大小在分配后不能更改，因此请使用可轻松支持最大规模大小的子网。 

## <a name="hybrid-connections"></a>混合连接

[混合连接](../service-bus-relay/relay-hybrid-connections-protocol.md)是可用于访问其他网络中的应用程序资源的一项 Azure 中继功能。 使用混合连接可以从应用访问应用程序终结点。 它不可用于访问你的应用程序。 在[应用服务计划](functions-scale.md#app-service-plan)中运行的函数可以使用混合连接。

在 Functions 中使用时，每个混合连接与单个 TCP 主机和端口组合相关联。 这意味着，混合连接终结点可以位于任何操作系统和任何应用程序上，前提是能够访问 TCP 侦听端口。 混合连接功能不知道、也不关心应用程序协议或者要访问的内容是什么。 它只提供网络访问。

有关详细信息，请参阅支持 Functions 和 Web 应用的[混合连接的应用服务文档](../app-service/app-service-hybrid-connections.md)。

## <a name="private-site-access"></a>专用站点访问

专用站点访问指的是仅可从专用网络（例如 Azure 虚拟网络内）对应用进行访问。 专用站点访问仅适用于部署了内部负载均衡器 (ILB) 的 ASE。  

可通过多种方法访问其他托管选项中的 VNET 资源，但只能使用 ASE 来通过 VNET 运行函数的触发器。

