---
title: 在 Azure Active Directory 中删除企业应用的用户或组分配 | Microsoft Docs
description: 如何在 Azure Active Directory 的企业应用中删除对用户或组的访问权限分配
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
origin.date: 02/14/2018
ms.date: 08/29/2018
ms.author: v-junlch
ms.reviewer: asteen
ms.custom: it-pro
ms.openlocfilehash: 2e318d599a3a5cb9f3d32a97b4776c7008819079
ms.sourcegitcommit: aee279ed9192773de55e52e628bb9e0e9055120e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2018
ms.locfileid: "43164952"
---
# <a name="remove-a-user-or-group-assignment-from-an-enterprise-app-in-azure-active-directory"></a>在 Azure Active Directory 中删除企业应用的用户或组分配
可以轻松地在 Azure Active Directory (Azure AD) 中删除用户或组对企业应用程序的已分配访问权限。 必须具有适当的权限才能管理企业应用，并且必须是目录的全局管理员。

> [!NOTE]
> 对于 Microsoft 应用程序（例如 Office 365 应用），请使用 PowerShell 删除到企业应用的用户分配。

## <a name="how-do-i-remove-a-user-or-group-assignment-to-an-enterprise-app-in-the-azure-portal"></a>如何在 Azure 门户中删除到企业应用的用户或组分配？
1. 使用目录全局管理员的帐户登录到 [Azure 门户](https://portal.azure.cn) 。
2. 选择“更多服务”，在文本框中输入 **Azure Active Directory**，并选择“Enter”。
3. 在“Azure Active Directory - *directoryname*”页面（即，正在管理的目录的 Azure AD 页面）上，选择“企业应用程序”。

    ![打开企业应用](./media/remove-user-or-group-access-portal/open-enterprise-apps.png)
4. 在“企业应用程序”页面上，选择“所有应用程序”。 此时会显示可管理应用的列表。
5. 在“企业应用程序 - 所有应用程序”页面上，选择一个应用。
6. 在 ***appname*** 页面（即标题中包含所选应用的名称的页面）上，选择“用户和组”。

    ![选择用户或组](./media/remove-user-or-group-access-portal/remove-app-users.png)
7. 在“***appname*** - 用户和组分配”页面上，选择一个或多个用户或组，并选择“删除”命令。 出现提示时确认所作的决定。

    ![选择“删除”命令](./media/remove-user-or-group-access-portal/remove-users.png)

## <a name="next-steps"></a>后续步骤

- [查看所有组](../fundamentals/active-directory-groups-view-azure-portal.md)
- [向企业应用分配用户或组](assign-user-or-group-access-portal.md)
- [禁用企业应用的用户登录](disable-user-sign-in-portal.md)
- [更改企业应用的名称或徽标](change-name-or-logo-portal.md)

<!-- Update_Description: update metedata properties -->