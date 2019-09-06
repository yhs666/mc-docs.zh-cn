---
title: 什么是 Azure Active Directory？ - Azure Active Directory | Microsoft Docs
description: 有关 Azure Active Directory 的概述和概念信息，包括术语、可用的许可证、相关功能的列表及其详细信息链接。
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.topic: overview
origin.date: 07/31/2019
ms.date: 08/27/2019
ms.author: v-junlch
ms.custom: it-pro, seodec18, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: d119d196dfd7d185a2ad9569c555ca23744b0925
ms.sourcegitcommit: 18a0d2561c8b60819671ca8e4ea8147fe9d41feb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2019
ms.locfileid: "70134180"
---
# <a name="what-is-azure-active-directory"></a>什么是 Azure Active Directory？

Azure Active Directory (Azure AD) 是 Microsoft 推出的基于云的标识和访问管理服务，可帮助员工登录及访问以下位置的资源：


## <a name="terminology"></a>术语

为了更好地理解 Azure AD 及其文档，我们建议查看以下术语。

|术语或概念|说明|
|---------------|-----------|
|标识| 可以获得身份验证的东西。 标识可以是具有用户名和密码的用户。 标识还包括可能需要通过密钥或证书进行身份验证的应用程序或其他服务器。|
|帐户| 具有与之关联的数据的标识。 你不能拥有没有标识的帐户。|
|Azure AD 帐户| 通过 Azure AD 或其他 Azure 云服务（例如 Office 365）创建的标识。 标识存储在 Azure AD 中，可供组织的云服务订阅访问。 此帐户有时也称为工作或学校帐户。|
|Azure 订阅| 用于为 Azure 云服务付费。 可以有多个订阅，这些订阅与一张信用卡关联。|
|Azure 租户| 组织在注册 Azure、Microsoft Intune 或 Office 365 等 Azure 云服务订阅时自动创建的专用且受信任的 Azure AD 实例。 一个 Azure 租户表示一个组织。|
|单租户| 可以将访问专用环境中的其他服务的 Azure 租户视为单租户。|
|多租户| 可以将访问共享环境中的其他服务的 Azure 租户（跨多个组织）视为多租户。|
|Azure AD 目录|每个 Azure 租户都有一个专用且受信任的 Azure AD 目录。 Azure AD 目录包括租户的用户、组和应用，用于针对租户资源执行标识和访问管理功能。|
|自定义域|每个新的 Azure AD directory 都附带了一个初始域名 domainname.partner.onmschina.cn。 除了该初始名称，还可以向列表添加组织的域名，其中包括用来开展业务的名称以及用户用来访问组织资源的名称。 添加自定义域名有助于创建用户所熟悉的用户名，例如 alain@contoso.com。|
|帐户管理员|从概念上讲，此经典订阅管理员角色是订阅的账单所有者。 此角色可以访问 [Azure 帐户中心](https://account.windowsazure.cn/Subscriptions)，用于管理一个帐户中的所有订阅。 有关详细信息，请参阅[经典订阅管理员角色、Azure 基于角色的访问控制 (RBAC) 角色和 Azure AD 管理员角色](../../role-based-access-control/rbac-and-directory-admin-roles.md)。|
|服务管理员|此经典订阅管理员角色用于管理所有 Azure 资源，包括访问权限。 此角色拥有在订阅范围内分配有“所有者”角色的用户的等效访问权限。 有关详细信息，请参阅[经典订阅管理员角色、Azure RBAC 角色和 Azure AD 管理员角色](../../role-based-access-control/rbac-and-directory-admin-roles.md)。|
|所有者|此角色有助于管理所有 Azure 资源，包括访问权限。 此角色在称为基于角色的访问控制 (RBAC) 的较新授权系统上构建，该系统可提供对 Azure 资源的精细访问管理。 有关详细信息，请参阅[经典订阅管理员角色、Azure RBAC 角色和 Azure AD 管理员角色](../../role-based-access-control/rbac-and-directory-admin-roles.md)。|
|Azure AD 全局管理员|此管理员角色自动分配给创建 Azure AD 租户的人员。 全局管理员可以执行 Azure AD 以及与 Azure AD 联合的任意服务（例如 Exchange Online、SharePoint Online 和 Skype for Business Online）的所有管理功能。 可以有多个全局管理员，但只有全局管理员才能向用户分配管理员角色（包括分配其他全局管理员）。<br><br>**注意**<br>此管理员角色在 Azure 门户中称为“全局管理员”，但在 Microsoft Graph API、Azure AD Graph API 和 Azure AD PowerShell 中称为“公司管理员”。 <br><br>有关各种管理员角色的详细信息，请参阅 [Azure Active Directory 中的管理员角色权限](../users-groups-roles/directory-assign-admin-roles.md)。|

## <a name="next-steps"></a>后续步骤

- [注册 Azure Active Directory Premium](active-directory-get-started-premium.md)

- [将 Azure 订阅关联到 Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)

- [访问 Azure Active Directory 并创建新租户](active-directory-access-create-new-tenant.md)

