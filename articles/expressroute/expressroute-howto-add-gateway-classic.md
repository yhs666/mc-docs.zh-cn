---
title: "使用 PowerShell 为 ExpressRoute 配置 VNet 网关（经典）：Azure "
description: "在 ExpressRoute 配置中使用 PowerShell 配置经典部署模型 VNet 的 VNet 网关。"
documentationcenter: na
services: expressroute
author: charwen
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: 85ee0bc1-55be-4760-bfb4-34d9f2c96f30
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 03/21/2017
ms.date: 
ms.author: v-yiso
ms.openlocfilehash: 06ddd7a28a7463f963eb1fb4dfdd311960ea8ac0
ms.sourcegitcommit: d5d647d33dba99fabd3a6232d9de0dacb0b57e8f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/14/2017
---
# 使用 PowerShell 配置 ExpressRoute 的虚拟网络网关（经典）
<a id="configure-a-virtual-network-gateway-for-expressroute-using-powershell-classic" class="xliff"></a>
> [!div class="op_single_selector"]
> * [Resource Manager - PowerShell](./expressroute-howto-add-gateway-resource-manager.md)
> * [经典 - PowerShell](./expressroute-howto-add-gateway-classic.md)
>
>

本文将指导你完成为预先存在的 VNet 添加、重设大小和删除虚拟网络 (VNet) 网关的步骤。 此配置的步骤专用于使用**经典部署模型**创建的、将在 ExpressRoute 配置中使用的 VNet。 

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**关于 Azure 部署模型**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## 开始之前
<a id="before-beginning" class="xliff"></a>

确认已安装此配置所需的 Azure PowerShell cmdlet（1.0.2 或更高版本）。 如果尚未安装 cmdlet，必须先安装，然后才能开始执行配置步骤。 有关安装 Azure PowerShell 的详细信息，请参阅 [如何安装和配置 Azure PowerShell](../powershell-install-configure.md)。

[!INCLUDE [expressroute-gateway-classic-ps](../../includes/expressroute-gateway-classic-ps-include.md)]

## 后续步骤
<a id="next-steps" class="xliff"></a>

创建 VNet 网关之后，可以将 VNet 链接到 ExpressRoute 线路。 请参阅[将虚拟网络链接到 ExpressRoute 线路](./expressroute-howto-linkvnet-classic.md)。