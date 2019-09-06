---
title: 重置用户的密码 - Azure Active Directory | Microsoft Docs
description: 有关如何使用 Azure Active Directory 重置用户密码的说明。
services: active-directory
author: msaburnley
manager: daveba
ms.assetid: fad5624b-2f13-4abc-b3d4-b347903a8f16
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
origin.date: 09/05/2018
ms.date: 08/27/2019
ms.author: v-junlch
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 07e78d8e5ab845cbfb3aec6e352b4a7ccc4c31bc
ms.sourcegitcommit: 18a0d2561c8b60819671ca8e4ea8147fe9d41feb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2019
ms.locfileid: "70134183"
---
# <a name="reset-a-users-password-using-azure-active-directory"></a>使用 Azure Active Directory 重置用户的密码

如果忘记密码、设备遭锁定用户无法使用，或用户从未收到密码，管理员可以重置用户的密码。

>[!Note]
>除非你的 Azure AD 租户是用户的主目录，否则你无法重置用户的密码。 这意味着，如果用户使用另一个组织中的帐户、Microsoft 帐户登录到你的组织，则你无法重置其密码。<br><br>如果用户使用外部 Azure AD 作为授权来源，则你无法重置密码。 只有用户或外部 Azure AD 中的管理员可以重置密码。

>[!Note]
>如果你不是管理员，只想获得有关如何重置你自己的工作或学校密码的说明，请参阅[重置工作或学校密码](../user-help/active-directory-passwords-update-your-own-password.md)。

## <a name="to-reset-a-password"></a>重置密码

1. 以用户管理员或密码管理员身份登录到 [Azure 门户](https://portal.azure.cn/)。 有关可用角色的详细信息，请参阅[在 Azure Active Directory 中分配管理员角色](../users-groups-roles/directory-assign-admin-roles.md#available-roles)

2. 选择“Azure Active Directory”  ，选择“用户”  ，搜索并选择需要重置的用户，然后选择“重置密码”。 

    此时将显示“Alain Charon - 个人资料”  页面，其中包含“重置密码”  选项。

    ![用户的个人资料页面，其中突出显示了“重置密码”选项](./media/active-directory-users-reset-password-azure-portal/user-profile-reset-password-link.png)

3. 在“重置密码”  页面中，选择“重置密码”  。

    > [!Note]
    > 使用 Azure Active Directory 时，会自动为用户生成临时密码。 使用本地 Active Directory 时，会为用户创建密码。

4. 复制该密码并将其提供给用户。 用户在下次登录过程中需要更改密码。

    >[!Note]
    >临时密码永不过期。 用户下次登录时，密码仍将有效，不管从生成临时密码后已经过去了多长时间。

## <a name="next-steps"></a>后续步骤

重置用户的密码后，可以执行以下基本流程：

- [添加或删除用户](add-users-azure-active-directory.md)

- [向用户分配角色](active-directory-users-assign-role-azure-portal.md)

- [添加或更改个人资料信息](active-directory-users-profile-azure-portal.md)

- [创建基本组并添加成员](active-directory-groups-create-azure-portal.md)

还可以执行更复杂的用户方案，例如，分配委托、使用策略，以及共享用户帐户。 有关其他可用操作的详细信息，请参阅 [Azure Active Directory 用户管理和文档](../users-groups-roles/index.yml)。

<!-- Update_Description: wording update -->