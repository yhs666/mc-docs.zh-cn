---
title: 在 Privileged Identity Management 中配置 Azure AD 角色设置 - Azure Active Directory | Microsoft Docs
description: 了解如何在 Azure AD Privileged Identity Management (PIM) 中配置 Azure AD 角色设置。
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
ms.openlocfilehash: 818f8ee71ba8b3a5fc6618699699533e55f9c5c1
ms.sourcegitcommit: a88cc623ed0f37731cb7cd378febf3de57cf5b45
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/08/2019
ms.locfileid: "73830802"
---
# <a name="configure-azure-ad-role-settings-in-privileged-identity-management"></a>在 Privileged Identity Management 中配置 Azure AD 角色设置

特权角色管理员可以在其 Azure Active Directory (Azure AD) 组织中自定义 Privileged Identity Management (PIM)，包括更改激活合格角色分配的用户的体验。

## <a name="open-role-settings"></a>打开角色设置

遵循以下步骤打开 Azure AD 角色的设置。

1. 打开“Azure AD Privileged Identity Management”。 

1. 单击“Azure AD 角色”。 

1. 单击“设置”  。

    ![Azure AD 角色 - 设置](./media/pim-how-to-change-default-settings/pim-directory-roles-settings.png)

1. 单击“角色”。 

1. 单击要配置其设置的角色。

    ![Azure AD 角色 - 设置角色](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-role.png)

    在每个角色的设置页上，有多个可以配置的设置。 这些设置只影响作为**合格**分配（不是**永久**分配）的用户。

## <a name="activations"></a>激活

使用“激活”  滑块设置角色在过期前保持活动状态的最大时间（以小时为单位）。 此值可为 1 到 72 小时。

## <a name="notifications"></a>通知

使用“通知”开关指定当角色激活时管理员是否将收到电子邮件通知  。 此通知对于检测未经授权或非法的激活相当有用。

设置为“启用”时，通知将发送至  ：

- 特权角色管理员
- 安全管理员
- 全局管理员

有关详细信息，请参阅 [Privileged Identity Management 中的电子邮件通知](pim-email-notifications.md)。

## <a name="incidentrequest-ticket"></a>事件/请求票证

使用“事件/请求票证”  开关要求合格管理员在激活其角色时提供票证编号。 这种做法可以使角色访问审核更高效。

## <a name="multi-factor-authentication"></a>多重身份验证

使用“多重身份验证”  开关指定是否要求用户在激活其角色之前，先使用 MFA 验证其身份。 他们只需在每个会话中验证其身份一次，而无需在每次激活角色时都执行身份验证。 启用 MFA 时，请记住两点提示：

- 使用 Microsoft 帐户作为电子邮件地址（通常是 @outlook.com，但不一定）的用户无法注册 Azure 多重身份验证。 如果想要将角色分配给使用 Microsoft 帐户的用户，应将其设置为永久管理员，或者为该角色禁用多重身份验证。
- 无法对 Azure AD 和 Office 365 的高特权角色禁用 Azure 多重身份验证。 此安全功能有助于保护以下角色：  
  
  - Azure 信息保护管理员
  - 计费管理员
  - 云应用程序管理员
  - 法规管理员
  - 条件访问管理员
  - Dynamics 365 管理员
  - 客户密码箱访问审批人
  - 目录写入者
  - Exchange 管理员
  - 全局管理员
  - Intune 管理员
  - Power BI 管理员
  - 特权角色管理员
  - 安全管理员
  - SharePoint 管理员
  - Skype for Business 管理员
  - 用户管理员

有关详细信息，请参阅[多重身份验证和 Privileged Identity Management](pim-how-to-require-mfa.md)。

## <a name="require-approval"></a>需要审批

如果要委派激活角色所需的审批，请按照以下步骤操作。

1. 将“要求批准”  设置为“启用”  。 该窗格扩展选项以选择审批者。

    ![Azure AD 角色 - 设置 - 需要审批](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-require-approval.png)

    如果未指定任何审批者，则特权角色管理员将成为默认的审批者，然后需要审批此角色的所有激活请求。

1. 若要指定审批者，请单击“选择审批者”  。

    ![Azure AD 角色 - 设置 - 需要审批](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-require-approval-select-approvers.png)

1. 除了特权角色管理员之外，还选择一个或多个审批者，然后单击“选择”  。 可以选择用户或组。 建议至少选择两个审批者。 即使将自己添加为审批者，也不能自行批准角色激活。 所选项将出现在所选审批者列表中。

1. 在指定所有角色设置后，选择“保存”  以保存更改。

<!--PLACEHOLDER: Need an explanation of what the temporary Global Administrator setting is for.-->

## <a name="next-steps"></a>后续步骤

- [在 Privileged Identity Management 中分配 Azure AD 角色](pim-how-to-add-role-to-user.md)
- [在 Privileged Identity Management 中为 Azure AD 角色配置安全警报](pim-how-to-configure-security-alerts.md)

