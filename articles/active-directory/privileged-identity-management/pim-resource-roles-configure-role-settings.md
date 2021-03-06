---
title: 在 PIM 中配置 Azure 资源角色设置 - Azure AD | Microsoft Docs
description: 了解如何在 Azure AD Privileged Identity Management (PIM) 中配置 Azure 资源角色设置。
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
origin.date: 11/08/2019
ms.date: 11/28/2019
ms.author: v-junlch
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: cda7db6d1fc760fa887b3b325f97129b32f09898
ms.sourcegitcommit: 9597d4da8af58009f9cef148a027ccb7b32ed8cf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/28/2019
ms.locfileid: "74655412"
---
# <a name="configure-azure-resource-role-settings-in-privileged-identity-management"></a>在 Privileged Identity Management 中配置 Azure 资源角色设置

配置 Azure 资源角色设置时，可定义应用于 Azure Active Directory (Azure AD) Privileged Identity Management (PIM) 中的 Azure 资源角色分配的默认设置。 使用以下步骤配置审批工作流并指定谁可以批准或拒绝请求。

## <a name="open-role-settings"></a>打开角色设置

请遵循以下步骤打开 Azure 资源角色的设置。

1. 使用具有[特权角色管理员](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator)角色的用户登录到 [Azure 门户](https://portal.azure.cn/)。

1. 打开“Azure AD Privileged Identity Management”。 

1. 选择“Azure 资源”  。

1. 选择要管理的资源，如订阅或管理组。

    ![列出可以管理的资源的“Azure 资源”页](./media/pim-resource-roles-configure-role-settings/resources-list.png)

1. 选择  “角色设置”。

    ![列出 Azure 资源角色的“角色设置”页](./media/pim-resource-roles-configure-role-settings/resources-role-settings.png)

1. 选择要配置其设置的角色。

    ![列出多个分配和激活设置的“角色设置详细信息”页](./media/pim-resource-roles-configure-role-settings/resources-role-setting-details.png)

1. 选择“编辑”打开“角色设置”窗格。   第一个选项卡用于更新在 Privileged Identity Management 中激活角色所需的配置。

    ![“编辑角色设置”页，其中的“激活”选项卡已打开](./media/pim-resource-roles-configure-role-settings/role-settings-activation-tab.png)

1. 选择“分配”  选项卡或页面底部的“下一步:  分配”按钮，打开分配设置选项卡。这些设置控制在 Privileged Identity Management 界面中进行的角色分配。

    ![角色设置页中的角色分配表](./media/pim-resource-roles-configure-role-settings/role-settings-assignment-tab.png)

1. 使用“通知”选项卡或页面底部的  “下一步:  激活”按钮即可转到此角色的通知设置选项卡。 这些设置控制与此角色相关的所有电子邮件通知。

    ![角色设置页中的角色“通知”选项卡](./media/pim-resource-roles-configure-role-settings/role-settings-notification-tab.png)

1. 随时选择“更新”按钮，对角色设置进行更新。 

## <a name="assignment-duration"></a>分配持续时间

配置角色的设置时，可以从用于每种分配类型（合格和活动）的两个分配持续时间选项中进行选择·。 在 Privileged Identity Management 中将用户分配到角色时，这些选项将成为默认的最大持续时间。

可以选择其中一个合格的  分配持续时间选项：

| | |
| --- | --- |
| **允许永久的合格分配** | 资源管理员可以分配永久的合格分配。 |
| **使合格分配在以下时间后过期** | 资源管理员可以要求所有合格分配都具有指定的开始和结束日期。 |

并且，可以选择其中一个活动  分配持续时间选项：

| | |
| --- | --- |
| **允许永久的活动分配** | 资源管理员可以分配永久的活动分配。 |
| **使活动分配在以下时间后过期** | 资源管理员可以要求所有活动分配都具有指定的开始和结束日期。 |

> [!NOTE]
> 资源管理员可续订具有特定结束日期的所有分配。 此外，用户也可启动自助服务请求来[扩展或续订角色分配](pim-resource-roles-renew-extend.md)。

## <a name="require-multi-factor-authentication"></a>需要多重身份验证

Privileged Identity Management 提供了两种不同的可选 Azure 多重身份验证强制执行方案。

### <a name="require-multi-factor-authentication-on-active-assignment"></a>要求在活动分配时进行多重身份验证

在某些情况下，你可能希望为用户或组分配短期（例如，一天）角色。 在这种情况下，分配的成员不需要请求激活。 在这种情况下，Privileged Identity Management 无法在用户使用其角色分配时强制实施多重身份验证，因为从分配角色时起，用户就已经在角色中处于活动状态。

为确保完成分配的资源管理员是其本人，可以通过选中“在活动分配时要求进行多重身份验证”  框来对活动分配强制执行多重身份验证。

### <a name="require-multi-factor-authentication-on-activation"></a>要求在激活时进行多重身份验证

可以要求符合角色条件的用户证明他们正在使用 Azure 多重身份验证，然后他们才能激活。 多重身份验证能够以合理的确定性确保用户是其本人。 强制执行此选项可以在用户帐户可能已遭入侵的情况下保护关键资源。

若要在激活前要求进行多重身份验证，请选中“在激活时要求进行多重身份验证”  框。

有关详细信息，请参阅[多重身份验证和 Privileged Identity Management](pim-how-to-require-mfa.md)。

## <a name="activation-maximum-duration"></a>最长激活持续时间

使用“最长激活持续时间”  滑块是角色在过期前保持活动状态的最大时间（以小时为单位）。 此值可以是 1 到 24 个小时。

## <a name="require-justification"></a>需要理由

你可以要求用户在激活时输入业务理由。 若需要理由，请选中“在活动分配时需要理由”  框或“在激活时需要理由”  框。

## <a name="require-approval-to-activate"></a>需要批准才能激活

如果要求批准以激活角色，请按照以下步骤操作。

1. 选中“需要批准以激活”  复选框。

1. 选择“选择审批者”打开“选择成员或组”页   。

    ![用于选择审批者的“选择用户或组”窗格](./media/pim-resource-roles-configure-role-settings/resources-role-settings-select-approvers.png)

1. 至少选择一个用户或组，然后单击“选择”  。 可以添加任何用户和组的组合。 必须至少选择 1 个审批者。 没有默认的审批者。

    所选项将出现在所选审批者列表中。

1. 在指定所有角色设置后，选择“更新”  以保存更改。

## <a name="next-steps"></a>后续步骤

- [在 Privileged Identity Management 中分配 Azure 资源角色](pim-resource-roles-assign-roles.md)
- [在 Privileged Identity Management 中为 Azure 资源角色配置安全警报](pim-resource-roles-configure-alerts.md)

