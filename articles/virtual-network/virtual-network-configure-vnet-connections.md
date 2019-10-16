---
title: 配置和验证 VNet 或 VPN 连接
description: 有关配置和验证各种 Azure VPN 与 VNet 部署的分步指南
services: virtual-network
documentationcenter: na
author: rockboyfor
manager: digimobile
editor: ''
ms.assetid: 0433a4f4-b5a0-476d-b398-1506c57eafa2
ms.service: virtual-network
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 08/28/2019
ms.date: 09/23/2019
ms.author: v-yeche
ms.openlocfilehash: d45982cd1bf77c1f64ccea5caaee5a13a2c1345e
ms.sourcegitcommit: 0d07175c0b83219a3dbae4d413f8e012b6e604ed
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/26/2019
ms.locfileid: "71306841"
---
# <a name="configure-and-validate-vnet-or-vpn-connections"></a>配置和验证 VNet 或 VPN 连接

本引导式演练提供有关在传输路由、VNet 到 VNet、BGP、多站点、点到站点等方案中配置和验证各种 Azure VPN 与 VNet 部署的分步指南。

借助 Azure VPN 网关的灵活性，可以在 Azure 中排列几乎所有类型的互连式虚拟网络 (VNet) 拓扑：可以跨区域、在不同的 VNet 类型（Azure 资源管理器与经典部署）之间、在 Azure 内部连接 VNet，或者将 VNet 连接到不同订阅中的本地混合环境，等等。 

## <a name="scenario-1-vnet-to-vnet-vpn-connection"></a>方案 1：VNet 到 VNet VPN 连接

通过 VPN 将一个虚拟网络连接到另一个虚拟网络（VNet 到 VNet）类似于将 VNet 连接到本地站点位置。 这两种连接类型都使用 VPN 网关来提供使用 **IPsec/IKE** 的安全隧道。 虚拟网络可位于相同或不同的区域，来自相同或不同的订阅。

![图像](./media/virtual-network-configure-vnet-connections/4034386_en_2.png)

图 1 - 使用 IPsec 的 VNet 到 VNet 连接

如果 VNet 位于同一区域，你可能会考虑使用 VNet 对等互连来连接这些 VNet，这种连接方式不使用 VPN 网关。若要提高吞吐量并减少延迟，请选择“配置并验证 VNet 对等互连”来配置 VNet 对等互连。 

如果两个 VNet 是使用 Azure 资源管理器部署模型创建的，请选择“配置并验证资源管理器 VNet 到资源管理器 VNet 的连接”来配置 VPN 连接。 

如果一个 VNet 是使用 Azure 经典部署模型创建的，另一个 VNet 是通过资源管理器创建的，请选择“配置并验证经典 VNet 到资源管理器 VNet 的连接”来配置 VPN 连接。 

### <a name="configuration-1-configure-vnet-peering-for-two-vnets-in-the-same-region"></a>配置 1：为同一区域中的两个 VNet 配置 VNet 对等互连

在开始实现 Azure VNet 对等互连之前，请确保满足以下配置 VNet 对等互连的先决条件：

* 对等虚拟网络必须位于同一 Azure 区域。
* 对等虚拟网络的 IP 地址空间不得重叠。
* 虚拟网络对等互连在两个虚拟网络之间进行。 对等互连之间没有任何派生的可传递关系。 例如，如果 VNetA 与 VNetB 对等互连，VNetB 与 VNetC 对等互连，但 VNetA *不* 与 VNetC 对等互连。

满足要求后，可以根据[创建虚拟网络对等互连教程](/virtual-network/virtual-network-create-peering)来创建并配置 VNet 对等互连。

若要检查 VNet 对等互连配置，请使用以下方法：

1. 使用具有必要[角色和权限](/virtual-network/virtual-network-manage-peering#roles-permissions)的帐户登录到 [Azure 门户](https://portal.azure.cn/)。
2. 在 Azure 门户顶部包含“搜索资源”文本的框中，键入“virtual networks”   。 当“虚拟网络”出现在搜索结果中时，请单击它。 
3. 在显示的“虚拟网络”边栏选项卡中，单击想要为其创建对等互连的虚拟网络。 
4. 在针对所选虚拟网络显示的窗格中，单击“设置”部分中的“对等互连”。  
5. 单击要检查其配置的对等互连。

![图像](./media/virtual-network-configure-vnet-connections/4034496_en_1.png)

使用 Azure Powershell 运行命令 [Get-AzureRmVirtualNetworkPeering](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermvirtualnetworkpeering?view=azurermps-4.1.0) 获取虚拟网络对等互连，例如：

```
PS C:\Users\User1> Get-AzureRmVirtualNetworkPeering -VirtualNetworkName Vnet10-01 -ResourceGroupName dev-vnets
Name                             : LinkToVNET10-02
Id                               : /subscriptions/GUID/resourceGroups/dev-vnets/providers/Microsoft.Network/virtualNetworks/VNET10-01/virtualNetworkPeerings/LinkToVNET10-0
2
Etag                             : W/"GUID"
ResourceGroupName                : dev-vnets
VirtualNetworkName               : vnet10-01
ProvisioningState                : Succeeded
RemoteVirtualNetwork             : {
                                  "Id": "/subscriptions/GUID/resourceGroups/DEV-VNET
                                   s/providers/Microsoft.Network/virtualNetworks/VNET10-02"
                                   }
AllowVirtualNetworkAccess        : True
AllowForwardedTraffic            : False
AllowGatewayTransit              : False
UseRemoteGateways                : False
RemoteGateways                   : null
RemoteVirtualNetworkAddressSpace : null
```

### <a name="connection-type-1-connect-a-resource-manager-vnet-to-anther-resource-manager-vnet-azure-resource-manager-to-azure-resource-manager"></a>连接类型 1：将资源管理器 VNet 连接到另一个资源管理器 VNet（Azure 资源管理器到 Azure 资源管理器）

可以直接配置从一个资源管理器 VNet 到另一个资源管理器 VNet 的连接，或配置使用 IPsec 的连接。

### <a name="configuration-2-configure-vpn-connection-between-resource-manager-vnets"></a>配置 2：在资源管理器 VNet 之间配置 VPN 连接

若要在不使用 IPsec 的情况下在资源管理器 VNet 之间配置连接，请参阅[使用 Azure 门户配置 VNet 到 VNet VPN 网关连接](/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal)。

若要在两个资源管理器 VNet 之间配置使用 IPsec 的连接，请针对每个 VNet 遵循[在 Azure 门户中创建站点到站点连接](/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)中的步骤 1-5。

> [!Note]
> 这些步骤仅适用于同一订阅中的 VNet。 如果 VNet 属于不同的订阅，则必须使用 PowerShell 进行连接。 请参阅 [PowerShell](/vpn-gateway/vpn-gateway-vnet-vnet-rm-ps) 一文。

### <a name="validate-vpn-connection-between-resource-manager-vnets"></a>验证资源管理器 VNet 之间的 VPN 连接

![图像](./media/virtual-network-configure-vnet-connections/4034493_en_2.png)

图 4 - 经典 VNet 到 Azure 资源管理器 VNet 的连接

若要检查是否已正确配置 VPN 连接，请按照以下说明操作：

> [!Note]
> 虚拟网络组件后面的编号（例如以下步骤中的“VNet 1”）对应于图 4 中的编号。

1. 检查连接的 VNet 中是否存在重叠的地址空间。

   > [!Note]
   > VNet 1 和 VNet 6 中不能存在重叠的地址空间。 

2. 验证是否在**连接对象** 4 中准确定义了 Azure 资源管理器 VNet 1 地址范围。
3. 验证是否在**连接对象** 3 中准确定义了 Azure 资源管理器 VNet 6 地址范围。
4. 验证连接对象 3 和 4 中的预共享密钥是否匹配。
5. 验证是否在**连接对象** 4 中准确定义了 Azure 资源管理器 VNet 网关 2 的 VIP。
6. 验证是否在**连接对象** 3 中准确定义了 Azure 资源管理器 VNet 网关 5 的 VIP。

### <a name="connection-type-2-connect-a-classic-vnet-to-a-resource-manager-vnet"></a>连接类型 2：将经典 VNet 连接到资源管理器 VNet

可以在位于不同订阅、不同区域中的 VNet 之间创建连接。 还可以连接已连接到本地网络的 VNet，前提是已将网关类型配置为基于路由。

若要在经典 VNet 与资源管理器 VNet 之间配置连接，请参阅[使用 Azure 门户从不同的部署模型连接虚拟网络](/vpn-gateway/vpn-gateway-connect-different-deployment-models-portal)了解详细信息。

![图像](./media/virtual-network-configure-vnet-connections/4034389_en_2.png)

图 5 - 经典 VNet 到 Azure 资源管理器 VNet 的连接

若要检查将经典 VNet 连接到 Azure 资源管理器 VNet 时的配置，请按照以下说明操作：

> [!Note]
> 虚拟网络组件后面的编号（例如以下步骤中的“VNet 1”）对应于图 5 中的编号。

1. 检查连接的 VNet 中是否存在重叠的地址空间。

   > [!Note]
   > VNet 1 和 VNet 6 中不能存在重叠的地址空间

2. 验证是否在经典本地网络定义 3 中准确定义了 Azure 资源管理器 VNet 6 地址范围。
3. 验证是否在 Azure 资源管理器**连接对象** 4 中准确定义了经典 VNet 1 地址范围。
4. 验证是否在 Azure 资源管理器**连接对象** 4 中准确定义了经典 VNet 网关 2 的 VIP。
5. 验证是否在**本地网络定义** 3 中准确定义了 Azure 资源管理器 VNet 网关 5 的 VIP。
6. 验证两个连接的 VNet 上的预共享密钥是否匹配：
   1. 经典 VNet - 本地网络定义3
   2. Azure 资源管理器 VNet - 连接对象 4

## <a name="scenario-2-point-to-site-vpn-connection"></a>方案 2：点到站点 VPN 连接

使用点到站点 (P2S) 配置可以创建从单个客户端计算机到虚拟网络的安全连接。 如果要从远程位置（例如，从家里或会议室）连接到 VNet，或者只有少数几台客户端计算机需要连接到虚拟网络，点到站点连接会非常有用。 P2S VPN 连接通过本机 Windows VPN 客户端从客户端计算机启动。 连接客户端使用证书进行身份验证。

![图像](./media/virtual-network-configure-vnet-connections/4034387_en_3.png)

图 2 - 点到站点连接

点到站点连接不需要 VPN 设备。 P2S 创建基于 SSTP（安全套接字隧道协议）的 VPN 连接。 可以使用不同的部署工具和部署模型来与 VNet 建立点到站点连接：

* [使用 Azure 门户配置与 VNet 的点到站点连接](/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal)
* [使用 Azure 门户（经典）配置与 VNet 的点到站点连接](/vpn-gateway/vpn-gateway-howto-point-to-site-classic-azure-portal)
* [使用 PowerShell 配置与 VNet 的点到站点连接](/vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps)

### <a name="validate-your-point-to-site-connections"></a>验证点到站点连接

[故障排除：Azure 点到站点连接问题](/vpn-gateway/vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems)一文逐步讲解了如何排查点到站点连接的常见问题。

## <a name="scenario-3-multi-site-vpn-connection"></a>方案 3：多站点 VPN 连接

可将站点到站点 (S2S) 连接添加到已建立 S2S 连接、点到站点连接或 VNet 到 VNet 连接的 VNet，这种连接通常称为**多站点**配置。 

![图像](./media/virtual-network-configure-vnet-connections/4034497_en_2.png)

图 3 - 多站点连接

Azure 当前使用两种部署模型：资源管理器和经典。 这两种模型彼此不完全兼容。 若要使用不同的模型配置**多站点**连接，请查看以下文章：

* [将站点到站点连接添加到包含现有 VPN 网关连接的 VNet](/vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal)
* [将站点到站点连接添加到包含现有 VPN 网关连接的 VNet（经典）](/vpn-gateway/vpn-gateway-multi-site)

> [!Note]
> 本文中的步骤不适用于 ExpressRoute 和站点到站点共存连接配置。 有关共存连接的详细信息，请参阅 [ExpressRoute/S2S 共存连接](/expressroute/expressroute-howto-coexist-resource-manager)。

## <a name="scenario-4-configure-transit-routing"></a>方案 4：配置传输路由

传输路由是一种特定的路由方案，在其中可以“菊花链”拓扑的形式连接多个网络。 此路由方式可让“链”两端的 VNet 中的资源通过其间的 VNet 相互通信。 如果没有传输路由，通过中心对等互连的网络或设备无法相互访问。

### <a name="configuration-1-configure-transit-routing-in-a-point-to-site-connection"></a>配置 1：在点到站点连接中配置传输路由

在此方案中，需在 VNetA 与 VNetB 之间配置站点到站点 VPN 连接，并配置点到站点 VPN，使客户端能够连接到 VNetA 的网关。 然后，可为通过 VNetA 连接到 VNetB 的点到站点客户端启用传输路由。 在 VNetA 与 VNetB 之间的站点到站点 VPN 上启用 BGP 后，将会支持此方案。 有关详细信息，请参阅[关于点到站点 VPN 路由](/vpn-gateway/vpn-gateway-about-point-to-site-routing)。

### <a name="configuration-2-configure-transit-routing-in-an-expressroute-connection"></a>配置 2：在 ExpressRoute 连接中配置传输路由

使用 Azure ExpressRoute 可通过连接服务提供商所提供的专用连接，将本地网络扩展到 Azure 云。 使用 ExpressRoute 可与 Azure、Office 365 和 Dynamics 365 等 Azure 云服务建立连接。 有关详细信息，请参阅 [ExpressRoute 概述](/expressroute/expressroute-introduction)。

![图像](./media/virtual-network-configure-vnet-connections/4034395_en_1.png)

图 6 - 与 Azure VNet 建立 ExpressRoute“专用对等互连”连接

> [!Note]
> 我们建议，如果 VNetA 和 VNetB 位于同一地缘政治区域，则[将两个 VNet 都链接到 ExpressRoute 线路](/expressroute/expressroute-howto-linkvnet-arm)，而不要配置传输路由。 如果 VNet 位于不同的地缘政治区域，并且你已获得 [ExpressRoute 高级版](/expressroute/expressroute-faqs#expressroute-premium)，则也可以直接将 VNet 链接到自己的线路。 

如果 ExpressRoute 和站点到站点连接共存，则不支持传输路由。 有关详细信息，请参阅[配置 ExpressRoute 和站点到站点共存连接](/expressroute/expressroute-howto-coexist-resource-manager)。

如果已启用 ExpressRoute 以将本地网络连接到 Azure 虚拟网络，则可以在要建立传输路由的 VNet 之间启用 VNet 对等互连。 要使本地网络能够连接到远程 VNet，必须配置[虚拟网络对等互连](/virtual-network/virtual-network-peering-overview#gateways-and-on-premises-connectivity)。 

> [!Note]
> VNet 对等互连仅适用于同一区域中的 VNet。

若要检查是否为 VNet 对等互连配置了传输路由，请按照以下说明操作：

1. 使用具有必要[角色和权限](/virtual-network/virtual-network-manage-peering#roles-permissions)的帐户登录到 [Azure 门户](https://portal.azure.cn/)。
2. 如前面的示意图（图 6）中所示，[在 VNet A 与 B 之间创建对等互连](/virtual-network/virtual-network-create-peering)。 
3. 在针对所选虚拟网络显示的窗格中，单击“设置”部分中的“对等互连”。  
4. 单击要查看的对等互连，然后单击“配置”，验证是否已在连接到 ExpressRoute 线路的 VNetA 上启用了“允许网关传输”，并在未连接到 ExpressRoute 线路的远程 VNetB 上启用了“使用远程网关”。   

### <a name="configuration-3-configure-transit-routing-in-a-vnet-peering-connection"></a>配置 3：在 VNet 对等互连中配置传输路由

将虚拟网络对等互连后，还可以将对等虚拟网络中的网关配置为本地网络的传输点。 若要在 VNet 对等互连中配置传输路由，请查看[虚拟网络到虚拟网络连接](/vpn-gateway/vpn-gateway-vnet-vnet-rm-ps?toc=/virtual-network/toc.json)。

> [!Note]
> 通过不同部署模型创建的虚拟网络之间的对等互连关系不支持网关传输。 若要使用网关传输，对等互连关系中的两个虚拟网络都必须通过 Resource Manager 创建。

若要检查是否为 VNet 对等互连配置了传输路由，请按照以下说明操作：

1. 使用具有必要[角色和权限](/virtual-network/virtual-network-manage-peering#roles-permissions)的帐户登录到 [Azure 门户](https://portal.azure.cn/)。
2. 在 Azure 门户顶部包含“搜索资源”文本的框中，键入“虚拟网络”。  当“虚拟网络”出现在搜索结果中时，请单击它。 
3. 在显示的“虚拟网络”边栏选项卡中，单击要检查其对等互连设置的虚拟网络。 
4. 在针对所选虚拟网络显示的窗格中，单击“设置”部分中的“对等互连”。  
5. 单击要查看的对等互连，并在“配置”下验证是否已启用“允许网关传输”和“使用远程网关”。   

![图像](./media/virtual-network-configure-vnet-connections/4035414_en_1.png)

### <a name="configuration-4-configure-transit-routing-in-a-vnet-to-vnet-connection"></a>配置 4：在 VNet 到 VNet 连接中配置传输路由

若要在 VNet 之间配置传输路由，必须使用资源管理器部署模型和 PowerShell 在所有中间 VNet 到 VNet 连接上启用 BGP。 有关说明，请参阅[如何使用 PowerShell 在 Azure VPN 网关上配置 BGP](/vpn-gateway/vpn-gateway-bgp-resource-manager-ps)。

使用经典部署模型通过 Azure VPN 网关传输流量是可行的，但依赖于网络配置文件中静态定义的地址空间。 使用经典部署模型的 Azure 虚拟网络和 VPN 网关尚不支持 BGP。 如果没有 BGP，手动定义传输地址空间很容易出错，因此不建议这样做。

> [!Note]
> 可以使用 Azure 门户（经典）或使用经典门户中的网络配置文件来配置经典 VNet 到 VNet 连接。 无法通过 Azure 资源管理器部署模型或 Azure 门户来创建或修改经典虚拟网络。 有关经典 VNet 传输路由的详细信息，请参阅[使用 VPN 的 Azure ARM 中的中心辐射、菊花链和全网格 VNET 拓扑 (V1)](https://blogs.msdn.microsoft.com/igorpag/2015/10/01/hubspoke-daisy-chain-and-full-mesh-vnet-topologies-in-azure-arm-using-vpn-v1/)。

### <a name="configuration-5-configure-transit-routing-in-a-site-to-site-connection"></a>配置 5：在站点到站点连接中配置传输路由

若要在使用站点到站点连接的本地网络与 VNet 之间配置传输路由，必须使用资源管理器部署模型和 PowerShell 在所有中间站点到站点连接上启用 BGP。有关说明，请参阅[如何使用 PowerShell 在 Azure VPN 网关上配置 BGP](/vpn-gateway/vpn-gateway-bgp-resource-manager-ps)。

使用经典部署模型通过 Azure VPN 网关传输流量是可行的，但依赖于网络配置文件中静态定义的地址空间。 使用经典部署模型的 Azure 虚拟网络和 VPN 网关尚不支持 BGP。 如果没有 BGP，手动定义传输地址空间很容易出错，因此不建议这样做。

> [!Note]
> 可以使用 Azure 门户（经典）或使用经典门户中的网络配置文件来配置经典站点到站点连接。 无法通过 Azure 资源管理器部署模型或 Azure 门户来创建或修改经典虚拟网络。 有关经典 VNet 传输路由的详细信息，请参阅[使用 VPN 的 Azure ARM 中的中心辐射、菊花链和全网格 VNET 拓扑 (V1)](https://blogs.msdn.microsoft.com/igorpag/2015/10/01/hubspoke-daisy-chain-and-full-mesh-vnet-topologies-in-azure-arm-using-vpn-v1/)。

## <a name="scenario-5-configure-bgp-for-a-vpn-gateway"></a>方案 5：为 VPN 网关配置 BGP

BGP 是在 Internet 上使用的，用于在两个或更多网络之间交换路由和可访问性信息的标准路由协议。 在 Azure 虚拟网络的上下文中使用 BGP 时，BGP 支持 Azure VPN 网关和本地 VPN 设备（称为 BGP 对等节点或邻居）。 这些设备会向这两个网关提供有关前缀可用性和可访问性的信息，以便通过所涉及的网关或路由器。 BGP 还可以通过将 BGP 网关从一个 BGP 对等节点获知的路由传播到所有其他 BGP 对等节点来允许在多个网络之间传输路由。 有关详细信息，请参阅[使用 Azure VPN 网关的 BGP 概述](/vpn-gateway/vpn-gateway-bgp-overview)。

### <a name="configure-bgp-for-a-vpn-connection"></a>为 VPN 连接配置 BGP

若要配置使用 BGP 的 VPN 连接，请参阅[使用 PowerShell 在 Azure VPN 网关上配置 BGP](/vpn-gateway/vpn-gateway-bgp-resource-manager-ps)。

> [!Note]
> 1. 通过为虚拟网络网关创建 AS 编号在虚拟网络网关上启用 BGP。 基本网关不支持 BGP。 若要检查网关的 SKU，请在 Azure 门户中转到“VPN 网关”边栏选项卡的“概述”部分。 如果 SKU 为“基本”，则必须将 SKU 更改为“VpnGw1”SKU（[调整网关大小](https://docs.microsoft.com/powershell/module/azurerm.network/resize-azurermvirtualnetworkgateway?view=azurermps-4.1.0&viewFallbackFrom=azurermps-4.0.0)）。   检查 SKU 会导致 20-30 分钟的停机时间。 网关获得正确的 SKU 后，可以通过 [Set-AzureRmVirtualNetworkGateway](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermvirtualnetworkgateway?view=azurermps-3.8.0) PowerShell cmdlet 添加 AS。 配置 AS 编号后，系统会自动提供网关的 BGP 对等互连 IP。
> 2. 必须使用 AS 编号和 BGP 对等互连地址**手动**提供 LocalNetworkGateway。 可以使用 [New-AzureRmLocalNetworkGateway](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermlocalnetworkgateway?view=azurermps-4.1.0) 或 [Set-AzureRmLocalNetworkGateway](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermlocalnetworkgateway?view=azurermps-4.1.0) PowerShell cmdlet 设置 **ASN** 和 **-BgpPeeringAddress** 值。 某些 AS 编号是为 Azure 保留的，不能按[关于使用 Azure VPN 网关的 BGP](/vpn-gateway/vpn-gateway-bgp-overview#bgp-faq) 中所述使用这些编号。
> 3. 最后，必须为连接对象启用 BGP。 可以通过 [New-AzureRmVirtualNetworkGatewayConnection](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetworkgatewayconnection?view=azurermps-4.1.0) 或 [Set-AzureRmVirtualNetworkGatewayConnection](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermvirtualnetworkgatewayconnection?view=azurermps-4.1.0) 将 **-EnableBGP** 值设置为 `$True`。

### <a name="validate-the-bgp-configuration"></a>验证 BGP 配置

若要检查是否正确配置了 BGP，可以运行 cmdlet `get-AzureRmVirtualNetworkGateway` 和 `get-AzureRmLocalNetworkGateway`，然后可以在 BgpSettingsText 部分看到 BGP 相关的输出。 例如：BgpSettingsText：

```
{

"Asn": AsnNumber,

"BgpPeeringAddress": "IP address",

"PeerWeight": 0

}
```

## <a name="scenario-6-highly-available-active-active-vpn-connection"></a>情景 6：高可用性主动-主动 VPN 连接

主动-主动与主机-待机网关之间的重要差异：

* 必须使用两个公共 IP 地址创建两个网关 IP 配置
* 必须设置 *EnableActiveActiveFeature* 标志
* 网关 SKU 必须是 VpnGw1、VpnGw2、VpnGw3

若要实现跨界连接和 VNet 到 VNet 连接的高可用性，应该部署多个 VPN 网关，在网络与 Azure 之间建立多个并行连接。 有关连接选项和拓扑的概述，请参阅[高可用性跨界连接与 VNet 到 VNet 连接](/vpn-gateway/vpn-gateway-highlyavailable)。

若要创建主动-主动跨界连接和 VNet 到 VNet 连接，请按照[配置与 Azure VPN 网关的主动-主动 S2S VPN 连接](/vpn-gateway/vpn-gateway-activeactive-rm-powershell)中的说明，以主动/主动模式配置 Azure VPN 网关。

> [!Note]  
> 1. 将地址添加到启用了 BGP 的本地网络网关时，主动-主动模式只会添加 BGP 对等互连的 /32 地址。  如果添加更多地址，这些地址将被视为静态路由，并优先于 BGP 路由。
> 2. 对于连接到 Azure 的本地网络，必须使用不同的 BGP ASN。 （如果它们是相同的，并且本地 VPN 设备已使用 ASN 与其他 BGP 邻居建立对等互连，则必须更改 VNet ASN。）

## <a name="scenario-7-change-an-azure-vpn-gateway-type-after-deployment"></a>方案 7：部署后更改 Azure VPN 网关类型

无法将 Azure VNet 网关类型从基于策略更改为基于路由，反之亦然。 必须先删除网关，在此情况下，IP 地址和预共享密钥 (PSK) 不会保留。 然后可以创建所需类型的新网关。 若要删除并创建网关，请执行以下步骤：

1. 删除与原始网关相关联的所有连接。
2. 使用 Azure 门户、PowerShell 或经典 PowerShell 删除网关： 
    * [使用 Azure 门户删除虚拟网络网关](/vpn-gateway/vpn-gateway-delete-vnet-gateway-portal)
    * [使用 PowerShell 删除虚拟网络网关](/vpn-gateway/vpn-gateway-delete-vnet-gateway-powershell)
    * [使用 PowerShell 删除虚拟网络网关（经典）](/vpn-gateway/vpn-gateway-delete-vnet-gateway-classic-powershell)
3. 遵循[创建 VPN 网关](/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal#a-namevnetgatewaya4-create-the-vpn-gateway)中的步骤创建所需类型的新网关，并完成 VPN 设置。

> [!Note]
> 此过程将花费大约 60 分钟时间。

## <a name="next-steps"></a>后续步骤

* [排查 Azure VM 间的连接问题](/virtual-network/virtual-network-troubleshoot-connectivity-problem-between-vms)

<!-- Update_Description: new article about virtual network configure vnet connections -->
<!--ms.date: 09/30/2019-->