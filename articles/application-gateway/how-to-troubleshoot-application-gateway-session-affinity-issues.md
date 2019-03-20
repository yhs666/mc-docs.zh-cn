---
title: 排查 Azure 应用程序网关会话相关性问题
description: 本文介绍如何排查 Azure 应用程序网关中的会话相关性问题
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
origin.date: 02/22/2019
ms.date: 03/11/2019
ms.author: v-junlch
ms.openlocfilehash: 3be5767cc57074efef6b87c2a20eb7fd63691d20
ms.sourcegitcommit: d750a61a0e52a41cff5607149e33b6be189075d4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/12/2019
ms.locfileid: "57788765"
---
# <a name="troubleshoot-azure-application-gateway-session-affinity-issues"></a>排查 Azure 应用程序网关会话相关性问题

了解如何诊断和解决 Azure 应用程序网关的会话相关性问题。

## <a name="overview"></a>概述

需要在同一服务器上保留用户会话时，可以使用基于 Cookie 的会话相关性功能。 借助网关托管的 Cookie，应用程序网关可以将来自用户会话的后续流量定向到同一服务器进行处理。 在用户会话的会话状态在服务器上进行本地保存的情况下，此功能十分重要。

## <a name="possible-problem-causes"></a>问题的可能原因

基于 Cookie 的会话相关性问题的主要可能原因如下：

- 未启用“基于 Cookie 的相关性”设置
- 应用程序无法处理基于 Cookie 的相关性
- 应用程序使用基于 Cookie 的相关性，但请求仍在后端服务器之间弹跳

### <a name="check-whether-the-cookie-based-affinity-setting-is-enabled"></a>检查是否启用了“基于 Cookie 的相关性”设置

如果你忘记了启用“基于 Cookie 的相关性”设置，则有时会出现会话相关性问题。 若要确定是否在 Azure 门户中的“HTTP 设置”选项卡上启用了“基于 Cookie 的相关性”设置，请遵照以下说明操作：

1. 登录到 [Azure 门户](https://portal.azure.cn/)。

2. 在**左侧导航**窗格中，单击“所有资源”。 在“所有资源”边栏选项卡中单击应用程序网关名称。 如果所选订阅中已包含多个资源，可在“按名称筛选…”中输入应用程序网关名称。 轻松访问应用程序网关。

3. 选择“设置”下的“HTTP 设置”选项卡。

   ![troubleshoot-session-affinity-issues-1](.\media\how-to-troubleshoot-application-gateway-session-affinity-issues\troubleshoot-session-affinity-issues-1.png)

4. 单击右侧的“appGatewayBackendHttpSettings”，检查是否为“基于 Cookie 的相关性”选择了“已启用”。

   ![troubleshoot-session-affinity-issues-2](.\media\how-to-troubleshoot-application-gateway-session-affinity-issues\troubleshoot-session-affinity-issues-2.jpg)



也可以使用以下方法之一，检查“backendHttpSettingsCollection”下的“CookieBasedAffinity”值是否设置为“Enabled”：

- 在 PowerShell 中运行 [Get-AzureRmApplicationGatewayBackendHttpSettings](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermapplicationgatewaybackendhttpsettings?view=azurermps-4.1.0)
- 使用 Azure 资源管理器模板通查 JSON 文件

```
"cookieBasedAffinity": "Enabled", 
```

### <a name="the-application-cannot-handle-cookie-based-affinity"></a>应用程序无法处理基于 Cookie 的相关性

#### <a name="cause"></a>原因

应用程序网关只能使用 Cookie 执行基于会话的相关性。

#### <a name="workaround"></a>解决方法

如果应用程序无法处理基于 Cookie 的相关性，则必须使用外部或内部 Azure 负载均衡器或其他第三方解决方案。

### <a name="application-is-using-cookie-based-affinity-but-requests-still-bouncing-between-back-end-servers"></a>应用程序使用基于 Cookie 的相关性，但请求仍在后端服务器之间弹跳

#### <a name="symptom"></a>症状

你已启用“基于 Cookie 的相关性”设置，但在 Internet Explorer 中使用短名 URL（例如 [http://website](http://website/)）访问应用程序网关时，请求仍在后端服务器之间弹跳。

若要识别此问题，请遵照以下说明操作：

1. 在连接到应用程序网关后面的应用程序的“客户端”上提取 Web 调试器跟踪（本示例使用 Fiddler）。
    **提示**如果你不知道如何使用 Fiddler，请选中底部的“我想要收集网络流量并使用 Web 调试器分析它”选项。

2. 检查并分析会话日志，确定客户端提供的 Cookie 是否包含 ARRAffinity 详细信息。 如果在 Cookie 集中找不到类似于 "**ARRAffinity=** *ARRAffinityValue*" 的 ARRAffinity 详细信息，则表示客户端未使用应用程序网关提供的 ARRA Cookie 做出回复。
    例如：

    ![troubleshoot-session-affinity-issues-3](.\media\how-to-troubleshoot-application-gateway-session-affinity-issues\troubleshoot-session-affinity-issues-3.png)

        ![troubleshoot-session-affinity-issues-4](.\media\how-to-troubleshoot-application-gateway-session-affinity-issues\troubleshoot-session-affinity-issues-4.png)

应用程序会持续尝试针对每个请求设置 Cookie，直到收到回复。

#### <a name="cause"></a>原因

此问题的可能原因是 Internet Explorer 和其他浏览器无法存储或使用包含短名 URL 的 Cookie。

#### <a name="resolution"></a>解决方法

若要解决此问题，应使用 FQDN 访问应用程序网关。 例如，使用 [http://website.com](http://website.com/) 或 [http://appgw.website.com](http://appgw.website.com/)。


## <a name="next-steps"></a>后续步骤

如果上述步骤无法解决问题，请开具[支持票证](https://www.azure.cn/support/contact/)。

