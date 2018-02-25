---
title: "在 Azure Active Directory 中向用户分配管理员角色 | Microsoft Docs"
description: "说明如何在 Azure Active Directory 中更改用户管理信息"
services: active-directory
documentationcenter: 
author: yunan2016
manager: digimobile
editor: 
ms.assetid: a1ca1a53-50d8-4bf0-ae8f-73fa1253e2d9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 01/08/2018
ms.date: 1/29/2018
ms.author: v-nany
ms.reviewer: jeffsta
ms.openlocfilehash: 3c9212773b70cf036d33e62874daf413d20a4ba8
ms.sourcegitcommit: 0b0d3b61e91a97277de8eda8d7a8e114b7c4d8c1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/23/2018
---
# <a name="assign-a-user-to-administrator-roles-in-azure-active-directory"></a>在 Azure Active Directory 中向用户分配管理员角色
本文介绍如何将管理角色分配给 Azure Active Directory (Azure AD) 中的用户。  默认情况下，添加的用户没有管理员权限，但你随时可以向他们分配角色。

## <a name="assign-a-role-to-a-user"></a>向用户分配角色
1. 使用目录全局管理员的帐户登录到 [Azure 门户](https://portal.azure.cn) 。
2. 选择“用户和组”。

   ![打开“用户管理”](./media/active-directory-users-assign-role-azure-portal/create-users-user-management.png)
3. 选择“所有用户”。
  
  ![打开“所有用户”组](./media/active-directory-users-assign-role-azure-portal/create-users-open-users-blade.png)
4. 从列表中选择用户。
5. 对于所选用户，选择“目录角色”，并为用户分配“目录角色”列表中的一个角色。 有关用户和管理员角色的详细信息，请参阅[在 Azure AD 中分配管理员角色](active-directory-assign-admin-roles-azure-portal.md)。

      ![为用户分配角色](./media/active-directory-users-assign-role-azure-portal/create-users-assign-role.png)
6. 选择“其他安全性验证” 。

## <a name="next-steps"></a>后续步骤
* [在新版 Azure 门户中重置用户的密码](active-directory-users-reset-password-azure-portal.md)
* [管理用户个人资料](active-directory-users-profile-azure-portal.md)
