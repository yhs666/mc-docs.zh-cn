---
title: 排查 Azure 应用程序网关与应用服务的问题 - 重定向到应用服务的 URL
description: 本文介绍如何排查将 Azure 应用程序网关与 Azure 应用服务配合使用时出现的重定向问题
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
origin.date: 07/19/2019
ms.date: 09/03/2019
ms.author: v-junlch
ms.openlocfilehash: 3c6d9b44b8880dbe475dd6afc255339cdebe0e89
ms.sourcegitcommit: 7fcf656522eec95d41e699cb257f41c003341f64
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2019
ms.locfileid: "70310850"
---
# <a name="troubleshoot-app-service-issues-in-application-gateway"></a>排查应用程序网关中的应用服务问题

了解如何诊断和解决在将应用服务器用作应用程序网关的后端目标时遇到的问题

## <a name="overview"></a>概述

本文介绍如何排查以下问题：

> [!div class="checklist"]
> * 有重定向时，应用服务的 URL 公开在浏览器中
> * 应用服务的 ARRAffinity Cookie 域设置为应用服务主机名 (example.chinacloudsites.cn) 而不是原始主机

当后端应用程序发送重定向响应时，你可能希望将客户端重定向到不同的 URL，而不是后端应用程序指定的 URL。 例如，当应用服务托管在应用程序网关后面，并要求客户端重定向到其相对路径时，你可能希望这样做。 （例如，从 contoso.chinacloudsites.cn/path1 重定向到 contoso.chinacloudsites.cn/path2。）当应用服务发送重定向响应时，它会在其响应的位置标头中，使用它从应用程序网关收到的请求中的相同主机名。 因此，客户端将直接向 contoso.chinacloudsites.cn/path2 发出请求，而不是通过应用程序网关 (contoso.com/path2) 发出请求。 不应该绕过应用程序网关。

此问题的主要可能原因如下：

- 在应用服务中配置了重定向。 只需在请求中添加一个尾随的斜杠即可配置重定向。
- Azure AD 身份验证导致重定向。

此外，在应用程序网关后面使用应用服务时，与应用程序网关关联的域名 (example.com) 不同于应用服务的域名（例如 example.chinacloudsites.cn），因此，你会发现，应用服务设置的 ARRAffinity Cookie 的域值包含了不符合需要的“example.chinacloudsites.cn”域名。 原始主机名 (example.com) 应是 Cookie 中的域名值。

## <a name="sample-configuration"></a>示例配置

- HTTP 侦听器：“基本”或“多站点”
- 后端地址池：应用服务
- HTTP 设置：已启用“从后端地址中选取主机名”
- 探测：已启用“从 HTTP 设置中选取主机名”

## <a name="cause"></a>原因

由于应用服务是多租户服务，因此它会使用请求中的主机标头将请求路由到正确的终结点。 但是，应用服务的默认域名 *.chinacloudsites.cn（例如 contoso.chinacloudsites.cn）不同于应用程序网关的域名（例如 contoso.com）。 由于来自客户端的原始请求将应用程序网关的域名 (contoso.com) 用作主机名，因此需要配置应用程序网关，使其在将请求路由到应用服务后端时，将原始请求中的主机名更改为应用服务的主机名。  为此，可以在应用程序网关的 HTTP 设置配置中使用“从后端地址中选取主机名”开关，并在运行状况探测配置中使用“从后端 HTTP 设置中选取主机名”开关。



![appservice-1](./media/troubleshoot-app-service-redirection-app-service-url/appservice-1.png)

出于此原因，当应用服务执行重定向时，除非另有配置，否则，它会在 Location 标头中使用替代的主机名“contoso.chinacloudsites.cn”，而不使用原始主机名“contoso.com”。 可以查看下面的示例请求和响应标头。
```
## Request headers to Application Gateway:

Request URL: http://www.contoso.com/path

Request Method: GET

Host: www.contoso.com

## Response headers:

Status Code: 301 Moved Permanently

Location: http://contoso.chinacloudsites.cn/path/

Server: Microsoft-IIS/10.0

Set-Cookie: ARRAffinity=b5b1b14066f35b3e4533a1974cacfbbd969bf1960b6518aa2c2e2619700e4010;Path=/;HttpOnly;Domain=contoso.chinacloudsites.cn

X-Powered-By: ASP.NET
```
在上面的示例中可以发现，响应标头包含重定向状态代码 301，location 标头包含应用服务的主机名而不是原始主机名“www.contoso.com”。

## <a name="solution-use-app-services-custom-domain-instead-of-default-domain-name"></a>解决方案：使用应用服务的自定义域而不是默认域名

若要解决重定向问题，还需要将应用程序网关接收的同一主机名传递给应用服务，而不要执行主机替代。

然后，应用服务会在指向应用程序网关而不是指向自身的同一原始主机标头中执行重定向（如果有）。

若要实现此目的，必须拥有一个自定义域并遵循下面所述的过程。

- 将该域注册到应用服务的自定义域列表。 为此，必须在自定义域中创建一个指向应用服务 FQDN 的 CNAME。 有关详细信息，请参阅[将现有的自定义 DNS 名称映射到 Azure 应用服务](/app-service/app-service-web-tutorial-custom-domain)。

    ![appservice-2](./media/troubleshoot-app-service-redirection-app-service-url/appservice-2.png)

- 这样做后，应用服务已准备好接受主机名“www.contoso.com”。 现在，请更改 DNS 中的 CNAME 条目，使其重新指向应用程序网关的 FQDN。 例如“appgw.chinanorth.chinacloudapp.cn”。

- 确保执行 DNS 查询时，域“www.contoso.com”解析为应用程序网关的 FQDN。

- 设置自定义探测以禁用“从后端 HTTP 设置中选取主机名”。 为此，可以在门户上取消选中探测设置中的相应复选框，或者在 PowerShell 中，不要在 Set-AzApplicationGatewayProbeConfig 命令中使用 -PickHostNameFromBackendHttpSettings 开关。 在探测的主机名字段中，输入应用服务的 FQDN“example.chinacloudsites.cn”，因为应用程序网关发出的探测请求会在主机标头中携带此 FQDN。

  > [!NOTE]
  > 执行下一步时，请确保自定义探测未关联到后端 HTTP 设置，因为此时 HTTP 设置中仍然启用了“从后端地址中选取主机名”开关。

- 设置应用程序网关的 HTTP 设置以禁用“从后端地址中选取主机名”。 为此，可以在门户上取消选中相应的复选框，或者在 PowerShell 中，不要在 Set-AzApplicationGatewayBackendHttpSettings 命令中使用 -PickHostNameFromBackendAddress 开关。

- 将自定义探测重新关联到后端 HTTP 设置，并验证后端的运行状况是否正常。

- 这样做后，应用程序网关应会将相同的主机名“www.contoso.com”转发到应用服务，并且同一个主机名上会发生重定向。 可以查看下面的示例请求和响应标头。

若要针对现有设置使用 PowerShell 来实施上述步骤，请按下面的示例 PowerShell 脚本操作。 请注意，我们没有在探测和 HTTP 设置配置中使用 -PickHostname 开关。

```azurepowershell
$gw=Get-AzApplicationGateway -Name AppGw1 -ResourceGroupName AppGwRG
Set-AzApplicationGatewayProbeConfig -ApplicationGateway $gw -Name AppServiceProbe -Protocol Http -HostName "example.chinacloudsites.cn" -Path "/" -Interval 30 -Timeout 30 -UnhealthyThreshold 3
$probe=Get-AzApplicationGatewayProbeConfig -Name AppServiceProbe -ApplicationGateway $gw
Set-AzApplicationGatewayBackendHttpSettings -Name appgwhttpsettings -ApplicationGateway $gw -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 30
Set-AzApplicationGateway -ApplicationGateway $gw
```
  ```
  ## Request headers to Application Gateway:

  Request URL: http://www.contoso.com/path

  Request Method: GET

  Host: www.contoso.com

  ## Response headers:

  Status Code: 301 Moved Permanently

  Location: http://www.contoso.com/path/

  Server: Microsoft-IIS/10.0

  Set-Cookie: ARRAffinity=b5b1b14066f35b3e4533a1974cacfbbd969bf1960b6518aa2c2e2619700e4010;Path=/;HttpOnly;Domain=www.contoso.com

  X-Powered-By: ASP.NET
  ```
  ## <a name="next-steps"></a>后续步骤

如果上述步骤无法解决问题，请开具[支持票证](https://www.azure.cn/support/contact/)。

<!-- Update_Description: wording update -->