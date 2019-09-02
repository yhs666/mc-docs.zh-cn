---
title: 将 Azure 网络资源移到新的订阅或资源组 | Azure
description: 使用 Azure 资源管理器将虚拟网络和其他网络资源移到新的资源组或订阅。
author: rockboyfor
ms.service: azure-resource-manager
ms.topic: conceptual
origin.date: 08/19/2019
ms.date: 08/26/2019
ms.author: v-yeche
ms.openlocfilehash: fb695b02275a998e0badbbeda2204a54de8de3b4
ms.sourcegitcommit: 599d651afb83026938d1cfe828e9679a9a0fb69f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/23/2019
ms.locfileid: "69993773"
---
<!--Verify successfully-->

# <a name="move-guidance-for-networking-resources"></a>网络资源移动指南

本文介绍如何针对特定方案移动虚拟网络和其他网络资源。

## <a name="dependent-resources"></a>依赖资源

移动虚拟网络时，还必须移动其从属资源。 对于 VPN 网关，必须移动 IP 地址、虚拟网络网关和所有关联的连接资源。 本地网络网关可以位于不同的资源组中。

若要移动带网络接口卡的虚拟机，必须移动所有依赖的资源。 移动与该网络接口卡对应的虚拟网络、该虚拟网络的所有其他网络接口卡，以及 VPN 网关。

## <a name="state-of-dependent-resources"></a>依赖资源的状态

如果源或目标资源组包含虚拟网络，则会在移动过程中检查虚拟网络的所有依赖资源的状态。 如果这些资源中有任何资源处于故障状态，则会阻止移动。 例如，如果某个使用虚拟网络的虚拟机故障，则会阻止移动。 即使该虚拟机不是要移动的资源之一，也不在要移动的资源组之一中，系统也会阻止移动。 为了避免此问题，请将资源移到没有虚拟网络的资源组。

## <a name="peered-virtual-network"></a>对等的虚拟网络

若要移动对等的虚拟网络，必须首先禁用虚拟网络对等互连。 在禁用后，可以移动虚拟网络。 在移动后，重新启用虚拟网络对等互连。

## <a name="subnet-links"></a>子网链接

如果虚拟网络的任何子网包含资源导航链接，则无法将虚拟网络移动到其他订阅。 例如，如果 Azure Redis 缓存资源部署到某个子网，则该子网具有资源导航链接。

## <a name="next-steps"></a>后续步骤

有关用于移动资源的命令，请参阅[将资源移到新资源组或订阅](../resource-group-move-resources.md)。

<!-- Update_Description: new article about networking move limitation -->
<!--ms.date: 08/26/2019-->