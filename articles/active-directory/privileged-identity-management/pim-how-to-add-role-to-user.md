---
title: 在 Privileged Identity Management 中分配 Azure AD 角色 - Azure Active Directory | Microsoft Docs
description: 了解如何在 Azure AD Privileged Identity Management (PIM) 中分配 Azure AD 角色。
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
origin.date: 10/22/2019
ms.date: 11/05/2019
ms.author: v-junlch
ms.collection: M365-identity-device-management
ms.openlocfilehash: c2b400eff25f7d5e6383eb08b5bea218cab7f2a1
ms.sourcegitcommit: a88cc623ed0f37731cb7cd378febf3de57cf5b45
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/08/2019
ms.locfileid: "73830819"
---
# <a name="assign-azure-ad-roles-in-privileged-identity-management"></a>在 Privileged Identity Management 中分配 Azure AD 角色

使用 Azure Active Directory (Azure AD)，全局管理员可以完成**永久性的** Azure AD 管理员角色分配。 可以使用 [Azure 门户](../users-groups-roles/directory-assign-admin-roles.md)或 [PowerShell 命令](https://docs.microsoft.com/powershell/module/azuread#directory_roles)创建这些角色分配。

Azure AD Privileged Identity Management (PIM) 服务还允许特权角色管理员完成永久性的管理员角色分配。 此外，特权角色管理员可将用户设置为 Azure AD 管理员角色的**合格**用户。 符合条件的管理员可在需要时激活角色，在完成任务后，其权限随即失效。

## <a name="make-a-user-eligible-for-a-role"></a>使用户符合角色的条件

遵循以下步骤可使用户符合 Azure AD 管理员角色的条件。

1. 使用[特权角色管理员](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator)角色成员用户的身份登录到 [Azure 门户](https://portal.azure.cn/)。

    有关如何授予其他管理员访问权限以管理 Privileged Identity Management 的信息，请参阅[授予其他管理员访问权限以管理 Privileged Identity Management](pim-how-to-give-access-to-pim.md)。

1. 打开“Azure AD Privileged Identity Management”。 

    如果尚未在 Azure 门户中启动 Privileged Identity Management，请转到[开始使用 Privileged Identity Management](pim-getting-started.md)。

1. 选择“Azure AD 角色”  。

1. 选择“角色”  或“成员”  。

    ![Azure AD 角色，其中突出显示了“角色和成员”菜单选项](./media/pim-how-to-add-role-to-user/pim-directory-roles.png)

1. 选择“添加成员”打开“添加受管理成员”。 

1. 依次选择“选择角色”、要管理的角色、“选择”。  

    ![“选择角色”窗格，列出了 Azure AD 角色](./media/pim-how-to-add-role-to-user/pim-select-a-role.png)

1. 依次选择“选择成员”  、要分配给角色的用户、“选择”  。

    ![“选择成员”窗格，可以在其中选择用户](./media/pim-how-to-add-role-to-user/pim-select-members.png)

1. 在“添加受管理成员”中，选择“确定”将该用户添加到角色。 

1. 在角色列表中，选择刚刚分配的角色以查看成员列表。

     分配角色后，选择的用户将显示在**符合**该角色条件的成员列表中。

    ![列出了角色的成员及其激活状态](./media/pim-how-to-add-role-to-user/pim-directory-role-eligible.png)

1. 用户符合角色的条件后，请告诉他们，可以按照[在 Privileged Identity Management 中激活 Azure AD 角色](pim-how-to-activate-role.md)中的说明来激活该角色。

    符合条件的管理员在激活期间需要注册 Azure 多重身份验证 (MFA)。 如果用户无法注册 MFA 或使用 Microsoft 帐户（通常是 @outlook.com），则需要将其设置为永久充当其角色。

## <a name="make-a-role-assignment-permanent"></a>将角色分配设为永久

默认情况下，新用户只符合 Azure AD 管理员角色的条件。 若要将某个角色分配设为永久，请执行以下步骤。

1. 打开“Azure AD Privileged Identity Management”。 

1. 选择“Azure AD 角色”  。

1. 选择“成员”  。

    ![Azure AD 角色 - 显示角色和激活状态的“成员”列表](./media/pim-how-to-add-role-to-user/pim-directory-role-list-members.png)

1. 选择要设为永久的**符合条件**的角色。

1. 依次选择“更多”  、“永久保留”  。

    ![一个窗格，列出了一个符合角色资格的用户，“更多”菜单选项处于打开状态](./media/pim-how-to-add-role-to-user/pim-make-perm.png)

    该角色现在会列为**永久**角色。

    ![“成员”列表，显示角色和现在已永久化的激活状态](./media/pim-how-to-add-role-to-user/pim-directory-role-list-members-permanent.png)

## <a name="remove-a-user-from-a-role"></a>从角色中删除用户

可将用户从角色分配中删除，但始终必须至少保留一个永久的全局管理员用户。 

按以下步骤从 Azure AD 管理员角色中删除特定的用户。

1. 打开“Azure AD Privileged Identity Management”。 

1. 选择“Azure AD 角色”  。

1. 选择“成员”  。

    ![Azure AD 角色 - 显示角色和激活状态的“成员”列表](./media/pim-how-to-add-role-to-user/pim-directory-role-list-members.png)

1. 选择要删除的角色分配。

1. 依次选择“更多”  、“删除”  。

    ![一个窗格，列出了一个具有永久角色的用户，其中的“更多”菜单选项处于打开状态](./media/pim-how-to-add-role-to-user/pim-remove-role.png)

1. 当系统要求你确认操作时，请选择“是”  。

    ![一条消息，询问你是否要从角色中删除成员](./media/pim-how-to-add-role-to-user/pim-remove-role-confirm.png)

    随即会删除该角色分配。

## <a name="authorization-error-when-assigning-roles"></a>分配角色时出现授权错误

方案：作为 Azure 资源的活动所有者或用户访问管理员，可以查看 Privileged Identity Management 中的资源，但不能执行任何操作，例如进行符合条件的分配或从资源概述页查看角色分配列表。 其中任何操作都会导致授权错误。

若要分配角色，必须向 MS-PIM 服务主体分配 Azure 基于角色的访问控制中的[“用户访问管理员”角色](../../role-based-access-control/built-in-roles.md#user-access-administrator)（与 Azure AD 管理角色相对）。 不需要等待 MS-PIM 被分配“用户访问管理员”角色，你可以手动分配该角色。

以下步骤向订阅的 MS-PIM 服务主体分配“用户访问管理员”角色。

1. 以 Azure AD 组织中的全局管理员身份登录 [Azure 门户](https://portal.azure.cn)。

1. 选择“所有服务”  ，然后选择“订阅”  。

1. 选择订阅。

1. 选择“访问控制(IAM)”  。

1. 选择“角色分配”  ，以在订阅范围查看角色分配的当前列表。

   ![订阅的“访问控制(IAM)”边栏选项卡](./media/pim-how-to-add-role-to-user/ms-pim-access-control.png)

1. 检查 **MS-PIM** 服务主体是否已分配有“用户访问管理员”  角色。

1. 如果不是，则选择“添加角色分配”以打开“添加角色分配”窗格   。

1. 在“角色”  下拉列表中，选择“用户访问管理员”  角色。

1. 在“选择”  列表中，找到并选择“MS-PIM”  服务主体。

   ![“添加角色分配”窗格 - 为 MS-PIM 服务主体添加权限](./media/pim-how-to-add-role-to-user/ms-pim-add-permissions.png)

1. 选择“保存”  以分配角色。

   过一会后，MS-PIM 服务主体将分配有在订阅范围内的“用户访问管理员”角色。

   ![“访问控制(标识和访问管理)”边栏选项卡，显示 MS-PIM 的用户访问管理员角色分配](./media/pim-how-to-add-role-to-user/ms-pim-user-access-administrator.png)

## <a name="next-steps"></a>后续步骤

- [在 Privileged Identity Management 中配置 Azure AD 管理员角色设置](pim-how-to-change-default-settings.md)
- [在 Privileged Identity Management 中分配 Azure 资源角色](pim-resource-roles-assign-roles.md)

<!-- Update_Description: wording update -->