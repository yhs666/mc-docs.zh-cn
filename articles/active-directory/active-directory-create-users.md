---
title: "向 Azure Active Directory 添加新用户 | Microsoft Docs"
description: "说明如何在 Azure Active Directory 中添加新用户或更改用户信息。"
services: active-directory
documentationcenter: 
author: alexchen2016
manager: digimobile
editor: 
ms.assetid: e3673727-6bec-4fdc-87a4-d65b213c4c3c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 07/26/2017
ms.date: 08/22/2017
ms.author: v-junlch
ms.reviewer: jeffsta
ms.custom: oldportal;it-pro;
robots: NOINDEX
ms.openlocfilehash: 922fdee622d8ab2ecbb2b70ace7464af60d84553
ms.sourcegitcommit: d43bb3a378692299062777197a5a030daa9d67d1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/11/2017
---
# <a name="add-new-users-or-users-with-microsoft-accounts-to-azure-active-directory"></a>向 Azure Active Directory 添加新用户或具有 Microsoft 帐户的用户
请添加用户以填充目录。 本文说明如何在组织中添加新用户，以及如何添加具有 Microsoft 帐户的用户。 有关在 Azure Active Directory 中添加来自其他目录的用户或添加来自合作伙伴公司的用户的详细信息，请参阅 [在 Azure Active Directory 中添加来自其他目录或合作伙伴公司的用户](active-directory-create-users-external.md)。 默认情况下，添加的用户没有管理员权限，但你随时可以向他们分配角色。

## <a name="add-a-user"></a>添加用户
1. 使用充当目录全局管理员的帐户登录到 [Azure 经典管理门户](https://manage.windowsazure.cn) 。
2. 选择“Active Directory” ，并选择组织目录的名称。
3. 选择“用户”选项卡，然后在命令栏中选择“添加用户”。
4. 在“告诉我们有关此用户的信息”页上的“用户类型”下，选择下列其中一项：

   - **组织中的新用户** - 在目录中添加新的用户帐户。
   - **具有现有 Microsoft 帐户的用户** - 将现有 Microsoft 使用者帐户添加到目录（例如 Outlook 帐户）
5. 根据“用户类型”输入用户名（适用于新用户）或电子邮件地址（适用于具有 Microsoft 帐户的用户）。
6. 在用户的“配置文件”页上，提供名字和姓氏、用户友好名称，并从“角色”列表中选择用户角色。 有关用户和管理员角色的详细信息，请参阅[在 Azure AD 中分配管理员角色](active-directory-assign-admin-roles.md)。 指定是否要为用户**启用多重身份验证**。
7. 在“获取临时密码”页上，选择“创建”。

> [!IMPORTANT]
> 如果所在的组织使用多个域，在添加用户帐户时你应知道以下问题：
>
> * 例如，若要跨域添加具有相同用户主体名称 (UPN) 的用户帐户，请**先**添加 geoffgrisso@contoso.partner.onmschina.cn，**再**添加 geoffgrisso@contoso.com。
> * **不要**在添加 geoffgrisso@contoso.partner.onmschina.cn 之前添加 geoffgrisso@contoso.com。 此顺序非常重要，事后想要撤消操作将很麻烦。
>
>

## <a name="change-user-information"></a>更改用户信息
可以更改任何用户属性，但对象 ID 除外。

1. 打开目录。
2. 选择“用户”  选项卡，并选择要更改的用户的显示名称。
3. 完成更改，并单击“保存” 。

如果要更改的用户已与本地 Active Directory 服务同步，则无法使用此过程来更改用户信息。 若要更改该用户，请使用本地 Active Directory 管理工具。

## <a name="whats-next"></a>后续步骤
- [在 Azure Active Directory 中添加来自其他目录或合作伙伴公司的用户](active-directory-create-users-external.md)
- [管理 Azure AD](active-directory-administer.md)
- [在 Azure AD 中管理密码](active-directory-manage-passwords.md)

<!--Image references-->
[1]: ./media/active-directory-create-users/RBACDirConfigTab.png
[2]: ./media/active-directory-create-users/RBACGuestAccessControls.png

<!--Update_Description: update metadata properties -->
