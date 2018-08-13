---
title: 在 Azure Active Directory 中添加或删除用户 | Microsoft Docs
description: 介绍如何在 Azure Active Directory 中添加新用户或删除现有用户
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: quickstart
origin.date: 01/08/2018
ms.date: 08/07/2018
ms.author: v-junlch
ms.reviewer: jeffsta
ms.custom: it-pro
ms.openlocfilehash: 75c03c05d7400da32c9230b411d4276c89e3dce6
ms.sourcegitcommit: 7cdf4633aea04e524cb48cb1990b750ae8be841c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/07/2018
ms.locfileid: "39584251"
---
# <a name="quickstart-add-new-users-to-azure-active-directory"></a>快速入门：将新用户添加到 Azure Active Directory
本文介绍如何使用 Azure 门户或通过同步本地 Windows Server AD 用户帐户数据，在组织的 Azure Active Directory (Azure AD) 租户中删除或添加组织中的用户。 

## <a name="add-cloud-based-users"></a>添加基于云的用户
1. 使用属于目录全局管理员的帐户登录到 [Azure Active Directory 管理中心](https://portal.azure.cn)。
2. 依次选择“Azure Active Directory”、“用户”。
3. 在“用户”中选择“所有用户”，然后选择“新建用户”。
   ![选择“添加”命令](./media/add-users-azure-active-directory/add-user.png)
4. 输入用户的详细信息，如**名称**和**用户名**。 用户名的域名部分必须是初始默认域名“[domain name].partner.onmschina.cn”或已验证的非联合[自定义域名](add-custom-domain.md)（例如“contoso.com”）。
5. 复制或以其他方式记下生成的用户密码，以便在此过程完成后可以提供给用户。
6. （可选）可以打开“个人资料”、“组”或“目录角色”并在其中填写用户信息。 有关用户和管理员角色的详细信息，请参阅[在 Azure AD 中分配管理员角色](../users-groups-roles/directory-assign-admin-roles.md)。
7. 在“用户”上，选择“创建”。
8. 以安全方式将生成的密码分发给新用户，以便用户可以登录。

> [!TIP]
> 此外，你还可以从本地 Windows Server AD 同步用户帐户数据。 Microsoft 的标识解决方案跨越本地和基于云的功能，创建单一用户标识对所有资源进行身份验证和授权，而不考虑其位置。 我们称此为混合标识。 [Azure AD Connect](/active-directory/connect/active-directory-aadconnect) 可用于集成本地目录与 Azure Active Directory 实现混合标识情景。 这样便可以为集成到 Azure AD 的 Office 365、Azure 和 SaaS 应用程序的用户提供一个通用标识。 

## <a name="delete-users-from-azure-ad"></a>从 Azure AD 中删除用户
1. 使用目录全局管理员的帐户登录到 [Azure 门户](https://portal.azure.cn)。
2. 选择“用户”。
3. 在“用户”边栏选项卡中，从列表中选择要删除的用户。 
4. 选择“删除用户”。 

### <a name="learn-more"></a>了解详细信息 
- [在 Azure AD 中为用户分配角色](active-directory-users-assign-role-azure-portal.md)
- [管理用户个人资料](active-directory-users-profile-azure-portal.md)

<!-- Update_Description: link update -->