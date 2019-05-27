---
title: 在 Azure Active Directory 中对企业应用禁用用户登录 | Microsoft Docs
description: 如何禁用企业应用程序，防止用户在 Azure Active Directory 中登录该程序
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
origin.date: 04/12/2019
ms.date: 05/13/2019
ms.author: v-junlch
ms.reviewer: asteen
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5018e1962e26c169c52739f7df7aef755332ee9e
ms.sourcegitcommit: 9235a1f313393f21b5c42cb7a1626b1b93feb8be
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/14/2019
ms.locfileid: "65598794"
---
# <a name="disable-user-sign-ins-for-an-enterprise-app-in-azure-active-directory"></a>在 Azure Active Directory 中对企业应用禁用用户登录
可以轻松地禁用企业应用程序，防止用户在 Azure Active Directory (Azure AD) 中登录该程序。 你需要具有合适的权限才能管理企业应用。 而且，你必须是目录的全局管理员。

## <a name="how-do-i-disable-user-sign-ins"></a>如何禁用用户登录？
1. 使用目录全局管理员的帐户登录到 [Azure 门户](https://portal.azure.cn) 。
1. 选择“所有服务”，在文本框中输入 **Azure Active Directory**，并选择“Enter”。
1. 在“Azure Active Directory -  目录名”窗格（即，正在管理的目录的 Azure AD 窗格）中，选择“企业应用程序”。
1. 在“企业应用程序 - 所有应用程序”窗格上，你会看到你可以管理的应用的列表。 选择一个应用。
1. 在 ***appname*** 窗格（即标题中包含所选应用的名称的窗格）中，选择“属性”。
1. 在“appname - 属性”窗格中，对“启用以让用户登录?”设置选择“否”。
1. 选择“保存”命令。

## <a name="next-steps"></a>后续步骤
* [查看所有组](../fundamentals/active-directory-groups-view-azure-portal.md)
* [向企业应用分配用户](assign-user-or-group-access-portal.md)
* [删除企业应用的用户分配](remove-user-or-group-access-portal.md)
* [Change the name or logo of an enterprise app](change-name-or-logo-portal.md)

<!-- Update_Description: wording update -->