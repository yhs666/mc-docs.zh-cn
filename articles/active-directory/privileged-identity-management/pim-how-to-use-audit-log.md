---
title: 在 PIM 中查看 Azure AD 角色的审核历史记录 - Azure Active Directory | Microsoft Docs
description: 了解如何在 Azure AD Privileged Identity Management (PIM) 中查看 Azure AD 角色的审核历史记录。
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
origin.date: 10/22/2019
ms.date: 11/05/2019
ms.author: v-junlch
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: e335aaf68e7b4dbf1b6c3313204bf3d56345e68b
ms.sourcegitcommit: a88cc623ed0f37731cb7cd378febf3de57cf5b45
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/08/2019
ms.locfileid: "73830778"
---
# <a name="view-audit-history-for-azure-ad-roles-in-pim"></a>在 PIM 中查看 Azure AD 角色的审核历史记录

可以使用 Privileged Identity Management (PIM) 审核历史记录来查看过去 30 天内所有特权角色的所有角色分配和激活操作。 若要查看 Azure Active Directory (Azure AD) 组织中活动的完整审核历史记录（包括管理员、最终用户和同步活动），可以使用 [Azure Active Directory 安全和活动报告](../reports-monitoring/overview-reports.md)。

## <a name="view-audit-history"></a>查看审核历史记录

按以下步骤查看 Azure AD 角色的审核历史记录。

1. 使用[特权角色管理员](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator)角色成员用户的身份登录到 [Azure 门户](https://portal.azure.cn/)。

1. 打开“Azure AD Privileged Identity Management”。 

1. 单击“Azure AD 角色”。 

1. 单击“目录角色审核历史记录”。 

    将会根据审核历史记录显示柱形图和总激活数、每日最大激活数以及每日平均激活数。

    ![目录角色审核历史记录](./media/pim-how-to-use-audit-log/directory-roles-audit-history.png)

    在页面底部会显示一个表，其中包含可用审核历史记录中每个操作的信息。 列的含义如下：

    | 列 | 说明 |
    | --- | --- |
    | 时间 | 发生操作的时间。 |
    | 请求者 | 已请求角色激活或更改的用户。 如果该值为“Azure 系统”，请查看 Azure 审核历史记录以获取详细信息。  |
    | 操作 | 请求者所采取的操作。 操作可能包含分配、取消分配、激活、停用或 AddedOutsidePIM。 |
    | 成员 | 正在激活或已分配给角色的用户。 |
    | 角色 | 由用户分配或激活的角色。 |
    | 理由 | 在激活期间向原因字段中输入的文本。 |
    | 过期时间 | 已激活角色过期的时间。 仅适用于符合条件的角色分配。 |

1. 若要对审核历史记录排序，请单击“时间”  、“操作”  和“角色”  按钮。

## <a name="filter-audit-history"></a>筛选审核历史记录

1. 在审核历史记录页顶部，单击“筛选”按钮。 

    将显示“更新图表参数”窗格  。

1. 在“时间范围”中，  选择时间范围。

1. 在**角色**中，选中指示要查看的角色的复选框。

    ![“更新图表参数”窗格](./media/pim-how-to-use-audit-log/update-chart-parameters.png)

1. 单击“完成”，查看已筛选的审核历史记录。 

## <a name="next-steps"></a>后续步骤

- [在 Privileged Identity Management 中查看 Azure 资源角色的活动和审核历史记录](azure-pim-resource-rbac.md)

<!-- Update_Description: wording update -->