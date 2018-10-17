---
title: 如何在 Azure Active Directory 中添加或删除用户 | Microsoft Docs
description: 了解如何使用 Azure Active Directory 添加新用户或删除现有用户。
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
origin.date: 09/04/2018
ms.date: 10/09/2018
ms.author: v-junlch
ms.reviewer: jeffsta
ms.custom: it-pro
ms.openlocfilehash: 0a93cdb6399582351bb376f3945c777175f538dd
ms.sourcegitcommit: d8b4e1fbda8720bb92cc28631c314fa56fa374ed
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/10/2018
ms.locfileid: "48913813"
---
# <a name="how-to-add-or-delete-users-using-azure-active-directory"></a>如何：使用 Azure Active Directory 添加或删除用户
使用 Azure AD 添加新用户或从 Azure Active Directory (Azure AD) 租户中删除现有用户。

## <a name="add-a-new-user"></a>添加新用户
可以使用 Azure Active Directory 创建新用户。

### <a name="to-add-a-new-user"></a>添加新用户
1. 以目录的全局管理员或用户管理员身份登录到 [Azure 门户](https://portal.azure.cn/)。

2. 依次选择“Azure Active Directory”、“用户”、“新建用户”。

    ![“用户 - 所有用户”页，其中突出显示了“新建用户”](./media/add-users-azure-active-directory/new-user-all-users-blade.png)

3. 在“用户”页上，填写所需的信息。

    ![添加新用户，包含用户信息的“用户”页](./media/add-users-azure-active-directory/new-user-user-blade.png)

    - **名称（必填）。** 新用户的名字和姓氏。 例如，Mary Parker。

    - **用户名（必填）。** 新用户的用户名。 例如，mary@contoso.com。 
    
        用户名的域名部分必须是初始默认域名 <_yourdomainname_>.partner.onmschina.cn，或者是一个自定义域名，例如 contoso.com。 若要详细了解如何创建自定义域名，请参阅[如何向 Azure Active Directory 添加自定义域名](add-custom-domain.md)。

    - **个人资料。** （可选）你可以添加关于用户的详细信息。 也可以在以后添加用户信息。 有关添加用户信息的详细信息，请参阅[如何添加或更改用户个人资料信息](active-directory-users-profile-azure-portal.md)。

    - **组。** （可选）可以将用户添加到一个或多个现有组。 也可以在以后将用户添加到组中。 有关将用户添加到组中的详细信息，请参阅[如何创建基本组并添加成员](active-directory-groups-create-azure-portal.md)。

    - **目录角色。** （可选）可以将用户添加到某个目录角色。 可以将用户分配为全局管理员，或者分配为 Azure AD 中的一个或多个其他管理员角色。 有关分配角色的详细信息，请参阅[如何向用户分配角色](active-directory-users-assign-role-azure-portal.md)。

4. 复制“密码”框中提供的自动生成的密码。 你需要将此密码提供给用户，以便在初次登录过程中使用。

5. 选择“创建”。

    此时将创建用户并将其添加到 Azure AD 租户中。

## <a name="add-a-new-user-within-a-hybrid-environment"></a>在混合环境内添加新用户
如果你的环境中同时包含 Azure Active Directory（云）和 Windows Server Active Directory（本地），则可以通过同步现有用户帐户数据来添加新用户。 有关混合环境和用户的详细信息，请参阅[将本地目录与 Azure Active Directory 集成](../connect/active-directory-aadconnect.md)。

## <a name="delete-a-user"></a>删除用户
可以使用 Azure Active Directory 删除现有用户。

### <a name="to-delete-a-user"></a>删除用户
1. 使用目录的全局管理员帐户登录到 [Azure 门户](https://portal.azure.cn/)。

2. 选择“Azure Active Directory”，选择“用户”，然后搜索并选择要从 Azure AD 租户中删除的用户。 例如，_Mary Parker_。

3. 选择“删除用户”。

    ![“用户 - 所有用户”页，其中突出显示了“删除用户”](./media/add-users-azure-active-directory/delete-user-all-users-blade.png)

    用户将被删除并且不再显示在“用户 - 所有用户”页上。

    >[!Note]
    >必须使用 Windows Server Active Directory 更新其授权来源为 Windows Server Active Directory 的用户的标识、联系信息或工作信息。 完成更新后，必须等待下一个同步循环完成，然后才能看到所做的更改。

## <a name="next-steps"></a>后续步骤
添加用户后，可以执行以下基本流程：

- [添加或更改个人资料信息](active-directory-users-profile-azure-portal.md)

- [向用户分配角色](active-directory-users-assign-role-azure-portal.md)

- [创建基本组并添加成员](active-directory-groups-create-azure-portal.md)

<!-- Update_Description: wording update -->