---
title: 将应用与 Azure 虚拟网络集成 - Azure 应用服务
description: 演示如何将 Azure 应用服务中的应用连接到新的或现有的 Azure 虚拟网络
services: app-service
documentationcenter: ''
author: ccompy
manager: stefsch
ms.assetid: 90bc6ec6-133d-4d87-a867-fcf77da75f5a
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 08/23/2017
ms.date: 03/25/2019
ms.author: v-biyu
ms.custom: seodec18
ms.openlocfilehash: d7fff5ff797635d690e906b74d587f62a8fabd60
ms.sourcegitcommit: b1a411528581081a0c93f44741a29bdd6b450f0e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/12/2019
ms.locfileid: "57787318"
---
# <a name="integrate-your-app-with-an-azure-virtual-network"></a>将应用与 Azure 虚拟网络进行集成
本文档介绍 Azure 应用服务虚拟网络集成功能，并说明如何在 [Azure 应用服务](overview.md)中使用应用对其进行设置。 使用 [Azure 虚拟网络][VNETOverview] (VNet) 可将多个 Azure 资源置于无法通过 Internet 路由的网络中。 然后可以使用 VPN 技术将这些网络连接到本地网络。 

在 Azure 中国区，Azure 应用服务只有一个窗体。  

* 支持全部定价计划的多租户系统


VNet 集成功能允许 Web 应用访问虚拟网络中的资源，但不允许通过虚拟网络对 Web 应用进行专用访问。 专用站点访问指的是仅可从专用网络（例如 Azure 虚拟网络内）对应用进行访问。 专用站点访问仅适用于部署了内部负载均衡器 (ILB) 的 ASE。 有关使用 ILB ASE 的详细信息，请先参阅此文章：[创建和使用 ILB ASE][ILBASE]。 

VNet 集成通常用于实现应用对 VNet 中的数据库和运行 Web 服务的访问。 使用 VNet 集成时，不需要公开 VM 中应用程序的公共终结点，可以改用无法通过 Internet 路由的专用地址。 

VNet 集成功能：

* 需要标准、高级定价计划 
* 适用于经典 VNet 或资源管理器 VNet 
* 支持 TCP 和 UDP
* 适用于 Web 应用、移动应用、API 应用和 Function 应用
* 允许应用一次只连接到 1 个 VNet
* 允许在应用服务计划中一次最多集成 5 个 VNet 
* 允许在应用服务计划中由多个应用使用同一个 VNet
* 需要使用点到站点 VPN 配置的虚拟网络网关
* 由于网关上的 SLA，可实现 99.9% 的 SLA

VNet 集成不支持某些功能，其中包括：

* 装载驱动器
* AD 集成 
* NetBios
* 专用站点访问
* 访问 ExpressRoute 上的资源 
* 访问服务终结点上的资源 

### <a name="getting-started"></a>入门
将 Web 应用连接到虚拟网络之前，需要牢记以下几点：

* 在将目标虚拟网络连接到应用之前，必须借助基于路由的网关启用点到站点 VPN。 
* VNet 所在的订阅必须与应用服务计划 (ASP) 所在的订阅相同。
* 与 VNet 集成的应用使用为该 VNet 指定的 DNS。

## <a name="enabling-vnet-integration"></a>启用 VNet 集成



### <a name="set-up-a-gateway-in-your-vnet"></a>在 VNet 中设置网关 ###

如果已使用点到站点地址配置网关，则可以跳至“配置 VNet 与应用的集成”这一步。  
若要创建网关，请执行以下操作：

1. 在 VNet 中[创建网关子网][creategatewaysubnet]。  

1. [创建 VPN 网关][creategateway]。 选择基于路由的 VPN 类型。

1. [设置点到站点地址][setp2saddresses]。 如果网关不在基本 SKU 中，则必须在点到站点配置中禁用 IKEV2 并选择 SSTP。 地址空间必须位于下列其中一个地址块中：

* 10.0.0.0/8 - 这指的是从 10.0.0.0 到 10.255.255.255 的 IP 地址范围
* 172.16.0.0/12 - 这指的是从 172.16.0.0 到 172.31.255.255 的 IP 地址范围 
* 192.168.0.0/16 - 这指的是从 192.168.0.0 到 192.168.255.255 的 IP 地址范围

如果只是创建网关用于应用服务 VNet 集成，则不需要上传证书。 创建网关可能需要 30 分钟。 若要将应用与 VNet 集成，必须先预配网关。 

### <a name="configure-vnet-integration-with-your-app"></a>使用应用配置 VNet 集成 ###

要在应用上启用 VNet 集成，请执行以下步骤： 

1. 在 Azure 门户中转到该应用并打开应用设置，然后选择“网络”>“VNet 集成”。 在配置 VNet 集成前，需将 ASP 缩放至标准或更佳的 SKU。 
 ![VNet 集成 UI][1]

1. 选择“添加 VNet”。 
 ![添加 VNet 集成][2]

1. 选择 VNet。 
  ![选择 VNet][8]
  
完成最后一步后，应用将会重启。  

## <a name="how-the-system-works"></a>系统的工作方式
VNet 集成功能基于点到站点 VPN 技术。 Azure 应用服务中的应用托管在多租户系统中，它可以阻止直接在 VNet 中预配应用的操作。 点到站点技术将网络访问限制于托管应用的虚拟机。 应用受到限制，只能通过混合连接或 VNet 集成向外发送流量至 Internet。 

![VNet 集成的工作原理][3]

## <a name="managing-the-vnet-integrations"></a>管理 VNet 集成
连接到 VNet 以及断开其连接的功能在应用级别执行。 可能影响多个应用的 VNet 集成的操作在应用服务计划级别执行。 可以通过应用 UID 获取 VNet 的详细信息。 同样的信息也会显示在 ASP 级别。 

 ![VNet 详细信息][4]

在“网络功能状态”页中，可查看应用是否已连接到 VNet。 如果 VNet 网关因某种原因而关闭，状态则会显示为未连接。 

现在应用级别的 VNet 集成 UI 中获得的信息与你通过 ASP 获得的详细信息是相同的。 下面是信息中的各项内容：

* VNet 名称 - 链接至虚拟网络 UI
* 位置 - 反映 VNet 的位置。 与其他位置的 VNet 集成可能会导致应用出现延迟。 
* 证书状态 - 反映证书在应用服务计划和 VNet 之间的同步状态。
* 网关状态 - 如果网关因某种原因而关闭，则应用无法访问 VNet 中的资源。 
* VNet 地址空间 - 显示 VNet 的 IP 地址空间。 
* 点到站点地址空间 - 显示 VNet 的点到站点 IP 地址空间。 对 VNet 进行调用时，应用的发件人地址将为这些地址之一。 
* 站点到站点地址空间 - 可以使用站点到站点 VPN 将 VNet 连接到本地资源或其他 VNet。 使用该 VPN 连接定义的 IP 范围如下所示。
* DNS 服务器 - 显示配置了 VNet 的 DNS 服务器。
* 路由到 VNet 的 IP 地址 - 显示路由的地址块，这些地址块用于驱动流量进入 VNet 

在 VNet 集成的应用视图中，能够执行的唯一操作是断开应用与当前所连接到的 VNet 的连接。 若要断开应用与 VNet 的连接，请选择“断开连接”。 在从 VNet 断开连接时，应用将会重启。 断开连接操作不会更改 VNet。 VNet 及其包括网关在内的配置都保持不变。 如果随后想要删除 VNet，则需先删除其中的资源（包括网关）。 

若要访问 ASP VNet 集成 UI，请打开 ASP UI 并选择“网络”。  在 VNet 集成下，选择“单击此处可配置”以打开网络功能状态 UI。

![ASP VNet 集成信息][5]

ASP VNet 集成 UI 会显示 ASP 中的应用使用的所有 VNet。 应用服务计划中任意数量的应用最多可能连接 5 个 VNet。 每个应用只能配置一个集成。 要查看每个 VNet 的详细信息，请单击感兴趣的 VNet。 此处有两种操作可以执行。

* **同步网络**。 同步网络操作确保证书与网络信息是同步的。如果添加或更改 VNet 的 DNS，则需执行“同步网络”操作。 此操作会重启所有使用此 VNet 的应用。
* **添加路由** 添加路由会驱动出站流量进入 VNet。

**路由** 在 VNet 中定义的路由，用于将流量从应用导入 VNet。 如果需要将其他出站流量发送到 VNet 中，则可以在此处添加地址块。   

**证书** 启用 VNet 集成后，必须进行证书交换以确保连接的安全性。 除了证书，还有 DNS 配置、路由以及其他类似的用于描述网络的内容。
如果更改了证书或网络信息，则需单击“同步网络”。 单击“同步网络”会导致应用与 VNet 之间的连接出现短暂的中断。 虽然应用不会重启，但失去连接会导致站点功能失常。 

## <a name="accessing-on-premises-resources"></a>访问本地资源
应用可以通过与具备站点到站点连接的 VNet 集成来访问本地资源。 若要访问本地资源，需要使用点到站点地址块更新本地 VPN 网关路由。 先设置站点到站点 VPN，接着应通过用于配置该 VPN 的脚本来正确地设置路由。 如果在创建站点到站点地址后才添加点到站点 VPN，则需手动更新路由。 具体操作信息取决于每个网关，在此不作说明。 此外，不能使用站点到站点 VPN 连接配置 BGP。

> [!NOTE]
> VNET 集成功能不会将应用与包含 ExpressRoute 网关的 VNet 集成。 即使 ExpressRoute 网关是以[共存模式][VPNERCoex]配置的，vNet 集成也不会实现。
> 
> 

## <a name="peering"></a>对等互连
可以使用 VNet 集成访问与所连接的 VNet 对等互连的 VNet 中的资源。 若要配置对等互连以使用应用，请执行以下操作：

1. 在应用所连接的 VNet 上添加对等互连连接。 在添加对等互连连接时，启用“允许虚拟网络访问”并单击“允许转发流量”和“允许网关传输”。
1. 在与所连接的 VNet 对等互连的 VNet 上添加对等互连连接。 在目标 VNet 上添加对等互连连接时，启用“允许虚拟网络访问”并单击“允许转发流量”和“允许远程网关”。
1. 转到门户中的“应用服务计划”>“网络”>“VNet 集成 UI”。  选择应用连接的 VNet。 在路由部分下，添加与应用所连接的 VNet 对等互连的 VNet 的地址范围。  


## <a name="pricing-details"></a>定价详细信息
使用 VNet 集成功能涉及到三种相关费用：

* ASP 定价层要求
* 数据传输费用
* VPN 网关费用。

应用需要属于“标准”、“高级”应用服务计划。 可在此处了解这些费用的更多详细信息：[应用服务定价][ASPricing]。 

数据出口方面也存在费用，即使 VNet 在同一数据中心也是如此。 [数据传输定价详细信息][DataPricing]中对这些费用进行了说明。 

点到站点 VPN 所需的 VNet 网关会产生费用。 [VPN 网关定价][VNETPricing]页上介绍了详细信息。

## <a name="troubleshooting"></a>故障排除
虽然此功能容易设置，但这并不意味着你就不会遇到问题。 如果在访问所需终结点时遇到问题，可以使用某些实用程序来测试从应用控制台发出的连接。 可以使用两种控制台。 一种是 Kudu 控制台，另一种是 Azure 门户中的控制台。 若要访问应用中的 Kudu 控制台，请转到“工具”->“Kudu”。 这相当于访问 [sitename].scm.chinacloudsites.cn。 打开后，转到“调试控制台”选项卡。若要访问 Azure 门户托管的控制台，请在应用中转到“工具”->“控制台”。 

#### <a name="tools"></a>工具
由于存在安全约束，**ping**、**nslookup** 和 **tracert** 工具无法通过控制台来使用。 为了填补这方面的空白，我们添加了两种单独的工具。 为了测试 DNS 功能，我们添加了名为 nameresolver.exe 的工具。 语法为：

    nameresolver.exe hostname [optional: DNS Server]

可以使用 **nameresolver** 来检查应用所依赖的主机名。 可以通过这种方式来测试 DNS 是否配置错误，或者测试你是否无权访问 DNS 服务器。

下一工具适用于测试与主机的 TCP 连接情况，以及端口组合情况。 该工具名为 **tcpping**，语法为：

    tcpping.exe hostname [optional: port]

**tcpping** 实用程序会告知是否可访问特定主机和端口。 仅满足以下条件才会显示成功：存在侦听主机和端口组合的应用程序，且可从应用对指定主机和端口进行网络访问。

#### <a name="debugging-access-to-vnet-hosted-resources"></a>针对 VNET 托管的资源进行访问权限调试
许多因素会阻止应用访问特定的主机和端口。  大多数情况下为以下三种情况：

* **存在防火墙。** 如果存在防火墙，则会发生 TCP 超时。 本例中的 TCP 超时为 21 秒。 使用 **tcpping** 工具测试连接性。 除了防火墙外，还有多种原因可能导致 TCP 超时。 
* **DNS 不可访问。** DNS 超时时间为每个 DNS 服务器 3 秒。 如果具有 2 个 DNS 服务器，则超时为 6 秒。 使用 nameresolver 查看 DNS 是否正常工作。 请记住，不能使用 nslookup，因其没有使用为 VNet 配置的 DNS。
* **点到站点地址范围无效。** 点到站点 IP 范围点需要处于 RFC 1918 专用 IP 范围 (10.0.0.0-10.255.255.255 / 172.16.0.0-172.31.255.255 / 192.168.0.0-192.168.255.255) 内。 如果范围的 IP 超出上述范围，则操作无法正常运行。 

如果这些情况不能解决问题，请首先查看以下简单因素：  

* 网关在门户中是否显示为已启动？
* 证书是否显示为处于同步状态？
* 是否有人更改了网络配置却没有在受影响的 ASP 中执行“同步网络”操作？ 

如果网关处于关闭状态，则将其重新启动。 如果证书不同步，则请转到 VNet 集成的 ASP 视图，并单击“同步网络”。 如果怀疑 VNet 配置存在与 ASP 不同步的更改，请单击“同步网络”。  “同步网络”操作将重启 ASP 中任何与该 VNet 集成的应用。 

如果这些都正常，则需进行更深入的故障诊断：

* 是否存在其他应用在使用 VNET 集成访问同一 VNET 中的资源？ 
* 是否能够转到应用控制台并使用 tcpping 来访问 VNET 中的任何其他资源？  

如果上面这两个问题中有一个的回答为“是”，则说明 VNet 集成功能正常，问题出在其他地方。 这种情况下，解决问题会更加困难，因为并没有简单的方法可以查看你为何无法访问主机:端口。 部分原因包括：

* 在主机上开启了防火墙，导致无法从点到站点 IP 范围访问应用程序端口。  跨子网通常需要公共访问权限。
* 目标主机已关闭
* 应用程序已关闭
* IP 或主机名错误
* 应用程序所侦听的端口不同于所期望的端口。 可以使用终结点主机上的“netstat -aon”匹配进程 ID 和侦听端口。 
* 网络安全组的配置方式导致无法从点到站点 IP 范围访问应用程序主机和端口

请记住，由于不知道应用会使用点到站点 IP 范围中的哪个 IP，因此需允许整个范围中的 IP 进行访问。 

其他调试步骤包括：

* 连接到 VNet 中的某个 VM，尝试在该处访问资源主机:端口。 若要针对 TCP 访问权限进行测试，请使用 PowerShell 命令 test-netconnection。 语法为：

      test-netconnection hostname [optional: -Port]

* 在某个 VM 中启动应用程序，测试能否在应用的控制台中访问该主机和端口

#### <a name="on-premises-resources"></a>本地资源 ####

如果应用无法访问本地资源，则请检查是否能够通过 VNet 访问该资源。 请使用 test-netconnection PowerShell 命令来针对 TCP 访问权限进行测试。 如果 VM 无法访问本地资源，请确保站点到站点 VPN 连接正常。 如果正常，则请检查前述项目以及本地网关配置和状态。 

如果 VNet 托管的 VM 能够访问本地系统但应用无法访问，则可能是由于以下某个原因：

* 在本地网关中未使用点到站点 IP 范围对路由进行配置
* 网络安全组阻止点到站点 IP 范围中的 IP 进行访问
* 本地防火墙阻止来自点到站点 IP 范围的流量


## <a name="powershell-automation"></a>PowerShell 自动化

可以使用 PowerShell 将应用服务与 Azure 虚拟网络进行集成。 有关就绪可运行的脚本，请参阅 [Connect an app in Azure App Service to an Azure Virtual Network](https://gallery.technet.microsoft.com/scriptcenter/Connect-an-app-in-Azure-ab7527e3)（将 Azure 应用服务中的应用连接到 Azure 虚拟网络）。
<!--Image references-->
[1]: ./media/web-sites-integrate-with-vnet/vnetint-app.png [2]: ./media/web-sites-integrate-with-vnet/vnetint-addvnet.png [3]: ./media/web-sites-integrate-with-vnet/vnetint-howitworks.png [4]: ./media/web-sites-integrate-with-vnet/vnetint-details.png [5]: ./media/web-sites-integrate-with-vnet/vnetint-aspdetails.png [6]: ./media/web-sites-integrate-with-vnet/vnetint-newvnet.png [7]: ./media/web-sites-integrate-with-vnet/vnetint-newvnetdetails.png [8]: ./media/web-sites-integrate-with-vnet/vnetint-selectvnet.png

<!--Links-->
[VNETOverview]: ../virtual-network/virtual-networks-overview.md
[AzurePortal]: http://portal.azure.cn/
[ASPricing]: https://www.azure.cn/pricing/details/app-service/
[VNETPricing]: https://www.azure.cn/pricing/details/vpn-gateway/
[DataPricing]: https://www.azure.cn/pricing/details/data-transfer/
[V2VNETP2S]: https://www.azure.cn/documentation/articles/vpn-gateway-howto-point-to-site-rm-ps/
[V2VNETPortal]: ../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md
[VPNERCoex]: ../expressroute/expressroute-howto-coexist-resource-manager.md

[creategatewaysubnet]: http://docs.azure.cn/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal#gatewaysubnet
[creategateway]: http://docs.azure.cn/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal#creategw
[setp2saddresses]: http://docs.azure.cn/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal#addresspool
