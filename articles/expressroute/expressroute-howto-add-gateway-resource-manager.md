---
title: '向 ExpressRoute 的 VNet 添加虚拟网络网关：PowerShell：Azure '
description: 本文介绍如何将 VNet 网关添加到为 ExpressRoute 创建的 Resource Manager VNet。
documentationcenter: na
services: expressroute
author: charwen
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: 63e0bd60-abad-4963-8e27-3aa973e0d968
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 02/21/2019
ms.author: v-yiso
ms.date: 04/01/2019
ms.openlocfilehash: 17ed79254cd10b93be63e085681d60f580bf029f
ms.sourcegitcommit: 41a1c699c77a9643db56c5acd84d0758143c8c2f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2019
ms.locfileid: "58348529"
---
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-powershell"></a>使用 PowerShell 配置 ExpressRoute 的虚拟网络网关
> [!div class="op_single_selector"]
> * [Resource Manager - Azure 门户](./expressroute-howto-add-gateway-portal-resource-manager.md)
> * [Resource Manager - PowerShell](./expressroute-howto-add-gateway-resource-manager.md)
> * [经典 - PowerShell](./expressroute-howto-add-gateway-classic.md)
>
>

本文指导你为预先存在的 VNet 添加虚拟网络 (VNet) 网关、重设网关大小以及删除网关。 此配置的步骤适用于使用资源管理器部署模型创建的 VNet（针对 ExpressRoute 配置）。 有关详细信息，请参阅[关于 ExpressRoute 的虚拟网络网关](expressroute-about-virtual-network-gateways.md)。

## <a name="before-beginning"></a>开始之前

### <a name="working-with-powershell"></a>使用 PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

### <a name="configuration-reference-list"></a>配置参考列表
[!INCLUDE [expressroute-gateway-rm-ps](../../includes/expressroute-gateway-rm-ps-include.md)]

## <a name="next-steps"></a>后续步骤

创建 VNet 网关之后，可以将 VNet 链接到 ExpressRoute 线路。 请参阅[将虚拟网络链接到 ExpressRoute 线路](./expressroute-howto-linkvnet-arm.md)。