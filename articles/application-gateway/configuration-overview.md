---
title: Azure 应用程序网关配置概述
description: 本文提供有关如何配置 Azure 应用程序网关中各个组件的信息
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
origin.date: 03/20/2019
ms.date: 03/29/2019
ms.author: v-junlch
ms.openlocfilehash: 2dcfda8c8f204f10c6d938672a3f03968a478cd8
ms.sourcegitcommit: b8fb6890caed87831b28c82738d6cecfe50674fd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/29/2019
ms.locfileid: "58628636"
---
# <a name="application-gateway-configuration-overview"></a>应用程序网关配置概述

应用程序网关由多个组件构成，可根据不同的场景以不同的方式配置这些组件。 本文逐步讲解如何配置每个组件。

![application-gateway-components](./media/configuration-overview/configuration-overview1.png)

上面的示例插图演示了包含 3 个侦听器的应用程序的配置。 前两个侦听器是分别用于 `http://acme.com/*` 和 `http://fabrikam.com/*` 的多站点侦听器， 两者在端口 80 上侦听。 第三个侦听器是支持端到端 SSL 终止的基本侦听器。 

## <a name="prerequisites"></a>先决条件

### <a name="azure-virtual-network-and-dedicated-subnet"></a>Azure 虚拟网络和专用子网

应用程序网关是虚拟网络中的专用部署。 需要在虚拟网络中为应用程序网关配置一个专用子网。 在此子网中，可以创建给定应用程序网关部署的多个实例。 还可以在该子网中部署其他应用程序网关，但不能在应用程序网关子网中部署其他任何资源。  

> [!NOTE]   
> 不支持在同一子网上混合使用 Standard_v2 和标准应用程序网关。

#### <a name="size-of-the-subnet"></a>子网的大小

如果配置了专用前端 IP 配置，则应用程序网关使用每个实例的一个专用 IP 地址，以及另一个专用 IP 地址。 另外，Azure 会在每个子网中保留五个 IP 地址（前四个 IP 地址和最后一个 IP 地址）供内部使用。 例如，如果应用程序网关设置为 15 个实例并且没有专用前端 IP，则子网中至少需要 20 个 IP 地址 - 5个 IP 地址供内部使用，15 个 IP 地址用于应用程序网关的 15 个实例。 因此，在本例中，需要 /27 或更大的子网大小。 如果有 27 个实例并且专用前端 IP 配置具有 IP 地址，则需要 33 个 IP 地址 - 27 个 IP 地址用于应用程序网关的 27 个实例，1 个 IP 地址用于专用前端 IP，5 个 IP 地址供内部使用。 因此，在本例中，需要 /26 或更大的子网大小。

建议至少使用 /28 子网大小。 这样可以获得 11 个可用地址。 如果应用程序负载需要 10 个以上的实例，则应考虑 /27 或 /26 子网大小。

#### <a name="network-security-groups-supported-on-the-application-gateway-subnet"></a>应用程序网关子网支持网络安全组

应用程序网关子网支持网络安全组 (NSG)，但存在以下限制： 

- 对于应用程序网关 v1 SKU，必须为端口 65503-65534 上的传入流量设置例外。 此端口范围是进行 Azure 基础结构通信所必需的。 它们受 Azure 证书的保护（处于锁定状态）。 如果没有适当的证书，外部实体（包括这些网关的客户）将无法对这些终结点做出任何更改。

- 不能阻止出站 Internet 连接。 NSG 中的默认出站规则已经允许 Internet 连接。 建议不要删除默认的出站规则，且不要创建其他拒绝出站 Internet 连接的出站规则。

- 必须允许来自 AzureLoadBalancer 标记的流量。

##### <a name="whitelist-application-gateway-access-to-a-few-source-ips"></a>将应用程序网关列入白名单以便能够访问一些源 IP

对应用程序网关子网使用 NSG 可以完成此方案。 应按列出的优先顺序对子网采取以下限制：

1. 允许来自源 IP/IP 范围的传入流量。
2. 允许来自所有源的请求传入端口 65503-65534，进行[后端运行状况通信](/application-gateway/application-gateway-diagnostics)。 此端口范围是进行 Azure 基础结构通信所必需的。 它们受 Azure 证书的保护（处于锁定状态）。 如果没有适当的证书，外部实体（包括这些网关的客户）将无法对这些终结点做出任何更改。
3. 允许 [NSG](/virtual-network/security-overview) 上的传入 Azure 负载均衡器探测（AzureLoadBalancer 标记）和入站虚拟网络流量（VirtualNetwork 标记）。
4. 使用“全部拒绝”规则阻止其他所有传入流量。
5. 允许发往 Internet 的所有目标的出站流量。

#### <a name="user-defined-routes-supported-on-the-application-gateway-subnet"></a>应用程序网关子网支持用户定义的路由

使用 v1 SKU 时，只要用户定义的路由 (UDR) 未更改端到端请求/响应通信，则应用程序网关子网支持用户定义的路由。 例如，可以在应用程序网关子网中设置 UDR 来指向用于数据包检查的防火墙设备，但必须确保数据包在检查后可以到达其预定目的地。 如果做不到这一点，可能会导致不正确的运行状况探测或流量路由行为。 这包括已了解的路由或通过 ExpressRoute 或 VPN 网关在虚拟网络中传播的默认 0.0.0.0/0 路由。

> [!NOTE]
> 在应用程序网关子网中使用 UDR 会导致[后端运行状况视图](/application-gateway/application-gateway-diagnostics#back-end-health)中的运行状态显示为“未知”，同时会导致应用程序网关日志和指标生成失败。 建议不要在应用程序网关子网中使用 UDR，以便能够查看后端运行状况、日志和指标。

## <a name="frontend-ip"></a>前端 IP

可将应用程序网关配置为使用公共 IP 地址和/或专用 IP 地址。 托管需要由客户端在 Internet 中通过面向 Internet 的 VIP 访问的后端时，需要使用公共 IP。 不向 Internet 公开的内部终结点（也称为内部负载均衡器 (ILB) 终结点）不需要公共 IP。 配置使用 ILB 的网关适用于不向 Internet 公开的内部业务线应用程序。 对于位于不向 Internet 公开的安全边界内的多层应用程序中的服务和层也很有用，但仍需要执行循环负载分散、会话粘性或安全套接字层 (SSL) 终止。

仅支持一个公共 IP 地址或一个专用 IP 地址。 在创建应用程序网关时选择前端 IP。 

- 如果使用公共 IP，则可以选择在应用程序网关所在的同一位置创建新的公共 IP 或使用现有的公共 IP。 如果创建新的公共 IP 地址，则以后无法更改选定的 IP 地址类型（静态或动态）。 有关详细信息，请参阅[静态与动态公共 IP](/application-gateway/application-gateway-components#static-vs-dynamic-public-ip) 

- 如果使用专用 IP，则可以选择在创建应用程序网关的子网中指定一个专用 IP 地址。 如果不显式指定，则系统会在该子网中自动选择一个任意 IP 地址。 有关详细信息，请参阅[使用内部负载均衡器 (ILB) 终结点创建应用程序网关](/application-gateway/application-gateway-ilb-arm)。

某个前端 IP 将关联到检查前端 IP 上的传入请求的侦听器。

## <a name="listeners"></a>侦听器

侦听器是一个逻辑实体，它可以使用端口、协议、主机和 IP 地址检查传入的连接请求。 因此，在配置侦听器时，需要输入与网关上传入请求中的对应值相同的端口、协议、主机和 IP 地址值。 使用 Azure 门户创建应用程序网关时，还可以通过选择侦听器的协议和端口来创建默认的侦听器。 此外，可以选择是否要在侦听器上启用 HTTP2 支持。 创建应用程序网关后，可以编辑此默认侦听器的设置 (*appGatewayHttpListener*/*appGatewayHttpsListener*) 和/或创建新的侦听器。

### <a name="listener-type"></a>侦听器类型

创建新侦听器时，可以选择[基本侦听器或多站点侦听器](/application-gateway/application-gateway-components#types-of-listeners)。 

- 如果在应用程序网关后面托管单个站点，请选择基本侦听器。 了解[如何创建包含基本侦听器的应用程序网关](/application-gateway/quick-create-portal)。

- 如果在同一个应用程序网关实例上配置具有相同父域的多个 Web 应用程序或多个子域，请选择多站点侦听器。 对于多站点侦听器，还需要输入主机名。 这是因为，应用程序网关需要使用 HTTP 1.1 主机标头才能在相同的公共 IP 地址和端口上托管多个网站。

#### <a name="order-of-processing-listeners"></a>侦听器的处理顺序

使用 v1 SKU 时，侦听器将按其显示顺序进行处理。 因此，如果基本侦听器与传入请求匹配，它会先处理该请求。 因此，应将多站点侦听器配置在基本侦听器之前，以确保将流量路由到正确的后端。

### <a name="frontend-ip"></a>前端 IP

选择要与此侦听器关联的前端 IP。 侦听器将在此 IP 上侦听传入的请求。

### <a name="frontend-port"></a>前端端口

选择前端端口。 可以选择现有的端口，或者创建新端口。 可以选择[允许的端口范围](/application-gateway/application-gateway-components#ports)内的任意值。 这样，不仅可以使用已知的端口（例如 80 和 443），而且还能根据具体的用途，使用任何允许的自定义端口。 一个端口可用于公共侦听器或专用侦听器。

### <a name="protocol"></a>协议

需要在 HTTP 与 HTTPS 协议之间进行选择。 

- 如果选择 HTTP，则客户端与应用程序网关之间的流量将以未加密的形式传送。

- 如果想要实现[安全套接字层 (SSL) 终止](/application-gateway/overview#secure-sockets-layer-ssl-terminationl)或[端到端 SSL 加密](/application-gateway/ssl-overview)，请选择 HTTPS。 如果选择 HTTPS，则客户端与应用程序网关之间的流量将会加密，并且 SSL 连接将在应用程序网关上终止。  如需端到端的 SSL 加密，则在配置后端 HTTP 设置时，还需要额外选择 HTTPS 协议。 这可以确保流量在从应用程序网关传输到后端时重新得到加密。

  若要配置安全套接字层 (SSL) 终止和端到端 SSL 加密，需将一个证书添加到侦听器，使应用程序网关能够根据 SSL 协议规范派生对称密钥。 然后，可以使用该对称密钥来加密和解密发送到网关的流量。 网关证书需要采用个人信息交换 (PFX) 格式。 此文件格式适用于导出私钥，后者是应用程序网关对流量进行加解密所必需的。 

#### <a name="supported-certificates"></a>支持的证书

请参阅[支持用于 SSL 终止的证书](/application-gateway/ssl-overview#certificates-supported-for-ssl-termination)。

### <a name="additional-protocol-support"></a>其他协议支持

#### <a name="http2-support"></a>HTTP2 支持

仅针对连接到应用程序网关侦听程序的客户端提供了 HTTP/2 协议支持。 与后端服务器池的通信是通过 HTTP/1.1 进行的。 默认情况下，HTTP/2 支持处于禁用状态。 以下 Azure PowerShell 代码片段示例展示了如何启用该支持：

```azurepowershell
$gw = Get-AzureRmApplicationGateway -Name test -ResourceGroupName hm

$gw.EnableHttp2 = $true

Set-AzureRmApplicationGateway -ApplicationGateway $gw
```

#### <a name="websocket-support"></a>WebSocket 支持

默认已启用 WebSocket 支持。 用户无法通过配置设置来选择性地启用或禁用 WebSocket 支持。 可对 HTTP 和 HTTPS 侦听器使用 WebSocket。 

### <a name="custom-error-page"></a>自定义错误页

可以在全局级别以及侦听器级别定义自定义错误页，但是，目前不支持通过 Azure 门户创建全局级别的自定义错误页。 可以在侦听器级别为 403 WAF 错误或 502 维护页配置自定义错误页。 还需要为给定的错误状态代码指定一个可公开访问的 Blob URL。 有关详细信息，请参阅[创建自定义错误页](/application-gateway/custom-error)。

![应用程序网关错误代码](./media/custom-error/ag-error-codes.png)

若要配置全局自定义错误页，请使用 [Azure PowerShell](/application-gateway/custom-error#azure-powershell-configuration) 进行配置 

### <a name="ssl-policy"></a>SSL 策略

可以通过后端服务器场来集中管理 SSL 证书和降低加密与解密开销。 这种集中式 SSL 处理还允许指定适合你组织安全要求的中央 SSL 策略。  可以选择默认、预定义或自定义的 SSL 策略。 

可以配置 SSL 策略来控制 SSL 协议版本。 可将应用程序网关配置为拒绝 TLS1.0、TLS1.1 和 TLS1.2。 SSL 2.0 和 3.0 默认已禁用，并且不可配置。 有关详细信息，请参阅[应用程序网关 SSL 策略概述](/application-gateway/application-gateway-ssl-policy-overview)。

创建侦听器后，将它与某个请求路由规则相关联。该规则确定如何将侦听器上收到的请求路由到后端。

## <a name="request-routing-rule"></a>请求路由规则

使用 Azure 门户创建应用程序网关时，可创建一个默认规则 (*rule1*)，该规则会将默认侦听器 (*appGatewayHttpListener*) 绑定到默认后端池 (*appGatewayBackendPool*) 和默认后端 HTTP 设置 (*appGatewayBackendHttpSettings*)。 创建应用程序网关后，可以编辑此默认规则的设置和/或创建新的规则。

### <a name="rule-type"></a>规则类型

创建新规则时，可以选择[基本规则或基于路径的规则](/application-gateway/application-gateway-components#request-routing-rule)。 

- 若要将关联的侦听器（例如 blog.contoso.com/*）上的所有请求转发到单个后端池，请选择基本侦听器。 
- 若要将包含特定 URL 路径的请求转发到特定的后端池，请选择基于路径的侦听器。 路径模式仅应用到 URL 的路径，而不应用到该 URL 的查询参数。


#### <a name="order-of-processing-rules"></a>规则的处理顺序

使用 v1 SKU 时，将按照路径在基于路径的规则的 URL 路径映射中的列出顺序处理传入请求的模式匹配。 出于此原因，如果某个请求与 URL 路径映射中的两个或更多个路径的模式相匹配，则会匹配最先列出的路径，并将请求转发到与该路径匹配的后端。

### <a name="associated-listener"></a>关联的侦听器

需要将一个侦听器关联到该规则，以便评估与该侦听器关联的请求路由规则，从而确定请求要路由到的后端池。

### <a name="associated-backend-pool"></a>关联的后端池

关联包含后端目标的后端池，该池将为侦听器收到的请求提供服务。 如果使用基本规则，则仅允许一个后端池，因为关联的侦听器上的所有请求将转发到此后端池。 如果使用基于路径的规则，请添加对应于每个 URL 路径的多个后端池。 与此处输入的 URL 路径匹配的请求将转发到相应的后端池。 另请添加默认后端池，因为与在此规则中输入的任何 URL 路径都不匹配的请求将转发到该后端池。

### <a name="associated-backend-http-setting"></a>关联的后端 HTTP 设置

为每个规则添加后端 HTTP 设置。 系统使用此设置中指定的端口号、协议和其他设置，将请求从应用程序网关路由到后端目标。 如果使用基本规则，则仅允许一个后端 HTTP 设置，因为系统会使用此 HTTP 设置将关联的侦听器上的所有请求转发到相应的后端目标。 如果使用基于路径的规则，请添加对应于每个 URL 路径的多个后端 HTTP 设置。 系统使用对应于每个 URL 路径的 HTTP 设置，将与此处输入的 URL 路径匹配的请求转发到相应的后端目标。 另请添加默认 HTTP 设置，因为系统会使用默认 HTTP 设置，将与在此规则中输入的任何 URL 路径都不匹配的请求转发到默认后端池。

### <a name="redirection-setting"></a>重定向设置

如果为基本规则配置了重定向，则关联的侦听器上的所有请求将重定向到重定向目标，从而实现全局重定向。 如果为基于路径的规则配置了重定向，则只会将特定站点区域（例如，由 /cart/* 表示的购物车区域）中的请求重定向到重定向目标，从而实现基于路径的重定向。 

有关重定向功能的信息，请参阅[重定向概述](/application-gateway/redirect-overview)。

- #### <a name="redirection-type"></a>重定向类型

  所需的重定向类型包括：Permanent(301)、Temporary(307)、Found(302) 或 See other(303)。

- #### <a name="redirection-target"></a>重定向目标

  可以选择另一个侦听器或外部站点作为重定向目标。 

  - ##### <a name="listener"></a>侦听器

    选择侦听器作为重定向目标有助于从网关上的一个侦听器重定向到另一个侦听器。 想要启用 HTTP 到 HTTPS 的重定向时 - 例如，将来自源侦听器（用于检查 HTTP 请求）的流量重定向到目标侦听器（用于检查传入的 HTTPS 请求）- 必须指定此设置。 还可以选择原始请求中的、要包含在转发到重定向目标的请求中的查询字符串和路径。![application-gateway-components](./media/configuration-overview/configure-redirection.png)

    有关 HTTP 到 HTTPS 的重定向的详细信息，请参阅[使用门户配置 HTTP 到 HTTP 的重定向](/application-gateway/redirect-http-to-https-portal)、[使用 PowerShell 配置 HTTP 到 HTTP 的重定向](/application-gateway/redirect-http-to-https-powershell)和[使用 CLI 配置 HTTP 到 HTTP 的重定向](/application-gateway/redirect-http-to-https-cli)

  - ##### <a name="external-site"></a>外部站点

    若要将与此类规则关联的侦听器上的流量重定向到外部站点，请选择外部站点。 可以选择原始请求中的、要包含在转发到重定向目标的请求中的查询字符串。 无法将原始请求中的路径转发到外部站点。

    有关重定向到外部站点的详细信息，请参阅[使用 PowerShell 将流量重定向到外部站点](/application-gateway/redirect-external-site-powershell)和 [/application-gateway/redirect-external-site-cli](/application-gateway/redirect-external-site-cli)

## <a name="http-settings"></a>HTTP 设置

应用程序网关使用此组件中指定的配置将流量路由到后端服务器。 创建 HTTP 设置后，需将其关联到一个或多个请求路由规则。

### <a name="cookie-based-affinity"></a>基于 Cookie 的相关性

需要在同一台服务器上保留用户会话时，此功能非常有用。 借助网关托管的 Cookie，应用程序网关可以将来自用户会话的后续流量定向到同一服务器进行处理。 在用户会话的会话状态在服务器上进行本地保存的情况下，此功能十分重要。 如果应用程序无法处理基于 Cookie 的相关性，则你无法使用此功能。 若要使用基于 Cookie 的会话相关性，应确保客户端支持 Cookie。 

### <a name="connection-draining"></a>连接清空

连接清空可帮助你在计划内服务更新期间正常删除后端池成员。 在创建规则期间，可将此设置应用到后端池的所有成员。 启用后，应用程序网关可确保后端池的所有已取消注册实例不再收到任何新请求，同时允许现有请求在所配置的时间限制内完成。 这适用于通过 API 调用显式从后端池中删除的后端实例，以及所报告的由运行状况探测确定为不正常的后端实例。

### <a name="protocol"></a>协议

应用程序网关支持使用 HTTP 和 HTTPS 协议将请求路由到后端服务器。 如果选择了 HTTP 协议，则流量将以未加密的形式传送到后端服务器。 如果无法接受与后端服务器之间进行未加密的通信，则应选择 HTTPS 协议。 如果使用此设置并在侦听器中选择 HTTPS 协议，则可以实现[端到端的 SSL](/application-gateway/ssl-overview)。 这样，就可以安全地将敏感数据以加密的形式传输到后端。 后端池中每个已启用端到端 SSL 的后端服务器都必须配置证书，以便能够进行安全的通信。

### <a name="port"></a>端口

后端服务器在此端口上侦听来自应用程序网关的流量。 可以配置 1 到 65535 的端口号。

### <a name="request-timeout"></a>请求超时

应用程序网关等待后端池做出响应的秒数，如此超过此秒数，则会返回“连接超时”错误。

### <a name="override-backend-path"></a>替代后端路径

使用此设置可以配置可选的自定义转发路径，以便在将请求转发到后端时使用。 这会将与“替代后端路径”字段中指定的自定义路径匹配的任意传入路径部分复制到转发的路径。 请参阅下表了解此功能的工作原理。

- 将 HTTP 设置附加到基本请求路由规则时：

  | 原始请求  | 替代后端路径 | 转发到后端的请求 |
  | ----------------- | --------------------- | ---------------------------- |
  | /home/            | /override/            | /override/home/              |
  | /home/secondhome/ | /override/            | /override/home/secondhome/   |

- 将 HTTP 设置附加到基于路径的请求路由规则时：

  | 原始请求           | 路径规则       | 替代后端路径 | 转发到后端的请求 |
  | -------------------------- | --------------- | --------------------- | ---------------------------- |
  | /pathrule/home/            | /pathrule*      | /override/            | /override/home/              |
  | /pathrule/home/secondhome/ | /pathrule*      | /override/            | /override/home/secondhome/   |
  | /home/                     | /pathrule*      | /override/            | /override/home/              |
  | /home/secondhome/          | /pathrule*      | /override/            | /override/home/secondhome/   |
  | /pathrule/home/            | /pathrule/home* | /override/            | /override/                   |
  | /pathrule/home/secondhome/ | /pathrule/home* | /override/            | /override/secondhome/        |

### <a name="use-for-app-service"></a>用于应用服务

这是一个 UI 快捷方式，用于选择应用服务后端的两个所需设置 - 启用“从后端地址中选取主机名”，以及创建新的自定义探测。 有关“从后端地址中选取主机名”设置的部分解释了为何要启用前一项设置。 如果同时从后端成员地址中选取了探测标头，则会创建新的探测。

### <a name="use-custom-probe"></a>使用自定义探测

此设置用于将[自定义探测](/application-gateway/application-gateway-probe-overview#custom-health-probe)与此 HTTP 设置相关联。 只能将一个自定义探测关联到某个 HTTP 设置。 如果未显式关联自定义探测，则会使用[默认探测](/application-gateway/application-gateway-probe-overview#default-health-probe-settings)来监视后端的运行状况。 建议创建自定义探测，以便能够更精细地控制后端的运行状况监视。

> [!NOTE]   
> 只有在将相应的 HTTP 设置显式关联到某个侦听器之后，自定义探测才会开始监视后端池的运行状况。

### <a name="pick-host-name-from-backend-address"></a>从后端地址中选取主机名

此功能使用 IP 地址或完全限定的域名 (FQDN) 动态将请求中的 *host* 标头设置为后端池的主机名。 如果后端的域名不同于应用程序网关的 DNS 名称，并且后端必须使用特定的 host 标头或 SNI 扩展才能解析为正确的终结点（例如，使用多租户服务作为后端时），则此功能非常有用。 由于应用服务是使用共享空间和单个 IP 地址的多租户服务，因此，只能使用自定义域设置中配置的主机名访问应用服务。 自定义域名默认为 *example.chinacloudsites.cn*。 因此，若要使用未显式注册到应用服务中的主机名或者使用应用程序网关的 FQDN 通过应用程序网关访问应用服务，则必须通过启用“从后端地址中选取主机名”设置，将原始请求中的主机名替代为应用服务的主机名。

如果你拥有某个自定义域并已将现有的自定义 DNS 名称映射到了应用服务，则不需要启用此设置。

> [!NOTE]   
> 应用服务环境 (ASE) 不需要此设置，因为 ASE 是专用部署。 

### <a name="host-name-override"></a>主机名替代

此功能可将应用程序网关上的传入请求中的 *host* 标头替换为此处指定的主机名。 例如，如果将 www\.contoso.com 指定为“主机名”设置，则将请求转发到后端服务器时，原始请求 https://appgw.chinanorth.chinacloudapp.cn/path1 将更改为 https://www.contoso.com/path1。 

## <a name="backend-pool"></a>后端池

后端池可以指向四种类型的后端成员：特定的虚拟机、虚拟机规模集、IP 地址/FQDN 或应用服务。 每个后端池可以指向同一类型的多个成员。 不支持指向同一后端池中不同类型的成员。 

创建后端池后，需将其关联到一个或多个请求路由规则。 此外，需要为应用程序网关上的每个后端池配置运行状况探测。 满足请求路由规则条件时，应用程序网关会将流量转发到相应后端池中正常运行的服务器（是否正常由运行状况探测决定）。

## <a name="health-probes"></a>运行状况探测

尽管应用程序网关默认会监视其后端中所有资源的运行状况，则我们强烈建议为每个后端 HTTP 设置创建一个自定义探测，以便能够更精细地控制运行状况监视。 若要了解如何配置自定义运行状况探测，请参阅[自定义运行状况探测设置](/application-gateway/application-gateway-probe-overview#custom-health-probe-settings)。

> [!NOTE]   
> 创建自定义运行状况探测后，需将其关联到后端 HTTP 设置。 只有在将相应的 HTTP 设置显式关联到某个侦听器之后，自定义探测才会开始监视后端池的运行状况。

## <a name="next-steps"></a>后续步骤

了解应用程序网关组件后，可以：

- [在 Azure 门户中创建应用程序网关](quick-create-portal.md)
- [使用 PowerShell 创建应用程序网关](quick-create-powershell.md)
- [使用 Azure CLI 创建应用程序网关](quick-create-cli.md)

