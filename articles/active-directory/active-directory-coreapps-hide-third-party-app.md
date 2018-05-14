---
title: 使应用程序不出现在用户在 Azure Active Directory 中的体验中 | Microsoft Docs
description: 如何使应用程序不出现在用户在 Azure Active Directory 访问面板或 Office 365 启动器的体验中。
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 01/04/2018
ms.date: 05/07/2018
ms.author: v-junlch
ms.reviewer: asteen
ms.custom: it-pro
ms.openlocfilehash: bf16260f183dff1ee07fd4aa110cd97c11cb2387
ms.sourcegitcommit: beee57ca976e21faa450dd749473f457e299bbfd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/10/2018
---
# <a name="hide-an-application-from-users-experience-in-azure-active-directory"></a>使应用程序不出现在用户在 Azure Active Directory 中的体验中

如果有不希望显示在用户的访问面板或 Office 365 启动器上的应用程序，可使用隐藏此应用磁贴的选项。  以下两个选项可用于从用户的应用启动器中隐藏应用程序。



### <a name="how-do-i-hide-a-third-party-app-from-users-access-panel-and-o365-app-launchers"></a>如何使第三方应用不显示在用户的访问面板和 O365 应用启动器上？

1. 使用目录全局管理员的帐户登录到 [Azure 门户](https://portal.azure.cn) 。
2. 选择“所有服务”，在文本框中输入 **Azure Active Directory**，并选择“Enter”。
3. 在 **Azure Active Directory - *目录名称*** 屏幕上 （即所管理目录的 Azure AD 屏幕），选择 “**企业应用程序**”。
![企业应用](media/active-directory-coreapps-hide-third-party-app/app1.png)
4. 在“企业应用程序”屏幕上，选择“所有应用程序”。 此时会显示可管理应用的列表。
5. 在“企业应用程序 - 所有应用程序”屏幕上，选择一个应用。</br>
![企业应用](media/active-directory-coreapps-hide-third-party-app/app2.png)
6. 在“appname”屏幕（即标题中包含所选应用名称的屏幕）上，选择“属性”。
7. 在“appname - 属性”屏幕上，对于“是否对用户可见?”选择“是”。
![企业应用](media/active-directory-coreapps-hide-third-party-app/app3.png)
8. 选择“保存”命令。

## <a name="next-steps"></a>后续步骤
* [查看所有组](active-directory-groups-view-azure-portal.md)
* [向企业应用分配用户或组](active-directory-coreapps-assign-user-azure-portal.md)
* [删除企业应用的用户或组分配](active-directory-coreapps-remove-assignment-azure-portal.md)
* [更改企业应用的名称或徽标](active-directory-coreapps-change-app-logo-user-azure-portal.md)

<!-- Update_Description: update metedata properties -->