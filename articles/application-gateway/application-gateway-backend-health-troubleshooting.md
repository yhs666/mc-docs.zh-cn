---
title: 排查 Azure 应用程序网关中的后端运行状况问题
description: 本文引导你排查 Azure 应用程序网关的后端运行状况问题
services: application-gateway
author: surajmb
ms.service: application-gateway
ms.topic: article
origin.date: 08/30/2019
ms.date: 09/18/2019
ms.author: v-junlch
ms.openlocfilehash: cdf2a2b11eaa3d69c0e8eed11bf4d6f7ddc43f34
ms.sourcegitcommit: b47a38443d77d11fa5c100d5b13b27ae349709de
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/18/2019
ms.locfileid: "71083287"
---
<a name="troubleshoot-backend-health-issues-in-application-gateway"></a>排查应用程序网关中的后端运行状况问题
==================================================

<a name="overview"></a>概述
--------

默认情况下，应用程序网关会探测后端服务器，以检查其运行状态，以及它们是否已准备好为请求提供服务。 用户可以创建自定义探测，并指定主机名、要探测的路径，以及表示正常的状态代码。 在这两种情况下，如果后端服务器未成功响应，则应用程序网关会将该服务器标记为“不正常”，并停止向其转发请求。 服务器成功开始响应后，它将继续为请求提供服务。

### <a name="how-to-check-backend-health"></a>如何检查后端运行状况

若要检查后端池的运行状况，可以使用 Azure 门户中的“后端运行状况”页。 或者，可以使用 [Azure PowerShell](https://docs.microsoft.com/powershell/module/az.network/get-azapplicationgatewaybackendhealth?view=azps-2.6.0)、[CLI](/cli/network/application-gateway?view=azure-cli-latest#az-network-application-gateway-show-backend-health) 或 [REST API](https://docs.microsoft.com/rest/api/application-gateway/applicationgateways/backendhealth) 来获取运行状态。 请查看有关每种方法的链接文章来了解详细信息。

所述任意方法检索到的状态可为以下三种类型：

1.  Healthy

2.  不正常

3.  Unknown

如果服务器的后端运行状态为“正常”，则表示应用程序网关会将请求转发到该服务器。 但是，如果后端池中所有服务器的后端运行状况不正常或未知，则在应用程序访问期间，你可能会遇到一些问题。 本文档将描述当后端服务器不正常或未知时显示的每个错误的症状、原因和解决方法。

<a name="backend-health-status-unhealthy"></a>后端运行状态为“不正常”
-------------------------------

如果后端运行状态显示为“不正常”，你会在门户中看到该状态，如以下屏幕截图所示：

![应用程序网关后端运行状况 - 不正常](./media/application-gateway-backend-health-troubleshooting/appgwunhealthy.png)

或者，如果你使用 Azure PowerShell、CLI 或 Azure REST API 查询，则会看到如下所示的响应：
```azurepowershell
PS C:\Users\testuser\> Get-AzApplicationGatewayBackendHealth -Name "appgw1" -ResourceGroupName "rgOne"
BackendAddressPools :
{Microsoft.Azure.Commands.Network.Models.PSApplicationGatewayBackendHealthPool}
BackendAddressPoolsText : [
{
                              "BackendAddressPool": {
                                "Id": "/subscriptions/536d30b8-665b-40fc-bd7e-68c65f816365/resourceGroups/rgOne/providers/Microsoft.Network/applicationGateways/appgw1/b
                          ackendAddressPools/appGatewayBackendPool"
                              },
                              "BackendHttpSettingsCollection": [
                                {
                                  "BackendHttpSettings": {
                                    "TrustedRootCertificates": [],
                                    "Id": "/subscriptions/536d30b8-665b-40fc-bd7e-68c65f816365/resourceGroups/rgOne/providers/Microsoft.Network/applicationGateways/appg
                          w1/backendHttpSettingsCollection/appGatewayBackendHttpSettings"
                                  },
                                  "Servers": [
                                    {
                                      "Address": "10.0.0.5",
                                      "Health": "Healthy"
                                    },
                                    {
                                      "Address": "10.0.0.6",
                                      "Health": "Unhealthy"
                                    }
                                  ]
                                }
                              ]
                            }
                        ]
```
一旦后端池中所有服务器的后端服务器状态都为“不正常”，请求将不会转发到这些服务器，并且应用程序网关会向请求方客户端返回“502 错误的网关”错误。 若要排查此问题，请检查“后端运行状况”选项卡的“详细信息”列。

“后端运行状况”选项卡的“详细信息”列中显示的消息提供有关问题的更详细见解，根据这些信息，我们可以着手排查问题。

> [!NOTE]
> 默认探测请求以 <protocol>://127.0.0.1:<port>/ 格式发送（例如，对于端口 80 上的 HTTP 探测，格式为 <http://127.0.0.1/>），并只将 HTTP 状态代码 200-399 的响应视为正常响应。 协议和目标端口继承自 HTTP 设置。 如果你希望应用程序网关探测不同的协议、主机名或路径，并接受其他状态代码为正常，请配置一个自定义探测，并将其关联到 HTTP 设置。

### <a name="error-messages"></a>错误消息

#### <a name="backend-server-timeout"></a>后端服务器超时

**消息：** 后端响应应用程序网关运行状况探测所花费的时间超过了探测设置中的超时阈值。

**原因：** 应用程序网关将 HTTP(S) 探测请求发送到后端服务器后，它会在配置的特定时间内等待后端服务器的响应。 如果后端服务器在超时值内未做出响应，则系统会将其标记为“不正常”，直到它再次在超时期限内开始响应。

**解决方法：** 检查后端服务器或应用程序，找出它无法在配置的超时内做出响应的原因；检查应用程序依赖项，例如，数据库是否存在可能导致响应延迟的任何问题。 如果你了解应用程序的行为，并且它应该只在超时值之后做出响应，请增大自定义探测设置中的超时值。
需要使用一个自定义探测来更改超时值。若要了解如何配置自定义探测，请[查看文档页](/application-gateway/application-gateway-create-probe-portal)。

若要增大超时，请执行以下步骤：

1.  直接访问后端服务器，并在该页上检查服务器做出响应所花费的时间。 可以使用任何工具来访问服务器，包括使用开发人员工具的浏览器。

2.  确定应用程序做出响应所需的时间后，单击“运行状况探测”选项卡，然后选择与 HTTP 设置关联的探测。

3.  输入大于应用程序响应时间的超时值（以秒为单位）。

4.  保存自定义探测设置，检查后端运行状况现在是否显示为“正常”。

#### <a name="dns-resolution-error"></a>DNS 解析错误

**消息：** 应用程序网关无法为此后端创建探测。 如果未正确输入后端的 FQDN，则往往会发生这种情况。 

**原因：** 如果后端池的类型为“IP 地址/FQDN”或“应用服务”，则应用程序网关将解析为使用 DNS 服务器（自定义或 Azure 默认）输入的 FQDN 的 IP 地址，并尝试连接到 HTTP 设置中指定的 TCP 端口上的服务器。 但如果显示此消息，则表示应用程序网关无法成功解析输入的 FQDN 的 IP 地址。

**解决方法：**

1.  验证后端池中输入的 FQDN 是否正确以及它是否为公共域，并尝试从本地计算机解析它。

2.  如果可以解析 IP 地址，则表示 VNet 中的 DNS 配置可能有错误。

3.  检查是否在 VNet 中配置了自定义 DNS 服务器。 如果是，请检查 DNS 服务器，确定它为何无法解析为给定 FQDN 的 IP 地址。

4.  如果是 Azure 默认 DNS，请与域名注册机构确认是否执行了正确的 A 记录或 CNAME 记录映射。

5.  如果域是专用/内部域，请尝试从同一 VNet 中的 VM 解析它，如果能够解析，请重启应用程序网关，然后再次检查。 若要重启应用程序网关，需要使用相关文档链接中提供的 PowerShell 命令[停止](https://docs.microsoft.com/powershell/module/azurerm.network/stop-azurermapplicationgateway?view=azurermps-6.13.0)再[启动](https://docs.microsoft.com/powershell/module/azurerm.network/start-azurermapplicationgateway?view=azurermps-6.13.0)它。

#### <a name="tcp-connect-error"></a>TCP 连接错误

**消息：** 应用程序网关无法连接到后端。
请检查后端是否在用于探测的端口上做出响应。
同时，请检查是否有任何 NSG/UDR/防火墙正在阻止访问此后端的 IP 和端口

**原因：** DNS 解析阶段完成后，应用程序网关会尝试连接到 HTTP 设置中配置的 TCP 端口上的后端服务器。 如果应用程序网关无法在指定的端口上建立 TCP 会话，则会将探测标记为不正常并显示此消息。

**解决方案：** 如果看到此错误，请进行以下检查：

1.  检查是否可以使用浏览器或 PowerShell 连接到 HTTP 设置中指定的端口上的后端服务器，例如，可使用以下命令：Test-NetConnection -ComputerName www.bing.com -Port 443

2.  如果指定的端口不是所需的端口，请输入应用程序网关的正确端口号，以连接到后端服务器

3.  如果在本地计算机的端口上也无法进行连接，请进行以下检查：

    a.  检查后端服务器的 NIC 和子网的 NSG 设置，并检查是否允许与配置的端口建立入站连接。 如果不允许，请创建新规则来允许连接。 若要了解如何创建 NSG 规则，请[查看文档页](/virtual-network/tutorial-filter-network-traffic#create-security-rules)。

    b.  如果允许出站公共和专用流量，请检查应用程序网关子网的 NSG 设置，以便可以建立连接。 查看 (a) 中链接的文档来详细了解如何创建 NSG 规则。
    ```azurepowershell
            $vnet = Get-AzVirtualNetwork -Name "vnetName" -ResourceGroupName "rgName"
            Get-AzVirtualNetworkSubnetConfig -Name appGwSubnet -VirtualNetwork $vnet
    ```

    c.  检查应用程序网关和后端服务器子网的 UDR 设置中是否存在任何路由异常。 确保 UDR 未将流量引离后端子网。 例如，检查到网络虚拟设备的路由或通过 ExpressRoute/VPN 播发到应用程序网关子网的默认路由。

    d.  若要检查 NIC 的有效路由和规则，可使用以下 PowerShell 命令。
    ```azurepowershell
            Get-AzEffectiveNetworkSecurityGroup -NetworkInterfaceName "nic1" -ResourceGroupName "testrg"
            Get-AzEffectiveRouteTable -NetworkInterfaceName "nic1" -ResourceGroupName "testrg"
    ```
4.  如果找不到 NSG 或 UDR 的任何问题，请检查后端服务器上是否存在应用程序相关的问题，从而导致客户端无法在配置的端口上建立 TCP 会话。 要检查的几个方面：

    e.  打开命令提示符 (Win+R -\> cmd)，输入 netstat 并按 Enter

    f.  检查服务器是否正在侦听配置的端口。
        例如：
    ```
            Proto Local Address Foreign Address State PID
            TCP 0.0.0.0:80 0.0.0.0:0 LISTENING 4
    ```
    g.  如果不是，请检查 Web 服务器设置，例如，IIS 中的站点绑定、NGINX 中的服务器块，以及 Apache 中的虚拟主机

    h.如果该值不存在，请单击“添加行”。  检查 OS 防火墙设置，确保允许传入的流量进入端口

#### <a name="http-status-code-mismatch"></a>HTTP 状态代码不匹配

**消息：** 后端 HTTP 响应的状态代码与探测设置不匹配。 预期值: {HTTPStatusCode0}，收到的值: {HTTPStatusCode1}.

**原因：** 建立 TCP 连接并完成 SSL 握手（如果启用了 SSL）后，应用程序网关会将探测作为 HTTP GET 请求发送到后端服务器。 如上所述，默认探测针对 \<协议\>://127.0.0.1:\<端口\>/，并将 200-399 范围内的响应状态代码视为正常。 如果服务器返回任何其他状态代码，则将服务器标记为不正常并显示此消息。

**解决方案：** 根据后端服务器的响应代码，可以执行以下步骤。 下面列出了一些常见的状态代码：

| **错误** | **操作** |
| --- | --- |
| 探测状态代码不匹配：收到 401 | 检查后端服务器是否要求身份验证。 应用程序网关探测此时无法传递用于身份验证的凭据。 允许探测状态代码匹配中出现 \"HTTP 401\"，或探测其中的服务器不需要身份验证的路径。 | |
| 探测状态代码不匹配：收到 403 | 禁止访问。 检查后端服务器上是否允许访问该路径。 | |
| 探测状态代码不匹配：收到 404 | 找不到页面。 如果可在后端服务器上访问主机名路径，请检查该路径。 将主机名或路径参数更改为可访问的值。 | |
| 探测状态代码不匹配：收到 405 | 应用程序网关的探测请求使用 HTTP GET 方法。 检查服务器是否允许该方法。 | |
| 探测状态代码不匹配：收到 500 | 内部服务器错误。 检查后端服务器的运行状况，以及服务是否正在运行。 | |
| 探测状态代码不匹配：收到 503 | 服务不可用。 检查后端服务器的运行状况，以及服务是否正在运行。 | |

或者，如果你认为响应合法，并且你希望应用程序网关接受其他状态代码为正常，可以创建自定义探测。 如果后端网站需要身份验证，并且由于探测请求不携带任何用户凭据，因此它们将会失败，而后端服务器将返回 HTTP 401 状态代码，则进行这种修改会很有作用。

若要创建自定义探测，请遵循[此处](/application-gateway/application-gateway-create-probe-portal)所列的步骤。

#### <a name="http-response-body-mismatch"></a>HTTP 响应正文不匹配

**消息：** 后端 HTTP 响应的正文与探测设置不匹配。 收到的响应正文不包含 {string}。

**原因：** 创建自定义探测时，可以选择通过匹配响应正文中的字符串将后端服务器标记为正常。 例如，可将应用程序网关配置为接受“unauthorized”作为要匹配的字符串。 如果探测请求的后端服务器响应包含字符串“unauthorized”，则会将该服务器标记为正常。
否则，会将其标记为不正常并显示此消息。

**解决方案：** 可以遵循以下步骤来解决此问题：

1.  在本地或者通过探测路径上的某台客户端计算机访问后端服务器，并检查响应正文。

2.  检查应用程序网关自定义探测配置，验证响应正文是否与配置的内容相匹配。

3.  如果不匹配，请使用可接受的正确字符串值更改探测配置。

可在[此处](/application-gateway/application-gateway-probe-overview#probe-matching)详细了解应用程序网关探测匹配。

#### <a name="backend-server-certificate-invalid-ca"></a>后端服务器证书的 CA 无效

**消息：** 后端使用的服务器证书未由已知的证书颁发机构 (CA) 签名。 请上传后端使用的服务器证书的根证书，将应用程序网关上的后端列入白名单。

**原因：** 在将服务器视为正常之前，应用程序网关 v2 的端到端 SSL 要求验证后端服务器的证书。
要信任某个 SSL 证书，后端服务器的证书必须由应用程序网关受信任存储中包含的 CA 颁发。 如果该证书不是由受信任的 CA 颁发（例如自签名证书），则用户应将颁发者的证书上传到应用程序网关。

**解决方案：** 遵循以下步骤导出受信任的根证书并将其上传到应用程序网关（下面提供的步骤适用于 Windows 客户端）

1.  登录到托管应用程序的计算机

2.  按 Win+R，或右键单击“开始”按钮并选择“运行”打开“运行”

3.  输入 certmgr.msc 并按 Enter。 也可以在“开始”菜单中搜索“证书管理器”。

4.  找到该证书（通常位于“证书 - 当前用户”>“个人”>“证书”中），并将其打开\'\\\\\'

5.  在“证书属性”中选择“证书路径”选项卡

6.  选择根证书，然后选择“查看证书”选项

7.  在“证书属性”中，切换到“详细信息”选项卡

8.  在“详细信息”选项卡中，选择“复制到文件”选项，并以 Base-64 编码的 X.509 (.CER) 格式保存文件

9.  保存后，在 Azure 门户中打开“应用程序网关 HTTP 设置”页

10. 打开“HTTP 设置”，单击“添加证书”并找到刚刚保存的证书文件

11. 单击“保存”以保存 HTTP 设置

或者，可以从客户端计算机导出根证书，方法是使用浏览器直接访问服务器（绕过应用程序网关），然后从浏览器导出根证书。

有关在应用程序网关中提取和上传受信任的根证书的详细步骤，请参阅[此文](/application-gateway/certificates-for-backend-authentication#export-trusted-root-certificate-for-v2-sku)。

#### <a name="trusted-root-certificate-mismatch"></a>受信任的根证书不匹配

**消息：** 后端使用的服务器证书的根证书与添加到应用程序网关的受信任根证书不匹配。 请确保添加正确的根证书，以将后端列入白名单

**原因：** 在将服务器视为正常之前，应用程序网关 v2 的端到端 SSL 要求验证后端服务器的证书。
要信任某个 SSL 证书，后端服务器的证书必须由应用程序网关受信任存储中包含的 CA 颁发。 如果该证书不是由受信任的 CA 颁发（例如自签名证书），则用户应将颁发者的证书上传到应用程序网关。

已上传到应用程序网关 HTTP 设置的证书必须与后端服务器证书的根证书相匹配。

**解决方案：** 如果看到上述错误消息，则表示已上传到应用程序网关的证书与上传到后端服务器的证书不匹配。

遵循上述步骤 1-11 将正确的受信任根证书上传到应用程序网关。

有关在应用程序网关中提取和上传受信任的根证书的详细步骤，请参阅[此文](/application-gateway/certificates-for-backend-authentication#export-trusted-root-certificate-for-v2-sku)。
> [!NOTE]
> 如果后端服务器在 TLS 握手期间未交换完整的证书链（包括“根”->“中间”（如果适用）->“叶”），则也可能会出现上述错误。 若要验证，可在任何客户端中使用 OpenSSL 命令，并使用应用程序网关探测中配置的设置连接到后端服务器。

例如，
```
OpenSSL> s_client -connect 10.0.0.4:443 -servername www.example.com -showcerts
```
如果输出未显示要返回的完整证书链，请使用完整链（包括根证书）再次导出证书，并在后端服务器中配置该证书。 

CONNECTED(00000188)\
depth=0 OU = Domain Control Validated, CN = \*.example.com\
verify error:num=20:unable to get local issuer certificate\
verify return:1\
depth=0 OU = Domain Control Validated, CN = \*.example.com\
verify error:num=21:unable to verify the first certificate\
verify return:1\
\-\-\-\
Certificate chain\
 0 s:/OU=Domain Control Validated/CN=*.example.com\
   i:/C=US/ST=Arizona/L=Scottsdale/O=GoDaddy.com, Inc./OU=http://certs.godaddy.com/repository//CN=Go Daddy Secure Certificate Authority - G2\
\-----BEGIN CERTIFICATE-----\
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx\
\-----END CERTIFICATE-----

#### <a name="backend-certificate-invalid-common-name-cn"></a>后端证书的公用名 (CN) 无效

**消息：** 后端证书的公用名 (CN) 与探测的主机标头不匹配。

**原因：** 应用程序网关将会验证后端 HTTP 设置中指定的主机名是否与后端服务器 SSL 证书提供的公用名 (CN) 的主机名相匹配。 这只是一种 Standard_v2 和 WAF_v2 SKU 行为。 Standard 和 WAF SKU 的 SNI 设置为后端池地址中的 FQDN。

在 v2 SKU 中，如果存在默认探测（未配置/关联自定义探测），或者 HTTP 设置中指定了“从后端地址中选取主机名”，并且后端地址池包含有效的 FQDN，则会根据 HTTP 设置中指定的主机名设置 SNI。

如果某个自定义探测已关联到 HTTP 设置，则会根据自定义探测配置中指定的主机名设置 SNI；如果在自定义探测中选中了“从后端 HTTP 设置中选取主机名”，则会根据 HTTP 设置中指定的主机名设置 SNI。

如果在 HTTP 设置中设置了“从后端地址中选取主机名”，则后端地址池必须包含有效的 FQDN。

如果看到上述错误消息，表示后端证书的公用名 (CN) 与自定义探测或 HTTP 设置中配置的主机名不匹配（如果选择了“从后端 HTTP 设置中选取主机名”）。 如果使用默认探测，主机名将设置为“127.0.0.1”。 如果这不是所需的值，应创建一个自定义探测，并将其关联到 HTTP 设置。

**解决方案：**

若要解决该问题，请执行以下步骤：

对于 Windows：

1.  登录到托管应用程序的计算机

2.  按 Win+R，或右键单击“开始”按钮并选择“运行”打开“运行”

3.  输入 certmgr.msc 并按 Enter。 也可以在“开始”菜单中搜索“证书管理器”。

4.  找到该证书（通常位于“证书 - 当前用户”>“个人”>“证书”中），并将其打开\'\\\\\'

5.  在“详细信息”选项卡中检查证书的使用者

6.  验证详细信息中的证书的 CN，并在自定义探测或 HTTP 设置的主机名字段中输入相同的 CN（如果选择了“从后端 HTTP 设置中选取主机名”）。 如果这不是网站所需的主机名，则必须获取该域的证书，或者在自定义探测/HTTP 设置配置中输入正确的主机名。

对于使用 OpenSSL 的 Linux

1.  在 OpenSSL 中运行此命令 
```
openssl x509 -in certificate.crt -text -noout
```

2.  在显示的属性中找到证书的 CN，并在 HTTP 设置的主机名字段中输入相同的 CN。 如果这不是网站所需的主机名，则必须获取该域的证书，或者在自定义探测/HTTP 设置配置中输入正确的主机名。

#### <a name="backend-certificate-is-invalid"></a>后端证书无效

**消息：** 后端证书无效。 当前日期不在证书上的“生效”和“失效”日期范围内。\"\"\"\"

**原因：** 每个证书存在有效性，除非服务器的 SSL 证书有效，否则 HTTPS 连接不安全。
当前日期必须在生效和失效日期范围内。 否则，证书将被视为无效，并成为一个安全隐患。 此时，应用程序网关会将后端服务器标记为不正常。

**解决方案：** 如果 SSL 证书已过期，请向供应商续订证书，并使用新证书更新服务器设置。 如果它是自签名证书，则你需要生成有效的证书，并将根证书上传到应用程序网关的 HTTP 设置。 为此，请执行以下步骤：

1.  在门户中打开应用程序网关 HTTP 设置

2.  选择包含已过期证书的 HTTP 设置，选择“添加证书”并打开新证书文件

3.  使用证书旁边的“删除”图标删除旧证书，然后单击“保存”

#### <a name="certificate-verification-failed"></a>证书验证失败

**消息：** 无法验证后端证书的有效性。 若要找出原因，请在 OpenSSL 诊断信息中检查与错误代码 {errorCode} 相关的消息

**原因：** 当应用程序网关无法验证证书的有效性时，将会发生此错误。

**解决方案：** 若要解决此问题，请检查服务器上的证书（如果已正确创建）。 例如，可以使用 [OpenSSL](https://www.openssl.org/docs/man1.0.2/man1/verify.html) 来验证证书及其属性，并尝试将证书重新上传到应用程序网关的 HTTP 设置。

<a name="backend-health-status-unknown"></a>后端运行状态未知
-------------------------------
如果后端运行状况显示为“未知”，此状态将显示在门户中，如以下屏幕截图所示：

![应用程序网关后端运行状况 - 未知](./media/application-gateway-backend-health-troubleshooting/appgwunknown.png)

以下一种或多种原因可能会导致后端运行状况显示为“未知”：

1.  应用程序网关子网中的 NSG 正在阻止从“Internet”对端口 65503-65534 (v1 SKU) 或 65200-65535 (v2 SKU) 进行入站访问
2.  使用默认路由 (0.0.0.0/0) 的应用程序网关子网中的 UDR 的下一跃点不是 Internet
3.  ExpressRoute/VPN 播发的默认路由通过 BGP 连接到 VNet
4.  在 VNet 中配置的自定义 DNS 服务器无法解析公共域名
5.  应用程序网关处于不正常状态

**解决方案：**

1.  检查 NSG 是否正在阻止从“Internet”对端口 65503-65534 (v1 SKU) 或 65200-65535 (v2 SKU) 进行访问

    a.  在应用程序网关“概述”选项卡中，选择“虚拟网络/子网”链接

    b.  在虚拟网络的“子网”选项卡中，选择已将应用程序网关部署到的子网

    c.  检查是否配置了任何 NSG

    d.  如果已配置，请在“搜索”选项卡中或者在“所有资源”下搜索该 NSG 资源

    e.  在“入站规则”部分添加一个入站规则，以允许目标端口范围 65503-65534 (v1 SKU) 或 65200-65535 (v2 SKU)，并允许使用“任何”或“Internet”作为源

    f.  单击“保存”，并验证是否可以成功查看后端的正常状态

    g.  或者，可以通过 [PowerShell/CLI](/virtual-network/manage-network-security-group) 执行此操作

2.  检查 UDR 是否包含下一跃点不为“Internet”的默认路由 (0.0.0.0/0)
    
    a.  执行步骤 1.a 和 1.b 以确定子网

    b.  检查是否配置了任何 UDR。 如果已配置，请在搜索栏中或者在“所有资源”下搜索该资源

    c.  检查是否存在下一跃点不为“Internet”的任何默认路由 (0.0.0.0/0)。 如果它是虚拟设备或虚拟网络网关，则你需要确保虚拟设备或本地设备能够正确地将数据包路由回到 Internet 目标，且无需修改数据包

    d.  否则，请将下一跃点更改为“Internet”，单击“保存”，并验证后端运行状况

3.  ExpressRoute/VPN 播发的默认路由通过 BGP 连接到 VNet

    a.  如果 ExpressRoute/VPN 通过 BGP 连接到 VNet，并且你正在播发默认路由，则需要确保数据包在不修改的情况下可路由回到 Internet 目标

    b.  可以使用应用程序网关门户中的“连接故障排除”选项进行验证

    c.  手动选择任何可通过 Internet 路由的 IP 地址（例如“1.1.1.1”）作为目标，并选择任何端口作为目标端口，然后检查连接。

    d.  如果下一跃点是“虚拟网络网关”，则可能存在通过 ExpressRoute 或 VPN 播发的默认路由

4.  如果在 VNet 中配置了自定义 DNS 服务器，请验证服务器是否可以解析公共域。 在应用程序网关必须连接到外部域（例如 OCSP 服务器）或检查证书的吊销状态等情况下，可能需要进行公共域名解析。

5.  若要验证应用程序网关是否正常且正在运行，请在门户中转到“资源运行状况”选项，并验证其状态是否为“正常”。 如果看到“不正常”或“降级”状态，请[联系支持人员](https://www.azure.cn/support/contact/)。

<a name="next-steps"></a>后续步骤
----------

若要详细了解应用程序网关诊断和日志记录，请参阅[此文](/application-gateway/application-gateway-diagnostics)。

