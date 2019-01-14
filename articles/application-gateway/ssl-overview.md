---
title: 在 Azure 应用程序网关上启用端到端 SSL
description: 本文概述应用程序网关的端到端 SSL 支持。
services: application-gateway
author: amsriva
ms.service: application-gateway
ms.topic: article
origin.date: 10/23/2018
ms.date: 01/09/2019
ms.author: v-junlch
ms.openlocfilehash: 3c24f7aca6b54deb55ebc1ba4a46215e88cb09d8
ms.sourcegitcommit: 023ab8b40254109d9edae1602c3488d13ef90954
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/09/2019
ms.locfileid: "54141676"
---
# <a name="overview-of-end-to-end-ssl-with-application-gateway"></a>应用程序网关的端到端 SSL 概述

应用程序网关支持在网关上终止 SSL，之后，流量通常会以未加密状态流到后端服务器。 此功能让 Web 服务器不用再负担昂贵的加密和解密开销。 但对于某些客户而言，与后端服务器的未加密通信不是可以接受的选项。 此通信未加密，可能是由于有安全要求、符合性要求，或应用程序可能仅接受安全连接。 对于此类应用程序，应用程序网关支持端到端 SSL 加密。

使用端到端 SSL 可以安全地将敏感数据以加密方式传输到后端，同时仍可利用应用程序网关提供的第 7 层负载均衡功能的优点。 部分功能包括：基于 Cookie 的会话相关性、基于 URL 的路由、基于站点的路由支持，或注入 X-Forwarded-* 标头。

如果配置为端到端 SSL 通信模式，应用程序网关会在网关上终止 SSL 会话，并解密用户流量。 然后，它会应用配置的规则，以选择要将流量路由到的适当后端池实例。 应用程序网关接下来会初始化到后端服务器的新 SSL 连接，并先使用后端服务器的公钥证书重新加密数据，此后再将请求传输到后端。 要启用端到端 SSL，请将 BackendHTTPSetting 中的协议设置设为 HTTPS，然后再将其应用到后端池。 后端池中每个已启用端到端 SSL 的后端服务器都必须配置证书，以便能够进行安全的通信。

![端到端 ssl 方案][1]

在此示例中，使用 TLS1.2 的请求通过端到端 SSL 路由到池 1 中的后端服务器。

## <a name="end-to-end-ssl-and-whitelisting-of-certificates"></a>端到端 SSL 和证书允许列表

应用程序网关只会与已知的后端实例通信，这些实例已将其证书加入应用程序网关的允许列表。 要启用证书允许列表，必须将后端服务器证书（不是根证书）的公钥上传到应用程序网关。 只允许连接到已知的和列入允许列表的后端。 其余后端会导致网关错误。 自签名证书仅用于测试目的，不建议用于生产工作负荷。 如前面的步骤中所述，此类证书必须加入应用程序网关的允许列表，才可以使用。

> [!NOTE]
> Azure 应用服务等受信任的 Azure 服务不需要身份验证证书设置。

## <a name="next-steps"></a>后续步骤

了解端到端 SSL 后，请转到[使用应用程序网关和 PowerShell 配置端到端 SSL](application-gateway-end-to-end-ssl-powershell.md)，以使用端到端 SSL 创建应用程序网关。

<!--Image references-->

[1]: ./media/ssl-overview/scenario.png

<!-- Update_Description: wording update -->