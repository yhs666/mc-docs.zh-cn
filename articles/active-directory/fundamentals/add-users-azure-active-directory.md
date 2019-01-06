---
title: 添加或删除用户 - Azure Active Directory | Microsoft Docs
description: 有关如何使用 Azure Active Directory 添加新用户或删除现有用户的说明。
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
origin.date: 09/04/2018
ms.date: 01/02/2019
ms.author: v-junlch
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.openlocfilehash: de77f78796a2e73ac6de6180176a862892c81a0e
ms.sourcegitcommit: 4f91d9bc4c607cf254479a6e5c726849caa95ad8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2019
ms.locfileid: "53996196"
---
# <a name="add-or-delete-users-using-azure-active-directory"></a>使用 Azure Active Directory 添加或删除用户
在 Azure Active Directory (Azure AD) 租户中添加新用户或删除现有用户。

## <a name="add-a-new-user"></a>添加新用户
可使用 Azure Active Directory 创建新用户。

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

4. 复制“密码”框中提供的自动生成的密码。 需要将此密码提供给用户以进行初始登录过程。

5. 选择“创建”。

    用户已创建并添加到 Azure AD 租户。

## <a name="add-a-new-user-within-a-hybrid-environment"></a>在混合环境内添加新用户
如果你的环境中同时包含 Azure Active Directory（云）和 Windows Server Active Directory（本地），则可以通过同步现有用户帐户数据来添加新用户。 有关混合环境和用户的详细信息，请参阅[将本地目录与 Azure Active Directory 进行集成](../hybrid/whatis-hybrid-identity.md)。

## <a name="delete-a-user"></a>删除用户
可使用 Azure Active Directory 删除现有用户。

### <a name="to-delete-a-user"></a>删除用户
1. 使用目录的全局管理员帐户登录到 [Azure 门户](https://portal.azure.cn/)。

2. 选择“Azure Active Directory”并选择“用户”然后搜索并选择想要从 Azure AD 租户中删除的用户。 例如，_Mary Parker_。

3. 选择“删除用户”。

    ![“用户 - 所有用户”页，其中突出显示了“删除用户”](./media/add-users-azure-active-directory/delete-user-all-users-blade.png)

    该用户已删除并不再显示在“用户 - 所有用户”页上。 可在接下来的 30 天内于“已删除用户”页查看该用户，在此期间可将其还原。 有关还原用户的详细信息，请参阅[如何还原或永久删除最近删除的用户](active-directory-users-restore.md)。

    >[!Note]
    >必须使用 Windows Server Active Directory 更新归属于 Windows Server Active Directory 的用户的身份、联系信息或工作信息。 完成更新后，必须等待下一个同步周期完成才能看到更改。

## <a name="next-steps"></a>后续步骤
添加用户后，可以执行以下基本过程：

- [添加或更改配置文件信息](active-directory-users-profile-azure-portal.md)

- [向用户分配角色](active-directory-users-assign-role-azure-portal.md)

- [创建基本组并添加成员](active-directory-groups-create-azure-portal.md)

或可执行其他用户管理任务，例如[恢复已删除的用户](active-directory-users-restore.md)。 有关其他可用操作的详细信息，请参阅 [Azure Active Directory 用户管理和文档](../users-groups-roles/index.yml)。

<!-- Update_Description: wording update -->