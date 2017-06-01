---
title: "将虚拟网络链接到 ExpressRoute 线路：PowerShell：经典：Azure "
description: "本文档概述如何使用经典部署模型和 PowerShell 将虚拟网络 (VNet) 链接到 ExpressRoute 线路。"
services: expressroute
documentationcenter: na
author: ganesr
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: 9b53fd72-9b6b-4844-80b9-4e1d54fd0c17
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/13/2016
ms.author: ganesr
ms.translationtype: Human Translation
ms.sourcegitcommit: 78da854d58905bc82228bcbff1de0fcfbc12d5ac
ms.openlocfilehash: 0ca44f78c13c6f49a0d64839d452b87edd1c0a90
ms.contentlocale: zh-cn
ms.lasthandoff: 04/22/2017

---
# <a name="connect-a-virtual-network-to-an-expressroute-circuit-using-powershell-classic"></a>使用 PowerShell 将虚拟网络连接到 ExpressRoute 线路（经典）
> [!div class="op_single_selector"]
> * [Resource Manager - Azure 门户](./expressroute-howto-linkvnet-portal-resource-manager.md)
> * [Resource Manager - PowerShell](./expressroute-howto-linkvnet-arm.md)
> * [经典 - PowerShell](./expressroute-howto-linkvnet-classic.md)
>
>

本文将帮助使用经典部署模型和 PowerShell 将虚拟网络 (VNet) 链接到 Azure ExpressRoute 线路。 虚拟网络可以在同一个订阅中，也可以属于另一个订阅。

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**关于 Azure 部署模型**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="configuration-prerequisites"></a>配置先决条件

1. 需要 Azure PowerShell 模块的最新版本。 可以从 [Azure 下载页](/downloads/)的 PowerShell 部分下载最新 PowerShell 模块。 有关如何配置计算机以使用 Azure PowerShell 模块的分步指导，请遵循[如何安装和配置 Azure PowerShell](../powershell-install-configure.md) 中的说明。 
2. 在开始配置之前，需要查看[先决条件](./expressroute-prerequisites.md)、[路由要求](./expressroute-routing.md)和[工作流](./expressroute-workflows.md)。
3. 你必须有一个活动的 ExpressRoute 线路。 
    - 请按说明[创建 ExpressRoute 线路](./expressroute-howto-circuit-classic.md)，并让连接提供商启用该线路。
    - 确保为线路配置 Azure 专用对等互连。 有关路由说明，请参阅[配置路由](./expressroute-howto-routing-classic.md)一文。 
    - 确保配置 Azure 专用对等互连，并运行用户网络和 Microsoft 之间的 BGP 对等互连，以便启用端到端连接。
    - 必须已创建并完全预配虚拟网络和虚拟网络网关。 请按说明[为 ExpressRoute 配置虚拟网络](./expressroute-howto-vnet-portal-classic.md)。

最多可以将 10 个虚拟网络链接到 ExpressRoute 线路。 所有虚拟网络都必须位于同一地缘政治区域。 如果已启用 ExpressRoute 高级外接程序，则可以将更多虚拟网络链接到 ExpressRoute 线路，或者链接其他地缘政治区域中的虚拟网络。 有关高级外接程序的更多详细信息，请参阅[常见问题解答](./expressroute-faqs.md)。

## <a name="connect-a-virtual-network-in-the-same-subscription-to-a-circuit"></a>将同一订阅中的虚拟网络连接到线路
可以使用以下 cmdlet 将虚拟网络链接到 ExpressRoute 线路。 在运行 cmdlet 之前，请确保已创建虚拟网络网关并可将其用于进行链接。

    New-AzureDedicatedCircuitLink -ServiceKey "*****************************" -VNetName "MyVNet"
    Provisioned

## <a name="connect-a-virtual-network-in-a-different-subscription-to-a-circuit"></a>将不同订阅中的虚拟网络连接到线路
用户可以在多个订阅之间共享 ExpressRoute 线路。 下图是在多个订阅之间共享 ExpressRoute 线路的简单示意图。

大型云中的每个较小云用于表示属于组织中不同部门的订阅。 组织内的每个部门可以使用自己的订阅部署其服务，但这些部门可以共享单个 ExpressRoute 线路以连接回本地网络。 单个部门（在此示例中为 IT 部门）可以拥有 ExpressRoute 线路。 组织内的其他订阅可以使用 ExpressRoute 线路。

> [!NOTE]
> 将对 ExpressRoute 线路所有者收取专用线路的连接和带宽费用。 所有虚拟网络共享相同的带宽。
> 
> 

![跨订阅连接](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)

### <a name="administration"></a>管理
*线路所有者* 是在其中创建 ExpressRoute 线路的订阅的管理员/共同管理员。 线路所有者可以授权其他订阅的管理员/共同管理员（称为 *线路用户*）使用他们拥有的专用线路。 有权使用组织的 ExpressRoute 线路的线路用户，在获得授权后可以将其订阅中的虚拟网络链接到 ExpressRoute 线路。

线路所有者有权随时修改和撤消授权。 撤消授权将导致从已撤消其访问权限的订阅中删除所有链接。

### <a name="circuit-owner-operations"></a>线路所有者操作

**创建授权**

线路所有者可授权其他订阅的管理员使用指定的线路。 在下面的示例中，线路 (Contoso IT) 管理员允许另一个订阅（开发-测试）的管理员最多将两个虚拟网络链接到线路。 Contoso IT 管理员可以通过指定开发-测试 Microsoft ID 启用此功能。 该 cmdlet 不会将电子邮件发送到指定的 Microsoft ID。 线路所有者需要显式通知其他订阅所有者：授权已完成。

    New-AzureDedicatedCircuitLinkAuthorization -ServiceKey "**************************" -Description "Dev-Test Links" -Limit 2 -MicrosoftIds 'devtest@contoso.com'

    Description         : Dev-Test Links
    Limit               : 2
    LinkAuthorizationId : **********************************
    MicrosoftIds        : devtest@contoso.com
    Used                : 0
```

**Reviewing authorizations**

The circuit owner can review all authorizations that are issued on a particular circuit by running the following cmdlet:

    Get-AzureDedicatedCircuitLinkAuthorization -ServiceKey: "**************************"

    Description         : EngineeringTeam
    Limit               : 3
    LinkAuthorizationId : ####################################
    MicrosoftIds        : engadmin@contoso.com
    Used                : 1

    Description         : MarketingTeam
    Limit               : 1
    LinkAuthorizationId : @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
    MicrosoftIds        : marketingadmin@contoso.com
    Used                : 0

    Description         : Dev-Test Links
    Limit               : 2
    LinkAuthorizationId : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
    MicrosoftIds        : salesadmin@contoso.com
    Used                : 2


**Updating authorizations**

The circuit owner can modify authorizations by using the following cmdlet:

    Set-AzureDedicatedCircuitLinkAuthorization -ServiceKey "**************************" -AuthorizationId "&&&&&&&&&&&&&&&&&&&&&&&&&&&&"-Limit 5

    Description         : Dev-Test Links
    Limit               : 5
    LinkAuthorizationId : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
    MicrosoftIds        : devtest@contoso.com
    Used                : 0


**Deleting authorizations**

The circuit owner can revoke/delete authorizations to the user by running the following cmdlet:

    Remove-AzureDedicatedCircuitLinkAuthorization -ServiceKey "*****************************" -AuthorizationId "###############################"


### Circuit user operations

**Reviewing authorizations**

The circuit user can review authorizations by using the following cmdlet:

    Get-AzureAuthorizedDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : ContosoIT
    Location                         : Washington DC
    MaximumAllowedLinks              : 2
    ServiceKey                       : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled
    UsedLinks                        : 0

**Redeeming link authorizations**

The circuit user can run the following cmdlet to redeem a link authorization:

    New-AzureDedicatedCircuitLink –servicekey "&&&&&&&&&&&&&&&&&&&&&&&&&&" –VnetName 'SalesVNET1'

    State VnetName
    ----- --------
    Provisioned SalesVNET1

## Next steps

For more information about ExpressRoute, see the [ExpressRoute FAQ](./expressroute-faqs.md).
