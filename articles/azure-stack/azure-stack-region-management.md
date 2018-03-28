---
title: Azure Stack 中的区域管理 | Microsoft Docs
description: Azure Stack 中的区域管理概述。
services: azure-stack
documentationcenter: ''
author: efemmano
manager: dsavage
editor: ''
ms.assetid: e94775d5-d473-4c03-9f4e-ae2eada67c6c
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 09/25/2017
ms.date: 03/02/2018
ms.author: v-junlch
ms.openlocfilehash: 16c91a17b866970a6207315c1673ac99c2e8191b
ms.sourcegitcommit: 9b5cc262f13a0fc9e0fd9495e3fbb6f394ba1812
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2018
---
# <a name="region-management-in-azure-stack"></a>Azure Stack 中的区域管理

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

Azure Stack 具有区域的概念，这些区域是由构成 Azure Stack 基础结构的硬件资源组成的逻辑实体。 在区域管理内，可以找到成功操作 Azure Stack 基础结构生命周期所需的所有资源。

一个集成系统部署（称为 *Azure Stack 云*）可构成一个区域。 每个 Azure Stack 开发工具包都有一个名为 **local** 的区域。 如果部署第二个 Azure Stack 集成系统，或者在单独的硬件上设置开发工具包的另一个实例，则此 Azure Stack 云为不同的区域。

## <a name="information-available-through-the-region-management-tile"></a>通过区域管理磁贴提供的信息
Azure Stack 在“区域管理”磁贴中提供了一组区域管理功能。 Azure Stack 操作员可在管理员门户的默认仪表板上访问此磁贴。 可以通过此磁贴来监视和更新 Azure Stack 区域和及其区域特定的组件。

 ![“区域管理”磁贴](./media/azure-stack-manage-region/image1.png)

 如果单击“区域管理”磁贴中的一个区域，可以访问以下信息：

  ![“区域管理”边栏选项卡上的窗格的说明](./media/azure-stack-manage-region/image2.png)

1. **资源菜单**。 可以在这里访问特定的基础结构管理领域，以及查看和管理用户资源，例如存储帐户和虚拟网络。

2. **警报**。 此磁贴会列出全系统警报，并提供有关每个警报的详细信息。

3. **更新**。 在此磁贴中，可以查看 Azure Stack 基础结构的当前版本。

4. **资源提供程序**。 通过“资源提供程序”可管理运行 Azure Stack 时所需的组件所提供的租户功能。 每个资源提供程序均附带管理体验。 这项体验可能包括特定提供程序的警报、指标和其他特定于资源提供程序的管理功能。
 
5. **基础结构角色**。 基础结构角色是运行 Azure Stack 时所需的组件。 仅报告警报的基础结构角色会列出。 通过单击角色，可以查看与特定角色关联的警报，以及运行此角色的角色实例。 尽管包含启动、重新启动或关闭基础结构角色实例的功能，但请**勿**在开发工具包环境中执行此操作。 这些选项仅专为每个基础结构角色具有多个角色实例的多节点环境而设计。 在开发工具包中重新启动角色实例（特别是 AzS-Xrp01）会导致系统不稳定。

## <a name="next-steps"></a>后续步骤
[在 Azure Stack 中监视运行状况和警报](azure-stack-monitor-health.md)

[在 Azure Stack 中管理更新](azure-stack-updates.md)







