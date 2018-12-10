---
title: 如何使用 Azure Active Directory 向用户分配目录角色 | Microsoft Docs
description: 了解如何使用 Azure Active Directory 向用户分配目录角色。
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
origin.date: 09/06/2018
ms.date: 10/09/2018
ms.author: v-junlch
ms.reviewer: jeffsta
ms.openlocfilehash: 4cddbe1d96822849fc5c95fa3edbc524eaa332d4
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52663019"
---
# <a name="how-to-assign-roles-and-administrators-to-users-with-azure-active-directory"></a>如何：使用 Azure Active Directory 向用户分配角色和管理员
如果你的组织中的某位用户需要有权管理 Azure Active Directory (Azure AD) 资源，则必须根据该用户需要有权执行的操作在 Azure AD 中为该用户分配合适的角色。

有关可用角色的详细信息，请参阅[在 Azure Active Directory 中分配管理员角色](../users-groups-roles/directory-assign-admin-roles.md)。 有关添加用户的详细信息，请参阅[向 Azure Active Directory 中添加新用户](add-users-azure-active-directory.md)。

## <a name="assign-roles"></a>分配角色
向用户分配 Azure AD 角色的一种常用方式是使用用户的“目录角色”页面。

### <a name="to-assign-a-role-to-a-user"></a>向用户分配角色
1. 使用目录的全局管理员帐户登录到 [Azure 门户](https://portal.azure.cn/)。

2. 选择“Azure Active Directory”，选择“用户”，然后搜索并选择要获得角色分配的用户。 例如，_Alain Charon_。

3. 在“Alain Charon - 个人资料”页面上，选择“目录角色”。

    此时将显示“Alain Charon - 目录角色”页面。

4. 选择“添加角色”，选择要分配给 Alain 的角色（例如“应用程序管理员”），然后选择“选择”。

    ![“目录角色”页面，其中显示了所选的角色](./media/active-directory-users-assign-role-azure-portal/directory-role-select-role.png)

    “应用程序管理员”角色将分配给 Alain Charon，并且它将显示在“Alain Charon - 目录角色”页面上。

## <a name="remove-a-role-assignment"></a>删除角色分配
如果需要删除用户的角色分配，也可以从“Alain Charon - 目录角色”页面执行该操作。

### <a name="to-remove-a-role-assignment-from-a-user"></a>删除用户的角色分配

1. 选择“Azure Active Directory”，选择“用户”，然后搜索并选择要删除角色分配的用户。 例如，_Alain Charon_。

2. 选择“目录角色”，选择“应用程序管理员”，然后选择“删除角色”。

    ![“目录角色”页面，其中显示了所选的角色和删除选项](./media/active-directory-users-assign-role-azure-portal/directory-role-remove-role.png)

    “应用程序管理员”角色将从 Alain Charon 中删除，并且不再显示在“Alain Charon - 目录角色”页面上。

## <a name="next-steps"></a>后续步骤
- [添加或删除用户](add-users-azure-active-directory.md)

- [添加或更改个人资料信息](active-directory-users-profile-azure-portal.md)

另外，你可以执行其他用户管理任务，例如，分配委托、使用策略以及共享用户帐户。 有关其他可用操作的详细信息，请参阅 [Azure Active Directory 用户管理和文档](../users-groups-roles/index.yml)。


<!-- Update_Description: wording update -->
