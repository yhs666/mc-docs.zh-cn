---
title: Azure 应用程序网关的工作原理
description: 本文提供有关应用程序网关工作原理的信息
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
origin.date: 02/20/2019
ms.date: 02/26/2019
ms.author: v-junlch
ms.openlocfilehash: 9951d2e88e71d978b208f075d702351d855081ff
ms.sourcegitcommit: e9f088bee395a86c285993a3c6915749357c2548
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/26/2019
ms.locfileid: "56837021"
---
# <a name="how-application-gateway-works"></a>应用程序网关的工作原理

应用程序网关是提供第 7 层 (HTTP) 负载均衡的云应用程序传送控制器，可用于管理跨服务器的 Web 流量。 此外，它还提供 Web 应用程序防火墙 (WAF) 功能用于集中保护 Web 服务，使其免受常见 Web 恶意利用和漏洞的影响。

当客户端向应用程序网关发出 HTTP 请求时，应用程序网关充当反向代理，根据配置将流量转发到后端服务器。 此外，应用程序网关还会监视其后端服务器的运行状况，确保只将流量路由到正常运行的后端。

![how-application-gateway-works](./media/how-application-gateway-works/how-application-gateway-works.png)

## <a name="how-request-is-accepted"></a>请求接受方式

客户端将请求发送到应用程序网关之前，会使用域名系统 (DNS) 服务器解析应用程序网关的域名。 由于应用程序网关位于 azure.cn 域中，因此 DNS 条目受 Azure 的控制。 Azure DNS 将 IP 地址返回到客户端，即应用程序网关的前端 IP 地址。 应用程序网关接受一个或多个侦听器上的传入流量。 侦听器是检查连接请求的进程。 侦听器上为客户端到应用程序网关的连接配置了前端 IP 地址、协议和端口号。 如果启用了 WAF，则应用程序网关会根据 WAF 规则检查请求标头和正文（如果有），以确定请求是有效的请求（在这种情况下，会将请求路由到后端）还是安全威胁（在这种情况下，将阻止请求）。  

## <a name="how-request-is-routed"></a>请求路由方式

如果发现请求有效（或如果未启用 WAF），则评估与侦听器关联的请求路由规则，以确定请求要路由到的后端池。 规则将按照门户中的列出顺序进行处理。 根据请求路由规则配置，应用程序网关确定是要将侦听器上的所有请求路由到特定的后端池、根据 URL 路径路由到不同的后端池，还是将请求重定向到另一个端口或外部站点

选择后端池后，应用程序网关将请求发送到该池中配置的正常后端服务器之一 (y.y.y.y)。 服务器的运行状况由运行状况探测决定。 如果后端池包含多个服务器，则应用程序网关会使用轮循机制算法在正常运行的服务器之间路由请求，因此将对服务器上的请求进行负载均衡。

确定后端服务器之后，应用程序网关会根据 HTTP 设置中的配置，与后端服务器建立新的 TCP 会话。 HTTP 设置组件指定与后端服务器建立新会话所需的协议、端口和其他路由相关设置。 HTTP 设置中使用的端口和协议确定应用程序网关与后端服务器之间的流量是要加密（从而可实现端到端的 SSL）还是不加密。 将原始请求发送到后端服务器时，应用程序网关遵循 HTTP 设置中指定的任何自定义配置，这些配置与以下操作相关：替代主机名、路径和协议；保持基于 Cookie 的会话相关性、连接清空、从后端选取主机名，等等。

应用程序网关还会将 x-forwarded-for、x-forwarded-proto 和 x-forwarded-port 这三个默认的 HTTP* 标头插入转发到后端的请求中。

后端服务器处理请求并将包含页面内容的 HTTP 响应发送到应用程序网关之后，应用程序网关会将该响应转发到客户端，而页面将显示在浏览器中。

## <a name="application-load-balancing-type"></a>应用程序负载均衡类型

可以使用应用程序网关作为内部应用程序负载均衡器或面向 Internet 的应用程序负载均衡器。 面向 Internet 的应用程序网关使用公共 IP 地址。 面向 Internet 的应用程序网关的 DNS 名称可公开解析为其公共 IP 地址。 因此，面向 Internet 的应用程序网关可以通过 Internet 路由来自客户端的请求。

内部应用程序网关仅使用专用 IP 地址。 内部应用程序网关的 DNS 名称可公开解析为其专用 IP 地址。 因此，内部负载均衡器只能路由有权访问应用程序网关 VNET 的客户端发出的请求。

请注意，面向 Internet 的应用程序网关和内部应用程序网关都使用专用 IP 地址将请求路由到后端服务器。 因此，后端服务器不需要公共 IP 地址即可接收来自内部应用程序网关或面向 Internet 的应用程序网关的请求。

## <a name="next-steps"></a>后续步骤

了解应用程序网关的工作原理后，请转到[应用程序网关组件](application-gateway-components.md)以更详细地了解其组件。

