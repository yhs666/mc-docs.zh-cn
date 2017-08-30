---
title: "本地数据网关 | Azure"
description: "如果 Azure 中的 Analysis Services 服务器要连接到本地数据源，则本地网关是必需的。"
services: analysis-services
documentationcenter: 
author: rockboyfor
manager: digimobile
editor: 
tags: 
ms.assetid: cd596155-b608-4a34-935e-e45c95d884a9
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
origin.date: 08/21/2017
ms.date: 08/28/2017
ms.author: v-yeche
ms.openlocfilehash: 0f642ed4d2d1efcbbb1bcc01ce834965014ea589
ms.sourcegitcommit: 1ca439ddc22cb4d67e900e3f1757471b3878ca43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/25/2017
---
# <a name="connecting-to-on-premises-data-sources-with-azure-on-premises-data-gateway"></a>使用 Azure 本地数据网关连接到本地数据源
本地数据网关的作用好似一架桥，提供本地数据源与云中的 Azure Analysis Services 服务器之间的安全数据传输。 除了适用于同一区域中的多个 Azure Analysis Services 服务器以外，最新版本的网关还适用于 Azure 逻辑应用、Power BI、Power 应用和 Microsoft Flow。 可将同一区域中的多个服务与单个网关相关联。 

 Azure Analysis Services 要求同一区域中具有相应的网关资源。 例如，如果中国东部区域有 Azure Analysis Services 服务器，则中国东部区域需有一个网关资源。 中国东部的多个服务器可以使用同一个网关。

首次安装网关的过程由四个部分组成：

- **下载并运行安装程序** - 此步骤在组织中的计算机上安装网关服务。

- **注册网关** - 在此步骤中指定网关的名称和恢复密钥，选择区域，并将网关注册到网关云服务。

- **在 Azure 中创建网关资源** - 此步骤在 Azure 订阅中创建网关资源。

- **将服务器连接到网关资源** - 在订阅中创建网关资源后，可以开始将服务器连接到该资源。

为订阅配置网关资源后，可将多个服务器和其他服务连接到该资源。 如果在不同的区域拥有服务器或其他服务，只需安装不同的网关并创建附加的网关资源。

若要立即开始，请参阅[安装并配置本地数据网关](analysis-services-gateway-install.md)。

## <a name="how-it-works"></a>工作原理
在组织中的计算机上安装的网关以 Windows 服务（**本地数据网关**）的形式运行。 此本地服务已通过 Azure 服务总线注册到网关云服务。 然后，可为 Azure 订阅创建网关资源网关云服务。 Azure Analysis Services 服务器随后会连接到网关资源。 如果服务器上的模型需要连接到本地数据源以执行查询或处理，查询和数据流会遍历网关资源、Azure 服务总线、本地数据网关服务和数据源。 

![工作原理](./media/analysis-services-gateway/aas-gateway-how-it-works.png)

查询和数据流：

1. 查询是云服务使用本地数据源的加密凭据创建的。 查询随后会发送到队列让网关处理。
2. 网关云服务分析该查询，并将请求推送到 [Azure 服务总线](/service-bus/)。
3. 本地数据网关会针对挂起的请求轮询 Azure 服务总线。
4. 网关获取查询，对凭据进行解密，并使用这些凭据连接到数据源。
5. 网关将查询发送到数据源以便执行。
6. 结果会从数据源返回到网关，并返回到云服务和服务器。

## <a name="windows-service-account"></a>Windows 服务帐户
本地数据网关配置为将 *NT SERVICE\PBIEgwService* 用于 Windows 服务登录凭据。 默认情况下，它有权作为服务登录；这位于正在安装网关的计算机的上下文中。 此凭据与用于连接到本地数据源或 Azure 帐户的帐户不相同。  

如果由于身份验证，代理服务器遇到问题，建议将 Windows 服务帐户更改为域用户或托管服务帐户。

## <a name="ports"></a>端口
网关会创建与 Azure 服务总线之间的出站连接。 它在以下出站端口上进行通信：TCP 443（默认值）、5671、5672、9350 到 9354。  网关不需要入站端口。

我们建议在防火墙中将数据区域的 IP 地址列入允许列表。 可以下载 [Azure 数据中心 IP 列表](https://www.microsoft.com/download/details.aspx?id=42064)。 该列表每周都会进行更新。

> [!NOTE]
> Azure 数据中心 IP 列表中列出的 IP 地址使用的是 CIDR 表示法。 例如，10.0.0.0/24 并不是指 10.0.0.0 到 10.0.0.24。 深入了解 [CIDR 表示法](http://whatismyipaddress.com/cidr)。
>
>

以下是该网关所用的完全限定域名。

| 域名 | 出站端口 | 说明 |
| --- | --- | --- |
| *.powerbi.cn |80 |用于下载安装程序的 HTTP。 |
| *.powerbi.cn |443 |HTTPS |
| *.analysis.chinacloudapi.cn |443 |HTTPS |
| *.login.chinacloudapi.cn |443 |HTTPS |
| *.servicebus.chinacloudapi.cn |5671-5672 |高级消息队列协议 (AMQP) |
| *.servicebus.chinacloudapi.cn |443, 9350-9354 |通过 TCP 的服务总线中继上的侦听器（需要 443 来获取访问控制令牌） |
| *.frontend.clouddatahub.net |443 |HTTPS |
| *.core.chinacloudapi.cn |443 |HTTPS |
| login.chinacloudapi.cn |443 |HTTPS |
| *.msftncsi.com |443 |在 Power BI 服务无法访问网关时用于测试 Internet 连接。 |
| *.microsoftonline-p.com |443 |用于根据配置进行身份验证。 |

### <a name="forcing-https-communication-with-azure-service-bus"></a>强制与 Azure 服务总线进行 HTTPS 通信
可以强制网关使用 HTTPS 而非直接 TCP 与 Azure 服务总线进行通信，但此操作可能会显著降低性能。 若要修改 *Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config* 文件，可将值从 `AutoDetect` 更改为 `Https`。 此文件通常位于 *C:\Program Files\On-premises data gateway*。

```
<setting name="ServiceBusSystemConnectivityModeString" serializeAs="String">
    <value>Https</value>
</setting>
```

<a name="faq"></a>
## <a name="frequently-asked-questions"></a>常见问题

### <a name="general"></a>常规

**问**：对于云中的数据源（如 SQL Azure），是否需要网关？ <br/>
**答**：不需要。 网关只连接到本地数据源。

**问**：网关是否必须安装在与数据源相同的计算机上？ <br/>
**答**：不是。 网关使用提供的连接信息连接到数据源。 从这种意义上讲，可将网关视为客户端应用程序。 网关只需能够连接到提供的服务器名称即可。

<a name="why-azure-work-school-account"></a>

**问**：为何需要使用工作或学校帐户登录？ <br/>
**答**：安装本地数据网关时只能使用 Azure 工作或学校帐户。 登录帐户存储在由 Azure Active Directory (Azure AD) 管理的租户中。 通常，Azure AD 帐户的用户主体名称 (UPN) 与电子邮件地址匹配。

**问**：凭据存储在何处？ <br/>
**答**：为数据源输入的凭据会加密，并存储在网关云服务中。 凭据在本地数据网关中解密。

**问**：对于网络带宽是否有要求？ <br/>
**答**：建议为网络连接配置较高的吞吐量。 每个环境是不同的，所发送的数据量会影响效果。 使用 ExpressRoute 可以帮助保证本地与 Azure 数据中心之间的吞吐量级别。
可以借助第三方工具 Azure Speed Test 应用来测量吞吐量。

**问**：从网关运行对数据源的查询时的延迟是多少？ 最佳体系结构是什么？ <br/>
**答**：若要降低网络延迟，请将网关安装在尽可能靠近数据源的位置。 如果可以在实际数据源上安装网关，这种距离可最大程度降低造成的延迟。 还需考虑数据中心。 例如，如果服务使用中国北部的数据中心，而你在 Azure VM 中托管 SQL Server，则 Azure VM 也应该位于中国北部。 这种距离可最大程度降低延迟并避免 Azure VM 产生传出费用。

**问**：如何将结果发送回云？ <br/>
**答**：可通过 Azure 服务总线发送结果。

**问**：是否存在从云到网关的入站连接？ <br/>
**答**：否。 网关使用与 Azure 服务总线之间的出站连接。

**问**：如果阻止出站连接，会发生什么情况？ 我需要打开什么？ <br/>
**答**：查看网关使用的端口和主机。

**问**：实际 Windows 服务的名称是什么？<br/>
**答**：在“服务”中，网关名为“Power BI 企业网关服务”。

**问**：是否可以使用 Azure Active Directory 帐户运行网关 Windows 服务？ <br/>
**答**：否。 该 Windows 服务必须具有有效的 Windows 帐户。 默认情况下，服务使用服务 SID“NT SERVICE\PBIEgwService”来运行。

### <a name="high-availability-and-disaster-recovery"></a>高可用性和灾难恢复

**问**：有哪些选项可用于灾难恢复？ <br/>
**答**：可以使用恢复密钥还原或移动网关。 安装网关时，指定恢复密钥。

**问**：恢复密钥的好处是什么？ <br/>
**答**：使用恢复密钥可在发生灾难后迁移或恢复网关设置。

## <a name="troubleshooting"></a>故障排除

**问**：如何查看正在发送到本地数据源的查询？ <br/>
**答**：可以启用查询跟踪，包括要发送的查询。 请记得在完成故障排除后将查询跟踪改回原始值。 一直保持启用查询跟踪会创建大量的日志。

还可以查看数据源用于跟踪查询的工具。 例如，可以使用 SQL Server 的扩展事件或 SQL 事件探查器以及 Analysis Services。

**问**：网关日志在何处？ <br/>
**答**：请参阅本主题稍后所述的“工具”。

### <a name="update-to-the-latest-version"></a>更新到最新版本

如果网关版本过时，可能会出现很多问题。 良好的常规做法是确保使用最新版本。 如果有一个月或更长时间未更新网关，可能要考虑安装最新版本的网关，并确定是否可以重现问题。

### <a name="error-failed-to-add-user-to-group--2147463168-pbiegwservice-performance-log-users"></a>错误: 无法将用户添加到组。 (-2147463168 PBIEgwService Performance Log Users)

如果尝试在不受支持的域控制器上安装网关，可能会收到此错误。 请确保将网关部署在不是域控制器的计算机上。

## <a name="tools"></a>工具

### <a name="collect-logs-from-the-gateway-configurer"></a>从网关配置器收集日志

可以为网关收集多个日志。 始终从日志开始！

#### <a name="installer-logs"></a>安装程序日志

`%localappdata%\Temp\Power_BI_Gateway_-Enterprise.log`

#### <a name="configuration-logs"></a>配置日志

`%localappdata%\Microsoft\Power BI Enterprise Gateway\GatewayConfigurator.log`

#### <a name="enterprise-gateway-service-logs"></a>企业网关服务日志

`C:\Users\PBIEgwService\AppData\Local\Microsoft\Power BI Enterprise Gateway\EnterpriseGateway.log`

#### <a name="event-logs"></a>事件日志

可在“应用程序和服务日志”下找到数据管理网关和 PowerBIGateway 日志。

### <a name="fiddler-trace"></a>Fiddler 跟踪

[Fiddler](http://www.telerik.com/fiddler) 是 Telerik 提供的一种免费工具，可监视 HTTP 流量。 可在客户端计算机中查看 Power BI 服务发生的此流量。 此服务可能会显示错误和其他相关信息。

### <a name="telemetry"></a>遥测
遥测可用于监视和排错。 

**启用遥测**

1.  查看计算机上的本地数据网关客户端目录。 通常为 **%systemdrive%\Program Files\On-premises data gateway**。 或者可以打开服务控制台，并查看可执行文件（本地数据网关服务的属性之一）的路径。
2.  在客户端目录的 Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config 文件中， 将 SendTelemetry 设置更改为 true。

    ```
        <setting name="SendTelemetry" serializeAs="String">
                    <value>true</value>
        </setting>
    ```

3.  保存更改并重启 Windows 服务：本地数据网关服务。

## <a name="next-steps"></a>后续步骤
* [管理 Analysis Services](analysis-services-manage.md)
* [从 Azure Analysis Services 获取数据](analysis-services-connect.md)

<!--Update_Description: add new feature about FAQ and troubleshoting-->