---
title: 采用 Azure ExpressRoute 所要满足的先决条件 | Azure
description: 本页提供了在订购 Azure ExpressRoute 线路之前需要满足的要求列表。
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: ''
ms.assetid: f872d25e-acfd-405d-9d1b-dcb9f323a2ff
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 03/28/2018
ms.date: ''
ms.author: v-yiso
ms.openlocfilehash: d3d5306977a5866b2b6fda40d4c68eee05f98099
ms.sourcegitcommit: 98c7d04c66f18b26faae45f2406a2fa6aac39415
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/03/2018
ms.locfileid: "39486971"
---
# <a name="expressroute-prerequisites--checklist"></a>ExpressRoute 先决条件和清单  

若要使用 ExpressRoute 连接到 Microsoft 云服务，需确认是否符合以下部分中所列的要求。

[!INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

## <a name="azure-account"></a>Azure 帐户
* 使用中的有效 Microsoft Azure 帐户。 需有此帐户才能设置 ExpressRoute 线路。 ExpressRoute 线路是 Azure 订阅中的资源。 即使连接限于非 Azure Microsoft 云服务（例如 Office 365 服务和 Dynamics 365），Azure 订阅也是一个必要条件。
* 有效的 Office 365 订阅（如果使用的是 Office 365 服务）。 有关详细信息，请参阅本文中的 [Office 365 的具体要求](#office-365-specific-requirements) 部分。

## <a name="connectivity-provider"></a>连接服务提供商
- 可使用 [ExpressRoute 连接合作伙伴](./expressroute-locations.md#partners) 连接到 Microsoft 云。 可以通过[三种方法](./expressroute-introduction.md)在本地网络与 Microsoft 之间设置连接。 
- 即使提供商不是 ExpressRoute 连接合作伙伴，也可通过[云交换提供商](./expressroute-locations.md#nonpartners)连接到 Microsoft 云。

## <a name="network-requirements"></a>网络要求
- **冗余连接**：对于客户与提供商之间的物理连接，没有冗余方面的要求。 即使只有 [一个到云交换的物理连接](./expressroute-faqs.md#onep2plink)，Microsoft 也会要求在 Microsoft 的路由器和对等互连路由器之间建立冗余的 BGP 会话。 
- 路由：你或你的提供商需设置和管理针对[路由域](./expressroute-circuit-peerings.md)的 BGP 会话，具体取决于连接到 Microsoft 云的方式。 某些以太网连接服务提供商或云交换服务提供商可能会以增值服务的形式提供 BGP 管理。



## <a name="next-steps"></a>后续步骤

- 有关 ExpressRoute 的详细信息，请参阅 [ExpressRoute 常见问题](./expressroute-faqs.md)。
- 查找 ExpressRoute 连接服务提供商。 请参阅 [ExpressRoute 合作伙伴和对等位置](./expressroute-locations.md)。
- 请参阅[路由](./expressroute-routing.md)的要求。
- 配置 ExpressRoute 连接。
  - [创建 ExpressRoute 线路](expressroute-howto-circuit-arm.md)
  - [配置路由](expressroute-howto-routing-arm.md)
  - [将 VNet 链接到 ExpressRoute 线路](expressroute-howto-linkvnet-arm.md)
    
<!--Update_Description:update meta properties only-->    