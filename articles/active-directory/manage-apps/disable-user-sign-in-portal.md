---
title: 在 Azure Active Directory 中对企业应用禁用用户登录 | Microsoft Docs
description: 如何禁用企业应用程序，防止用户在 Azure Active Directory 中登录该程序
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
origin.date: 08/28/2017
ms.date: 08/29/2018
ms.author: v-junlch
ms.reviewer: asteen
ms.custom: it-pro
ms.openlocfilehash: d941ebfaf7c96e3de00e74d4299f7704d6512a42
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52651608"
---
# <a name="disable-user-sign-ins-for-an-enterprise-app-in-azure-active-directory"></a>在 Azure Active Directory 中对企业应用禁用用户登录
可以轻松地禁用企业应用程序，防止用户在 Azure Active Directory (Azure AD) 中登录该程序。 必须具有适当的权限才能管理企业应用，并且必须是目录的全局管理员。

## <a name="how-do-i-disable-user-sign-ins"></a>如何禁用用户登录？
1. 使用目录全局管理员的帐户登录到 [Azure 门户](https://portal.azure.cn) 。
2. 选择“所有服务”，在文本框中输入 **Azure Active Directory**，并选择“Enter”。
3. 在“Azure Active Directory -  目录名”窗格（即，正在管理的目录的 Azure AD 窗格）中，选择“企业应用程序”。

    ![打开企业应用](./media/disable-user-sign-in-portal/open-enterprise-apps.png)
4. 在“企业应用程序”窗格中，选择“所有应用程序”。 此时会显示可管理应用的列表。
5. 在“企业应用程序 - 所有应用程序”窗格中，选择一个应用。
6. 在 ***appname*** 窗格（即标题中包含所选应用的名称的窗格）中，选择“属性”。

    ![选择“所有应用程序”命令](./media/disable-user-sign-in-portal/select-app.png)
7. 在“appname - 属性”窗格中，对“启用以让用户登录?”设置选择“否”。
8. 选择“保存”命令。

## <a name="next-steps"></a>后续步骤
- [查看所有组](../fundamentals/active-directory-groups-view-azure-portal.md)
- [Assign a user or group to an enterprise app](assign-user-or-group-access-portal.md)
- [Remove a user or group assignment from an enterprise app](remove-user-or-group-access-portal.md)
- [更改企业应用的名称或徽标](change-name-or-logo-portal.md)

<!-- Update_Description: update metedata properties -->