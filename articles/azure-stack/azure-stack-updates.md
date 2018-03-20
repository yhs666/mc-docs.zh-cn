---
title: "在 Azure Stack 中管理更新概述 | Microsoft Docs"
description: "了解 Azure Stack 集成系统的更新管理。"
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 9b0781f4-2cd5-4619-a9b1-59182b4a6e43
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 09/25/2017
ms.date: 03/04/2018
ms.author: v-junlch
ms.openlocfilehash: 68f10afbcc34e010ba8f25c40cf903795585f918
ms.sourcegitcommit: 9b5cc262f13a0fc9e0fd9495e3fbb6f394ba1812
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2018
---
# <a name="manage-updates-in-azure-stack-overview"></a>在 Azure Stack 中管理更新概述

*适用于：Azure Stack 集成系统*

Microsoft 会以一般的步调发布 Azure Stack 集成系统的更新包，通常会落在每个月的第四个星期二，从正式上市开始。 请咨询 OEM 了解特定通知过程，以确保组织收到更新通知，或者在 Overview\Release Notes\Integrated Systems 发行说明下的这里查看，了解有关特定版本的详细信息。

每次发布的 Microsoft 软件更新均打包为单个更新包。 作为 Azure Stack 操作员，可以从管理员门户轻松导入和安装这些更新包，并监视这些更新包的安装进度。 

原始设备制造商 (OEM) 硬件供应商也会发布更新，例如驱动程序和固件更新。 这些更新是由 OEM 硬件供应商以单独包的形式提供，并从 Microsoft 更新分别进行管理。

若要保持系统受支持，必须始终将 Azure Stack 更新为特定版本级别。 请务必查看 [Azure Stack 服务策略](azure-stack-servicing-policy.md)。

> [!NOTE]
> 无法将 Azure Stack 更新包应用于 Azure Stack 开发工具包。 更新包专为集成系统所设计。

## <a name="the-update-resource-provider"></a>更新资源提供程序

Azure Stack 包含协调 Microsoft 软件更新应用的更新资源提供程序。 此资源提供程序可确保在所有物理主机、Service Fabric 应用程序和运行时以及所有基础结构虚拟机及其关联的服务上应用更新。

当更新安装时，由于更新进程以 Azure Stack 中的各种子系统（例如，物理主机和基础结构虚拟机）为目标，因此你可以轻松查看高级状态。

## <a name="plan-for-updates"></a>规划更新

我们强烈建议你向用户通知任何维护操作，并尽可能将正常维护时段安排在非工作时间。 维护操作可能会同时影响租户工作负荷和门户操作。

## <a name="using-the-update-tile-to-manage-updates"></a>使用“更新”磁贴管理更新
从管理员门户管理更新是简单的过程。 Azure Stack 操作员可以导航到仪表板中的“更新”磁贴：

- 查看重要信息，例如当前版本。
- 安装更新，并监视进度。
- 查看以前安装的更新的更新历史记录。
 
## <a name="determine-the-current-version"></a>确定当前版本

“更新”磁贴会显示当前的 Azure Stack 版本。 可以在管理员门户中使用下列其中一种方法转到“更新”磁贴：

- 在仪表板上的“更新”磁贴中查看当前版本。
 
   ![默认仪表板上的“更新”磁贴](./media/azure-stack-updates/image1.png)
 
- 在“区域管理”磁贴上，单击区域名称。 在“更新”磁贴中查看当前版本。

## <a name="next-steps"></a>后续步骤

- [Azure Stack 服务策略](azure-stack-servicing-policy.md) 
- [Azure Stack 中的区域管理](azure-stack-region-management.md)     



