---
title: 在 Azure Active Directory 中分配管理员角色 | Microsoft Docs
description: 管理员角色可以添加用户、分配管理角色、重置用户密码、管理用户许可证，或者管理域。 分配为管理员角色的用户在组织已订阅的所有云服务中具有相同的权限。
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
origin.date: 09/07/2018
ms.date: 10/11/2018
ms.author: v-junlch
ms.reviewer: vincesm
ms.custom: it-pro
ms.openlocfilehash: 7124309e4475668c77172c74d2ed9df96533c5b1
ms.sourcegitcommit: 4c7b45c574675b157e6520295f86b7e19450f5e3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/11/2018
ms.locfileid: "49088926"
---
# <a name="assigning-administrator-roles-in-azure-active-directory"></a>在 Azure Active Directory 中分配管理员角色

使用 Azure Active Directory (Azure AD) 时，可以指定不同的管理员来执行不同的功能。 可以在 Azure AD 门户中指定管理员用于执行各种任务，例如，添加或更改用户、分配管理角色、重置用户密码、管理用户许可证，以及管理域名。

## <a name="details-about-the-global-administrator-role"></a>有关全局管理员角色的详细信息

全局管理员有权访问所有管理功能。 默认情况下，系统会将注册 Azure 订阅的人员指派为目录的全局管理员角色。 只有全局管理员才能分配其他管理员角色。

## <a name="assign-or-remove-administrator-roles"></a>分配或删除管理员角色

若要了解如何在 Azure Active Directory 中向用户分配管理角色，请参阅[在 Azure Active Directory 中向用户分配管理角色](../fundamentals/active-directory-users-assign-role-azure-portal.md)。

## <a name="available-roles"></a>可用的角色

提供以下管理员角色：

- **[应用程序管理员](#application-administrator)**：此角色中的用户可以创建和管理企业应用程序、应用程序注册和应用程序代理设置的所有方面。 此角色还可以同意委派权限，以及除 Microsoft Graph 和 Azure AD Graph 之外的应用程序权限。 在创建新应用程序注册或企业应用程序时，不会将此角色的成员添加为所有者。

- **[应用程序开发人员](#application-developer)**：在将设置“用户可以注册应用程序”设置为“否”时，此角色中的用户可以创建应用程序注册。 在设置“用户可以同意应用代表他们访问公司数据”设置为“否”时，此角色还使成员能够代表自己授予同意。 在创建新应用程序注册或企业应用程序时，会将此角色的成员添加为所有者。

- **[计费管理员](#billing-administrator)**：进行采购、管理订阅、管理支持票证并监视服务运行状况。

- **[云应用程序管理员](#cloud-application-administrator)**：此角色中的用户具有与应用程序管理员角色相同的权限，但不包括管理应用程序代理的权限。 此角色授予创建和管理企业应用程序和应用程序注册的所有方面的权限。 此角色还可以同意委派权限，以及除 Microsoft Graph 和 Azure AD Graph 之外的应用程序权限。 在创建新应用程序注册或企业应用程序时，不会将此角色的成员添加为所有者。

- **[合规性管理员](#compliance-administrator)**：拥有此角色的用户具有 Office 365 安全与合规中心和 Exchange 管理中心中的管理权限。 有关详细信息，请参阅 [About Office 365 admin roles](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d)（关于 Office 365 管理员角色）。
 
- **[Dynamics 365 服务管理员/CRM 服务管理员](#dynamics-365-service-administrator)**：具有此角色的用户在 Microsoft Dynamics 365 Online（如果存在此服务）中拥有全局权限，并可以管理支持票证和监视服务运行状况。 有关详细信息，请参阅[使用服务管理员角色管理租户](https://docs.microsoft.com/dynamics365/customer-engagement/admin/use-service-admin-role-manage-tenant)。

- **[Exchange 服务管理员](#exchange-service-administrator)**：具有此角色的用户在 Microsoft Exchange Online（如果存在此服务）中拥有全局权限。 有关详细信息，请参阅 [About Office 365 admin roles](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d)（关于 Office 365 管理员角色）。

- **[全局管理员/公司管理员](#company-administrator)**：具有此角色的用户有权访问 Azure Active Directory 以及使用 Azure Active Directory 标识的服务（例如 Exchange Online、SharePoint Online 和 Skype for Business Online）中的所有管理功能。 注册 Azure Active Directory 租户的人员将成为全局管理员。 只有全局管理员才能分配其他管理员角色。 公司中可以有多个全局管理员。 全局管理员可以为任何用户和所有其他管理员重置密码。

  > [!NOTE]
  > 在 Microsoft 图形 API、Azure AD 图形 API 和 Azure AD PowerShell 中，此角色标识为“公司管理员”。 它是 [Azure 门户](https://portal.azure.cn)中的“全局管理员”。
  >
  >

- **[来宾邀请者](#guest-inviter)**：此角色中的用户可以管理 Azure Active Directory B2B 来宾用户邀请当“成员可以邀请”用户设置设置为“否”。 

- **[密码管理员/支持管理员](#helpdesk-administrator)**：具有此角色的用户可以更改密码、使刷新令牌失效、管理服务请求和监视服务运行状况。 支持管理员只能为用户和其他支持管理员更改密码和使刷新令牌失效。 使刷新令牌失效会强制用户重新登录。

  > [!NOTE]
  > 在 Microsoft 图形 API、Azure AD 图形 API 和 Azure AD PowerShell 中，此角色标识为“支持管理员”。 它是 [Azure 门户](https://portal.azure.cn/)中的“密码管理员”。
  >
  >
  
- **[Power BI 服务管理员](#power-bi-service-administrator)**：具有此角色的用户在 Power BI（如果存在此服务）中拥有全局权限，并可管理支持票证和监视服务运行状况。 有关详细信息，请参阅[了解 Power BI 管理员角色](https://docs.microsoft.com/power-bi/service-admin-role)。

- **[特权角色管理员](#privileged-role-administrator)**：具有此角色的用户可以管理角色分配以及 Azure AD Privileged Identity Management 内的 Azure Active Directory。 此外，此角色允许 Privileged Identity Management 的所有方面。

- **[安全管理员](#security-administrator)**：具有此角色的用户具有“安全读取者”角色的所有只读权限，以及能够管理与安全相关的服务配置的能力：Azure Active Directory Identity Protection、Azure 信息保护和 Office 365 安全与合规中心。 [Office 365 安全与合规中心](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1)提供了有关 Office 365 权限的详细信息。
  
  | In | 有权执行的操作 |
  | --- | --- |
  | Identity Protection Center |<ul><li>安全读取者角色的所有权限。<li>此外，还能够执行除了重置密码以外的所有 IPC 操作。 |
  | Privileged Identity Management |<ul><li>安全读取者角色的所有权限。<li>**不能**管理 Azure AD 角色成员身份或设置。 |
  | <p>监视 Office 365 服务运行状况</p><p>Office 365 安全与合规中心 |<ul><li>安全读取者角色的所有权限。<li>可以配置高级威胁防护功能中的所有设置（恶意软件和病毒保护、恶意 URL 配置、URL 跟踪等）。 |
  
- **[安全读取者](#security-reader)**：具有此角色的用户具有全局只读访问权限，包括 Azure Active Directory、Identity Protection、Privileged Identity Management，以及能够读取 Azure Active Directory 登录报告和审核日志中的所有信息。 角色还授予 Office 365 安全与合规中心的只读权限。 [Office 365 安全与合规中心](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1)提供了有关 Office 365 权限的详细信息。

  | In | 有权执行的操作 |
  | --- | --- |
  | Identity Protection Center |读取安全功能的所有安全报告和设置信息<ul><li>反垃圾邮件<li>加密<li>数据丢失防护<li>反恶意软件<li>高级威胁防护<li>防网络钓鱼<li>邮件流规则 |
  | Privileged Identity Management |<p>以只读方式访问 Azure AD PIM 中所显示的一切信息：Azure AD 角色分配的策略和报告、安全审阅，以及在未来还可通过读取来访问 Azure AD 角色分配以外的方案的策略数据和报告。<p>**不能**注册 Azure AD PIM 或对其进行任何更改。 担任此角色的人员可以在 PIM 的门户中或通过 PowerShell，为其他角色（例如，全局管理员或特权角色管理员）的候选用户激活角色。 |
  | <p>监视 Office 365 服务运行状况</p><p>Office 365 安全与合规中心</p> |<ul><li>读取和管理警报<li>读取安全策略<li>读取威胁情报、云应用发现以及搜索和调查中的隔离区<li>读取所有报告 |

- **[服务支持管理员](#service-support-administrator)**：具有此角色的用户可以打开支持请求与 Microsoft Azure 和 Office 365 服务和服务仪表板和消息中心在 Azure 门户和 Office 365 管理门户中的视图。 有关详细信息，请参阅 [About Office 365 admin roles](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d)（关于 Office 365 管理员角色）。

- **[SharePoint 服务管理员](#sharepoint-service-administrator)**：具有此角色的用户在 Microsoft SharePoint Online（如果存在此服务）中拥有全局权限，并可以管理支持票证和监视服务运行状况。 有关详细信息，请参阅 [About Office 365 admin roles](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d)（关于 Office 365 管理员角色）。

- **[Skype for Business/Lync 服务管理员](#lync-service-administrator)**：具有此角色的用户具有 Microsoft Skype for Business 中的全局权限，以及管理 Azure Active Directory 中的特定于 Skype 的用户属性。 此外，此角色可授予管理支持票证、监视服务运行状况以及访问 Teams 和 Skype for Business 管理中心的能力。 帐户必须获取 Teams 许可证，否则无法运行 Teams PowerShell cmdlet。 有关详细信息，请参阅[关于 Skype for Business 管理员角色](https://support.office.com/en-us/article/about-the-skype-for-business-admin-role-aeb35bda-93fc-49b1-ac2c-c74fbeb737b5)；有关 Teams 许可信息，请参阅 [Skype for Business 和 Microsoft Teams 附加许可](https://docs.microsoft.com/en-us/skypeforbusiness/skype-for-business-and-microsoft-teams-add-on-licensing/skype-for-business-and-microsoft-teams-add-on-licensing)

  > [!NOTE]
  > 在 Microsoft 图形 API、Azure AD 图形 API 和 Azure AD PowerShell 中，此角色标识为“Lync 服务管理员”。 它是 [Azure 门户](https://portal.azure.cn/)中的“Skype for Business 服务管理员”。
  >
  >

- **[用户帐户管理员](#user-account-administrator)**：具有此角色的用户可以创建和管理用户和组的所有方面。 此外，此角色包括管理支持票证的功能，并监视服务运行状况。 适用某些限制。 例如，此角色不允许删除全局管理员。 用户帐户管理员只能更改用户、支持管理员和其他用户帐户管理员的密码以及使刷新令牌失效。 使刷新令牌失效会强制用户重新登录。

| 有权执行的操作 | 无权执行的操作 |
| --- | --- |
| <p>查看公司信息和用户信息</p><p>管理 Office 支持票证</p><p>只能为用户、支持管理员和其他用户帐户管理员更改密码</p><p>创建和管理用户视图</p><p>创建、编辑和删除用户与组，以及管理用户许可证，但有限制。 他/她不能删除全局管理员或创建其他管理员。</p> |<p>为 Office 产品执行计费和采购操作</p><p>管理域</p><p>管理公司信息</p><p>向其他人委派管理角色</p><p>使用目录同步</p><p>启用或禁用多重身份验证</p><p>查看审核日志</p> |

## <a name="deprecated-roles"></a>已弃用的角色

不应使用以下角色。 这些角色已弃用，并将从 Azure AD 中删除。

- 即席许可证管理员
- 设备联接
- 设备管理器
- 设备用户
- 经电子邮件验证的用户创建者
- 邮箱管理员
- 工作区设备联接

## <a name="detailed-azure-active-directory-permissions"></a>详细的 Azure Active Directory 权限
下表描述 Azure Active Directory 中授予每个角色的特定权限。 某些角色（例如全局管理员）可能在 Azure Active Directory 外部的 Microsoft 服务中拥有其他权限。


### <a name="application-administrator"></a>应用程序管理员
可以创建和管理应用注册和企业应用的所有方面。

  > [!NOTE]
  > 此角色从“目录读取者”角色继承其他权限。
  >
  >

| **操作** | **说明** |
| --- | --- |
| microsoft.aad.directory/applications/audience/update | 更新 Azure Active Directory 中的 applications.audience 属性。 |
| microsoft.aad.directory/applications/authentication/update | 更新 Azure Active Directory 中的 applications.authentication 属性。 |
| microsoft.aad.directory/applications/default/update | 更新 Azure Active Directory 中应用程序的基本属性。 |
| microsoft.aad.directory/applications/create | 在 Azure Active Directory 中创建应用程序。 |
| microsoft.aad.directory/applications/credentials/update | 更新 Azure Active Directory 中的 applications.credentials 属性。 |
| microsoft.aad.directory/applications/delete | 删除 Azure Active Directory 中的应用程序。 |
| microsoft.aad.directory/applications/owners/update | 更新 Azure Active Directory 中的 applications.owners 属性。 |
| microsoft.aad.directory/applications/permissions/update | 更新 Azure Active Directory 中的 applications.permissions 属性。 |
| microsoft.aad.directory/applications/policies/update | 更新 Azure Active Directory 中的 applications.policies 属性。 |
| microsoft.aad.directory/appRoleAssignments/create | 在 Azure Active Directory 中创建 appRoleAssignments。 |
| microsoft.aad.directory/appRoleAssignments/read | 读取 Azure Active Directory 中的 appRoleAssignments。 |
| microsoft.aad.directory/appRoleAssignments/update | 更新 Azure Active Directory 中的 appRoleAssignments。 |
| microsoft.aad.directory/appRoleAssignments/delete | 删除 Azure Active Directory 中的 appRoleAssignments。 |
| microsoft.aad.directory/policies/applicationConfiguration/default/read | 读取 Azure Active Directory 中的 policies.applicationConfiguration 属性。 |
| microsoft.aad.directory/policies/applicationConfiguration/default/update | 更新 Azure Active Directory 中的 policies.applicationConfiguration 属性。 |
| microsoft.aad.directory/policies/applicationConfiguration/create | 在 Azure Active Directory 中创建策略。 |
| microsoft.aad.directory/policies/applicationConfiguration/delete | 删除 Azure Active Directory 中的策略。 |
| microsoft.aad.directory/policies/applicationConfiguration/owners/read | 读取 Azure Active Directory 中的 policies.applicationConfiguration 属性。 |
| microsoft.aad.directory/policies/applicationConfiguration/owners/update | 更新 Azure Active Directory 中的 policies.applicationConfiguration 属性。 |
| microsoft.aad.directory/policies/applicationConfiguration/policyAppliedTo/read | 读取 Azure Active Directory 中的 policies.applicationConfiguration 属性。 |
| microsoft.aad.directory/servicePrincipals/default/update | 更新 Azure Active Directory 中 servicePrincipals 的基本属性。 |
| microsoft.aad.directory/servicePrincipals/create | 在 Azure Active Directory 中创建 servicePrincipals。 |
| microsoft.aad.directory/servicePrincipals/delete | 删除 Azure Active Directory 中的 servicePrincipals。 |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | 更新 Azure Active Directory 中的 servicePrincipals.appRoleAssignedTo 属性。 |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | 更新 Azure Active Directory 中的 servicePrincipals.appRoleAssignments 属性。 |
| microsoft.aad.directory/servicePrincipals/owners/update | 更新 Azure Active Directory 中的 servicePrincipals.owners 属性。 |
| microsoft.aad.directory/servicePrincipals/policies/update | 更新 Azure Active Directory 中的 servicePrincipals.policies 属性。 |
| microsoft.aad.directory/users/assignLicense | 管理 Azure Active Directory 中用户的许可证。 |
| microsoft.aad.reports/allEntities/read | 读取 Azure AD 报告。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 读取和配置 Azure 服务运行状况。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 创建和管理 Azure 支持票证。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 读取和配置 Office 365 服务运行状况。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 创建和管理 Office 365 支持票证。 |

### <a name="application-developer"></a>应用程序开发人员
可以创建独立于用户可以注册应用程序设置的应用程序注册。

  > [!NOTE]
  > 此角色从“目录读取者”角色继承其他权限。
  >
  >

| **操作** | **说明** |
| --- | --- |
| microsoft.aad.directory/applications/createAsOwner | 在 Azure Active Directory 中创建应用程序。 添加创建者作为第一个所有者，创建的对象根据创建者的 250 个创建对象配额计数。 |
| microsoft.aad.directory/appRoleAssignments/createAsOwner | 在 Azure Active Directory 中创建 appRoleAssignments。 添加创建者作为第一个所有者，创建的对象根据创建者的 250 个创建对象配额计数。 |
| microsoft.aad.directory/oAuth2PermissionGrants/createAsOwner | 在 Azure Active Directory 中创建 oAuth2PermissionGrants。 添加创建者作为第一个所有者，创建的对象根据创建者的 250 个创建对象配额计数。 |
| microsoft.aad.directory/servicePrincipals/createAsOwner | 在 Azure Active Directory 中创建 servicePrincipals。 添加创建者作为第一个所有者，创建的对象根据创建者的 250 个创建对象配额计数。 |

### <a name="billing-administrator"></a>计费管理员
可以执行与常见计费相关的任务，例如更新付款信息。

  > [!NOTE]
  > 此角色从“目录读取者”角色继承其他权限。
  >
  >

  > [!NOTE]
  > 此角色拥有 Azure Active Directory 外部的其他权限。 有关详细信息，请参阅上面的角色说明。
  >
  >

| **操作** | **说明** |
| --- | --- |
| microsoft.aad.directory/organization/default/update | 更新 Azure Active Directory 中组织的基本属性。 |
| microsoft.aad.directory/organization/trustedCAsForPasswordlessAuth/update | 更新 Azure Active Directory 中的 organization.trustedCAsForPasswordlessAuth 属性。 |
| microsoft.azure.accessService/allEntities/allTasks | 管理 Azure 访问服务的所有方面。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 读取和配置 Azure 服务运行状况。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 创建和管理 Azure 支持票证。 |
| microsoft.commerce.billing/allEntities/allTasks | 管理 Office 365 计费的各个方面。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 读取和配置 Office 365 服务运行状况。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 创建和管理 Office 365 支持票证。 |

### <a name="cloud-application-administrator"></a>云应用管理员
可以创建和管理应用注册和企业应用的所有方面，应用代理除外。

  > [!NOTE]
  > 此角色从“目录读取者”角色继承其他权限。
  >
  >

| **操作** | **说明** |
| --- | --- |
| microsoft.aad.directory/applications/audience/update | 更新 Azure Active Directory 中的 applications.audience 属性。 |
| microsoft.aad.directory/applications/authentication/update | 更新 Azure Active Directory 中的 applications.authentication 属性。 |
| microsoft.aad.directory/applications/default/update | 更新 Azure Active Directory 中应用程序的基本属性。 |
| microsoft.aad.directory/applications/create | 在 Azure Active Directory 中创建应用程序。 |
| microsoft.aad.directory/applications/credentials/update | 更新 Azure Active Directory 中的 applications.credentials 属性。 |
| microsoft.aad.directory/applications/delete | 删除 Azure Active Directory 中的应用程序。 |
| microsoft.aad.directory/applications/owners/update | 更新 Azure Active Directory 中的 applications.owners 属性。 |
| microsoft.aad.directory/applications/permissions/update | 更新 Azure Active Directory 中的 applications.permissions 属性。 |
| microsoft.aad.directory/applications/policies/update | 更新 Azure Active Directory 中的 applications.policies 属性。 |
| microsoft.aad.directory/appRoleAssignments/create | 在 Azure Active Directory 中创建 appRoleAssignments。 |
| microsoft.aad.directory/appRoleAssignments/update | 更新 Azure Active Directory 中的 appRoleAssignments。 |
| microsoft.aad.directory/appRoleAssignments/delete | 删除 Azure Active Directory 中的 appRoleAssignments。 |
| microsoft.aad.directory/policies/applicationConfiguration/create | 在 Azure Active Directory 中创建策略。 |
| microsoft.aad.directory/policies/applicationConfiguration/default/read | 读取 Azure Active Directory 中的 policies.applicationConfiguration 属性。 |
| microsoft.aad.directory/policies/applicationConfiguration/default/update | 更新 Azure Active Directory 中的 policies.applicationConfiguration 属性。 |
| microsoft.aad.directory/policies/applicationConfiguration/delete | 删除 Azure Active Directory 中的策略。 |
| microsoft.aad.directory/policies/applicationConfiguration/owners/read | 读取 Azure Active Directory 中的 policies.applicationConfiguration 属性。 |
| microsoft.aad.directory/policies/applicationConfiguration/owners/update | 更新 Azure Active Directory 中的 policies.applicationConfiguration 属性。 |
| microsoft.aad.directory/policies/applicationConfiguration/policyAppliedTo/read | 读取 Azure Active Directory 中的 policies.applicationConfiguration 属性。 |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | 更新 Azure Active Directory 中的 servicePrincipals.appRoleAssignedTo 属性。 |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | 更新 Azure Active Directory 中的 servicePrincipals.appRoleAssignments 属性。 |
| microsoft.aad.directory/servicePrincipals/default/update | 更新 Azure Active Directory 中 servicePrincipals 的基本属性。 |
| microsoft.aad.directory/servicePrincipals/create | 在 Azure Active Directory 中创建 servicePrincipals。 |
| microsoft.aad.directory/servicePrincipals/delete | 删除 Azure Active Directory 中的 servicePrincipals。 |
| microsoft.aad.directory/servicePrincipals/owners/update | 更新 Azure Active Directory 中的 servicePrincipals.owners 属性。 |
| microsoft.aad.directory/servicePrincipals/policies/update | 更新 Azure Active Directory 中的 servicePrincipals.policies 属性。 |
| microsoft.aad.directory/users/assignLicense | 管理 Azure Active Directory 中用户的许可证。 |
| microsoft.aad.reports/allEntities/read | 读取 Azure AD 报告。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 读取和配置 Azure 服务运行状况。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 创建和管理 Azure 支持票证。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 读取和配置 Office 365 服务运行状况。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 创建和管理 Office 365 支持票证。 |

### <a name="company-administrator"></a>公司管理员
可以管理 Azure AD 和使用 Azure AD 标识的 Microsoft 服务的所有方面。

  > [!NOTE]
  > 此角色从角色中继承其他权限。
  >
  >

  > [!NOTE]
  > 此角色拥有 Azure Active Directory 外部的其他权限。 有关详细信息，请参阅上面的角色说明。
  >
  >

| **操作** | **说明** |
| --- | --- |
| microsoft.aad.directory/administrativeUnits/allProperties/allTasks | 创建和删除 administrativeUnits，然后读取和更新 Azure Active Directory 中的所有属性。 |
| microsoft.aad.directory/applications/allProperties/allTasks | 创建和删除应用程序，然后读取和更新 Azure Active Directory 中的所有属性。 |
| microsoft.aad.directory/appRoleAssignments/allProperties/allTasks | 创建和删除 appRoleAssignments，然后读取和更新 Azure Active Directory 中的所有属性。 |
| microsoft.aad.directory/contacts/allProperties/allTasks | 创建和删除联系人，然后读取和更新 Azure Active Directory 中的所有属性。 |
| microsoft.aad.directory/contracts/allProperties/allTasks | 创建和删除协定，然后读取和更新 Azure Active Directory 中的所有属性。 |
| microsoft.aad.directory/devices/allProperties/allTasks | 创建和删除设备，然后读取和更新 Azure Active Directory 中的所有属性。 |
| microsoft.aad.directory/directoryRoles/allProperties/allTasks | 创建和删除 directoryRoles，然后读取和更新 Azure Active Directory 中的所有属性。 |
| microsoft.aad.directory/directoryRoleTemplates/allProperties/allTasks | 创建和删除 directoryRoleTemplates，然后读取和更新 Azure Active Directory 中的所有属性。 |
| microsoft.aad.directory/domains/allProperties/allTasks | 创建和删除域，然后读取和更新 Azure Active Directory 中的所有属性。 |
| microsoft.aad.directory/groups/allProperties/allTasks | 创建和删除组，然后读取和更新 Azure Active Directory 中的所有属性。 |
| microsoft.aad.directory/groupSettings/allProperties/allTasks | 创建和删除 groupSettings，然后读取和更新 Azure Active Directory 中的所有属性。 |
| microsoft.aad.directory/groupSettingTemplates/allProperties/allTasks | 创建和删除 groupSettingTemplates，然后读取和更新 Azure Active Directory 中的所有属性。 |
| microsoft.aad.directory/loginTenantBranding/allProperties/allTasks | 创建和删除 loginTenantBranding，然后读取和更新 Azure Active Directory 中的所有属性。 |
| microsoft.aad.directory/oAuth2PermissionGrants/allProperties/allTasks | 创建和删除 oAuth2PermissionGrants，然后读取和更新 Azure Active Directory 中的所有属性。 |
| microsoft.aad.directory/organization/allProperties/allTasks | 创建和删除组织，然后读取和更新 Azure Active Directory 中的所有属性。 |
| microsoft.aad.directory/policies/allProperties/allTasks | 创建和删除策略，然后读取和更新 Azure Active Directory 中的所有属性。 |
| microsoft.aad.directory/roleAssignments/allProperties/allTasks | 创建和删除 roleAssignments，然后读取和更新 Azure Active Directory 中的所有属性。 |
| microsoft.aad.directory/roleDefinitions/allProperties/allTasks | 创建和删除 roleDefinitions，然后读取和更新 Azure Active Directory 中的所有属性。 |
| microsoft.aad.directory/scopedRoleMemberships/allProperties/allTasks | 创建和删除 scopedRoleMemberships，然后读取和更新 Azure Active Directory 中的所有属性。 |
| microsoft.aad.directory/serviceAction/activateService | 可以在 Azure Active Directory 中执行 Activateservice 服务操作 |
| microsoft.aad.directory/serviceAction/disableDirectoryFeature | 可以在 Azure Active Directory 中执行 Disabledirectoryfeature 服务操作 |
| microsoft.aad.directory/serviceAction/enableDirectoryFeature | 可以在 Azure Active Directory 中执行 Enabledirectoryfeature 服务操作 |
| microsoft.aad.directory/serviceAction/getAvailableExtentionProperties | 可以在 Azure Active Directory 中执行 Getavailableextentionproperties 服务操作 |
| microsoft.aad.directory/servicePrincipals/allProperties/allTasks | 创建和删除 servicePrincipals，然后读取和更新 Azure Active Directory 中的所有属性。 |
| microsoft.aad.directory/subscribedSkus/allProperties/allTasks | 创建和删除 subscribedSkus，然后读取和更新 Azure Active Directory 中的所有属性。 |
| microsoft.aad.directory/users/allProperties/allTasks | 创建和删除用户，然后读取和更新 Azure Active Directory 中的所有属性。 |
| microsoft.aad.directorySync/allEntities/allTasks | 在 Azure AD Connect 中执行所有操作。 |
| microsoft.aad.identityProtection/allEntities/allTasks | 创建和删除所有资源，然后读取和更新 microsoft.aad.identityProtection 中的标准属性。 |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | 读取 microsoft.aad.privilegedIdentityManagement 中的所有资源。 |
| microsoft.aad.reports/allEntities/allTasks | 读取和配置 Azure AD 报告。 |
| microsoft.azure.accessService/allEntities/allTasks | 管理 Azure 访问服务的所有方面。 |
| microsoft.azure.informationProtection/allEntities/allTasks | 管理 Azure 信息保护的各个方面。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 读取和配置 Azure 服务运行状况。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 创建和管理 Azure 支持票证。 |
| microsoft.commerce.billing/allEntities/allTasks | 管理 Office 365 计费的各个方面。 |
| microsoft.intune/allEntities/allTasks | 管理 Intune 的各个方面。 |
| microsoft.office365.complianceManager/allEntities/allTasks | 管理 Office 365 合规性管理器的各个方面 |
| microsoft.office365.exchange/allEntities/allTasks | 管理 Exchange Online 的各个方面。 |
| microsoft.office365.lockbox/allEntities/allTasks | 管理 Office 365 客户密码箱的各个方面 |
| microsoft.powerApps.powerBI/allEntities/allTasks | 管理 Power BI 的各个方面。 |
| microsoft.office365.protectionCenter/allEntities/allTasks | 管理 Office 365 防护中心的各个方面。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 读取和配置 Office 365 服务运行状况。 |
| microsoft.office365.sharepoint/allEntities/allTasks | 创建和删除所有资源，然后读取和更新 microsoft.office365.sharepoint 中的标准属性。 |
| microsoft.office365.skypeForBusiness/allEntities/allTasks | 管理 Skype for Business Online 的各个方面。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 创建和管理 Office 365 支持票证。 |
| microsoft.powerApps.dynamics365/allEntities/allTasks | 管理 Dynamics 365 的各个方面。 |

### <a name="compliance-administrator"></a>符合性管理员
可以读取和管理 Azure AD 和 Office 365 中的符合性配置和报表。

  > [!NOTE]
  > 此角色从“目录读取者”角色继承其他权限。
  >
  >

  > [!NOTE]
  > 此角色拥有 Azure Active Directory 外部的其他权限。 有关详细信息，请参阅上面的角色说明。
  >
  >

| **操作** | **说明** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | 管理 Azure 访问服务的所有方面。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 读取和配置 Azure 服务运行状况。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 创建和管理 Azure 支持票证。 |
| microsoft.office365.complianceManager/allEntities/allTasks | 管理 Office 365 合规性管理器的各个方面 |
| microsoft.office365.exchange/allEntities/allTasks | 管理 Exchange Online 的各个方面。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 读取和配置 Office 365 服务运行状况。 |
| microsoft.office365.sharepoint/allEntities/allTasks | 创建和删除所有资源，然后读取和更新 microsoft.office365.sharepoint 中的标准属性。 |
| microsoft.office365.skypeForBusiness/allEntities/allTasks | 管理 Skype for Business Online 的各个方面。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 创建和管理 Office 365 支持票证。 |

### <a name="crm-service-administrator"></a>CRM 服务管理员
可以管理 Dynamics 365 产品的所有方面。

  > [!NOTE]
  > 此角色从“目录读取者”角色继承其他权限。
  >
  >

  > [!NOTE]
  > 此角色拥有 Azure Active Directory 外部的其他权限。 有关详细信息，请参阅上面的角色说明。
  >
  >

| **操作** | **说明** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | 管理 Azure 访问服务的所有方面。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 读取和配置 Azure 服务运行状况。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 创建和管理 Azure 支持票证。 |
| microsoft.powerApps.dynamics365/allEntities/allTasks | 管理 Dynamics 365 的各个方面。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 读取和配置 Office 365 服务运行状况。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 创建和管理 Office 365 支持票证。 |

### <a name="customer-lockbox-access-approver"></a>客户密码箱访问审批者
可以批准 Microsoft 支持人员访问客户组织数据的请求。

  > [!NOTE]
  > 此角色从“目录读取者”角色继承其他权限。
  >
  >

  > [!NOTE]
  > 此角色拥有 Azure Active Directory 外部的其他权限。 有关详细信息，请参阅上面的角色说明。
  >
  >

| **操作** | **说明** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | 管理 Azure 访问服务的所有方面。 |
| microsoft.office365.lockbox/allEntities/allTasks | 管理 Office 365 客户密码箱的各个方面 |

### <a name="exchange-service-administrator"></a>Exchange 服务管理员
可以管理 Exchange 产品的所有方面。

  > [!NOTE]
  > 此角色从“目录读取者”角色继承其他权限。
  >
  >

  > [!NOTE]
  > 此角色拥有 Azure Active Directory 外部的其他权限。 有关详细信息，请参阅上面的角色说明。
  >
  >

| **操作** | **说明** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | 管理 Azure 访问服务的所有方面。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 读取和配置 Azure 服务运行状况。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 创建和管理 Azure 支持票证。 |
| microsoft.office365.exchange/allEntities/allTasks | 管理 Exchange Online 的各个方面。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 读取和配置 Office 365 服务运行状况。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 创建和管理 Office 365 支持票证。 |

### <a name="guest-inviter"></a>来宾邀请者
可以独立于�成员可以邀请来宾�设置来邀请来宾用户。

  > [!NOTE]
  > 此角色从角色中继承其他权限。
  >
  >

| **操作** | **说明** |
| --- | --- |
| microsoft.aad.directory/users/appRoleAssignments/read | 读取 Azure Active Directory 中的 users.appRoleAssignments 属性。 |
| microsoft.aad.directory/users/default/read | 读取 Azure Active Directory 中用户的基本属性。 |
| microsoft.aad.directory/users/directReports/read | 读取 Azure Active Directory 中的 users.directReports 属性。 |
| microsoft.aad.directory/users/invitedBy/read | 读取 Azure Active Directory 中的 users.invitedBy 属性。 |
| microsoft.aad.directory/users/inviteGuest | 邀请 Azure Active Directory 中的来宾用户。 |
| microsoft.aad.directory/users/invitedUsers/read | 读取 Azure Active Directory 中的 users.invitedUsers 属性。 |
| microsoft.aad.directory/users/manager/read | 读取 Azure Active Directory 中的 users.manager 属性。 |
| microsoft.aad.directory/users/memberOf/read | 读取 Azure Active Directory 中的 users.memberOf 属性。 |
| microsoft.aad.directory/users/oAuth2PermissionGrants/default/read | 读取 Azure Active Directory 中的 users.oAuth2PermissionGrants 属性。 |
| microsoft.aad.directory/users/ownedDevices/read | 读取 Azure Active Directory 中的 users.ownedDevices 属性。 |
| microsoft.aad.directory/users/ownedObjects/read | 读取 Azure Active Directory 中的 users.ownedObjects 属性。 |
| microsoft.aad.directory/users/registeredDevices/read | 读取 Azure Active Directory 中的 users.registeredDevices 属性。 |

### <a name="helpdesk-administrator"></a>支持管理员
可以重置非管理员和支持理员的密码。

  > [!NOTE]
  > 此角色从“目录读取者”角色继承其他权限。
  >
  >

| **操作** | **说明** |
| --- | --- |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | 使 Azure Active Directory 中的所有用户刷新令牌无效。 |
| microsoft.aad.directory/users/password/update | 更新 Azure Active Directory 中所有用户的密码。 有关详细信息，请参阅联机文档。 |
| microsoft.azure.accessService/allEntities/allTasks | 管理 Azure 访问服务的所有方面。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 读取和配置 Azure 服务运行状况。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 创建和管理 Azure 支持票证。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 读取和配置 Office 365 服务运行状况。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 创建和管理 Office 365 支持票证。 |

### <a name="lync-service-administrator"></a>Lync 服务管理员
可以管理 Skype for Business 产品的所有方面。

  > [!NOTE]
  > 此角色从“目录读取者”角色继承其他权限。
  >
  >

  > [!NOTE]
  > 此角色拥有 Azure Active Directory 外部的其他权限。 有关详细信息，请参阅上面的角色说明。
  >
  >

| **操作** | **说明** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | 管理 Azure 访问服务的所有方面。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 读取和配置 Azure 服务运行状况。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 创建和管理 Azure 支持票证。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 读取和配置 Office 365 服务运行状况。 |
| microsoft.office365.skypeForBusiness/allEntities/allTasks | 管理 Skype for Business Online 的各个方面。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 创建和管理 Office 365 支持票证。 |

### <a name="power-bi-service-administrator"></a>Power BI 服务管理员
可以管理 Power BI 产品的所有方面。

  > [!NOTE]
  > 此角色从“目录读取者”角色继承其他权限。
  >
  >

  > [!NOTE]
  > 此角色拥有 Azure Active Directory 外部的其他权限。 有关详细信息，请参阅上面的角色说明。
  >
  >

| **操作** | **说明** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | 管理 Azure 访问服务的所有方面。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 读取和配置 Azure 服务运行状况。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 创建和管理 Azure 支持票证。 |
| microsoft.powerApps.powerBI/allEntities/allTasks | 管理 Power BI 的各个方面。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 读取和配置 Office 365 服务运行状况。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 创建和管理 Office 365 支持票证。 |

### <a name="privileged-role-administrator"></a>特权角色管理员
可以管理 Azure AD 中的角色分配

  > [!NOTE]
  > 此角色从“目录读取者”角色继承其他权限。
  >
  >

  > [!NOTE]
  > 此角色拥有 Azure Active Directory 外部的其他权限。 有关详细信息，请参阅上面的角色说明。
  >
  >

| **操作** | **说明** |
| --- | --- |
| microsoft.aad.directory/directoryRoles/update | 更新 Azure Active Directory 中的 directoryRoles。 |
| microsoft.aad.privilegedIdentityManagement/allEntities/allTasks | 创建和删除所有资源，然后读取和更新 microsoft.aad.privilegedIdentityManagement 中的标准属性。 |

### <a name="security-administrator"></a>安全管理员
可以读取安全信息和报告

  > [!NOTE]
  > 此角色从“目录读取者”角色继承其他权限。
  >
  >

  > [!NOTE]
  > 此角色拥有 Azure Active Directory 外部的其他权限。 有关详细信息，请参阅上面的角色说明。
  >
  >

| **操作** | **说明** |
| --- | --- |
| microsoft.aad.directory/applications/policies/update | 更新 Azure Active Directory 中的 applications.policies 属性。 |
| microsoft.aad.directory/policies/default/update | 更新 Azure Active Directory 中策略的基本属性。 |
| microsoft.aad.directory/policies/create | 在 Azure Active Directory 中创建策略。 |
| microsoft.aad.directory/policies/delete | 删除 Azure Active Directory 中的策略。 |
| microsoft.aad.directory/policies/owners/update | 更新 Azure Active Directory 中的 policies.owners 属性。 |
| microsoft.aad.directory/servicePrincipals/policies/update | 更新 Azure Active Directory 中的 servicePrincipals.policies 属性。 |
| microsoft.aad.identityProtection/allEntities/read | 读取 microsoft.aad.identityProtection 中的所有资源。 |
| microsoft.aad.identityProtection/allEntities/update | 更新 microsoft.aad.identityProtection 中的所有资源。 |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | 读取 microsoft.aad.privilegedIdentityManagement 中的所有资源。 |
| microsoft.azure.accessService/allEntities/allTasks | 管理 Azure 访问服务的所有方面。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 读取和配置 Azure 服务运行状况。 |
| microsoft.office365.protectionCenter/allEntities/read | 读取 Office 365 防护中心的各个方面。 |
| microsoft.office365.protectionCenter/allEntities/update | 更新 microsoft.office365.protectionCenter 中的所有资源。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 读取和配置 Office 365 服务运行状况。 |

### <a name="security-reader"></a>安全读取者
可以读取 Azure AD 和 Office 365 中的安全信息和报表。

  > [!NOTE]
  > 此角色从“目录读取者”角色继承其他权限。
  >
  >

  > [!NOTE]
  > 此角色拥有 Azure Active Directory 外部的其他权限。 有关详细信息，请参阅上面的角色说明。
  >
  >

| **操作** | **说明** |
| --- | --- |
| microsoft.aad.identityProtection/allEntities/read | 读取 microsoft.aad.identityProtection 中的所有资源。 |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | 读取 microsoft.aad.privilegedIdentityManagement 中的所有资源。 |
| microsoft.azure.accessService/allEntities/allTasks | 管理 Azure 访问服务的所有方面。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 读取和配置 Azure 服务运行状况。 |
| microsoft.office365.protectionCenter/allEntities/read | 读取 Office 365 防护中心的各个方面。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 读取和配置 Office 365 服务运行状况。 |

### <a name="service-support-administrator"></a>服务支持管理员
可以读取服务运行状况信息和管理支持票证。

  > [!NOTE]
  > 此角色从“目录读取者”角色继承其他权限。
  >
  >

  > [!NOTE]
  > 此角色拥有 Azure Active Directory 外部的其他权限。 有关详细信息，请参阅上面的角色说明。
  >
  >

| **操作** | **说明** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | 管理 Azure 访问服务的所有方面。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 读取和配置 Azure 服务运行状况。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 创建和管理 Azure 支持票证。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 读取和配置 Office 365 服务运行状况。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 创建和管理 Office 365 支持票证。 |

### <a name="sharepoint-service-administrator"></a>SharePoint 服务管理员
可以管理 SharePoint 服务的所有方面。

  > [!NOTE]
  > 此角色从“目录读取者”角色继承其他权限。
  >
  >

  > [!NOTE]
  > 此角色拥有 Azure Active Directory 外部的其他权限。 有关详细信息，请参阅上面的角色说明。
  >
  >

| **操作** | **说明** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | 管理 Azure 访问服务的所有方面。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 读取和配置 Azure 服务运行状况。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 创建和管理 Azure 支持票证。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 读取和配置 Office 365 服务运行状况。 |
| microsoft.office365.sharepoint/allEntities/allTasks | 创建和删除所有资源，然后读取和更新 microsoft.office365.sharepoint 中的标准属性。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 创建和管理 Office 365 支持票证。 |

### <a name="user-account-administrator"></a>用户帐户管理员
可以管理用户与组的所有方面

  > [!NOTE]
  > 此角色从“目录读取者”角色继承其他权限。
  >
  >

| **操作** | **说明** |
| --- | --- |
| microsoft.aad.directory/appRoleAssignments/create | 在 Azure Active Directory 中创建 appRoleAssignments。 |
| microsoft.aad.directory/appRoleAssignments/delete | 删除 Azure Active Directory 中的 appRoleAssignments。 |
| microsoft.aad.directory/appRoleAssignments/update | 更新 Azure Active Directory 中的 appRoleAssignments。 |
| microsoft.aad.directory/contacts/default/update | 更新 Azure Active Directory 中联系人的基本属性。 |
| microsoft.aad.directory/contacts/create | 在 Azure Active Directory 中创建联系人。 |
| microsoft.aad.directory/contacts/delete | 删除 Azure Active Directory 中的联系人。 |
| microsoft.aad.directory/groups/appRoleAssignments/update | 更新 Azure Active Directory 中的 groups.appRoleAssignments 属性。 |
| microsoft.aad.directory/groups/default/update | 更新 Azure Active Directory 中组的基本属性。 |
| microsoft.aad.directory/groups/create | 在 Azure Active Directory 中创建组。 |
| microsoft.aad.directory/groups/createAsOwner | 在 Azure Active Directory 中创建组。 添加创建者作为第一个所有者，创建的对象根据创建者的 250 个创建对象配额计数。 |
| microsoft.aad.directory/groups/delete | 删除 Azure Active Directory 中的组。 |
| microsoft.aad.directory/groups/hiddenMembers/read | 读取 Azure Active Directory 中的 groups.hiddenMembers 属性。 |
| microsoft.aad.directory/groups/members/update | 更新 Azure Active Directory 中的 groups.members 属性。 |
| microsoft.aad.directory/groups/owners/update | 更新 Azure Active Directory 中的 groups.owners 属性。 |
| microsoft.aad.directory/groups/restore | 还原 Azure Active Directory 中的组。 |
| microsoft.aad.directory/groups/settings/update | 更新 Azure Active Directory 中的 groups.settings 属性。 |
| microsoft.aad.directory/users/appRoleAssignments/update | 更新 Azure Active Directory 中的 users.appRoleAssignments 属性。 |
| microsoft.aad.directory/users/assignLicense | 管理 Azure Active Directory 中用户的许可证。 |
| microsoft.aad.directory/users/default/update | 更新 Azure Active Directory 中用户的基本属性。 |
| microsoft.aad.directory/users/create | 在 Azure Active Directory 中创建用户。 |
| microsoft.aad.directory/users/delete | 删除 Azure Active Directory 中的用户。 |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | 使 Azure Active Directory 中的所有用户刷新令牌无效。 |
| microsoft.aad.directory/users/manager/update | 更新 Azure Active Directory 中的 users.manager 属性。 |
| microsoft.aad.directory/users/password/update | 更新 Azure Active Directory 中所有用户的密码。 有关详细信息，请参阅联机文档。 |
| microsoft.aad.directory/users/restore | 还原 Azure Active Directory 中已删除的用户。 |
| microsoft.aad.directory/users/userPrincipalName/update | 更新 Azure Active Directory 中的 users.userPrincipalName 属性。 |
| microsoft.azure.accessService/allEntities/allTasks | 管理 Azure 访问服务的所有方面。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 读取和配置 Azure 服务运行状况。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 创建和管理 Azure 支持票证。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 读取和配置 Office 365 服务运行状况。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 创建和管理 Office 365 支持票证。 |

### <a name="user"></a>User
成员用户的默认角色。 可以读取所有目录信息并写入一组有限的目录信息。

  > [!NOTE]
  > 此角色从“目录读取者”角色继承其他权限。
  >
  >

| **操作** | **说明** |
| --- | --- |
| microsoft.aad.directory/applications/createAsOwner | 在 Azure Active Directory 中创建应用程序。 添加创建者作为第一个所有者，创建的对象根据创建者的 250 个创建对象配额计数。 |
| microsoft.aad.directory/groups/default/read | 读取 Azure Active Directory 中组的基本属性。 |
| microsoft.aad.directory/groups/createAsOwner | 在 Azure Active Directory 中创建组。 添加创建者作为第一个所有者，创建的对象根据创建者的 250 个创建对象配额计数。 |
| microsoft.aad.directory/oAuth2PermissionGrants/create | 在 Azure Active Directory 中创建 oAuth2PermissionGrants。 |
| microsoft.aad.directory/oAuth2PermissionGrants/delete | 删除 Azure Active Directory 中的 oAuth2PermissionGrants。 |
| microsoft.aad.directory/oAuth2PermissionGrants/update | 更新 Azure Active Directory 中的 oAuth2PermissionGrants。 |
| microsoft.aad.directory/servicePrincipals/createAsOwner | 在 Azure Active Directory 中创建 servicePrincipals。 添加创建者作为第一个所有者，创建的对象根据创建者的 250 个创建对象配额计数。 |
| microsoft.aad.directory/users/activateServicePlan | Azure Active Directory 中的 Activateserviceplan 用户。 |
| microsoft.aad.directory/users/inviteGuest | 邀请 Azure Active Directory 中的来宾用户。 |
| microsoft.aad.directory/applications/default/update | 更新 Azure Active Directory 中应用程序的基本属性。 |
| microsoft.aad.directory/applications/delete | 删除 Azure Active Directory 中的应用程序。 |
| microsoft.aad.directory/applications/owners/update | 更新 Azure Active Directory 中的 applications.owners 属性。 |
| microsoft.aad.directory/applications/permissions/update | 更新 Azure Active Directory 中的 applications.permissions 属性。 |
| microsoft.aad.directory/applications/policies/update | 更新 Azure Active Directory 中的 applications.policies 属性。 |
| microsoft.aad.directory/applications/restore | 还原 Azure Active Directory 中的应用程序。 |
| microsoft.aad.directory/devices/disable | 禁用 Azure Active Directory 中的设备。 |
| microsoft.aad.directory/groups/appRoleAssignments/update | 更新 Azure Active Directory 中的 groups.appRoleAssignments 属性。 |
| microsoft.aad.directory/groups/default/update | 更新 Azure Active Directory 中组的基本属性。 |
| microsoft.aad.directory/groups/delete | 删除 Azure Active Directory 中的组。 |
| microsoft.aad.directory/groups/dynamicMembershipRule/update | 更新 Azure Active Directory 中的 groups.dynamicMembershipRule 属性。 |
| microsoft.aad.directory/groups/members/update | 更新 Azure Active Directory 中的 groups.members 属性。 |
| microsoft.aad.directory/groups/owners/update | 更新 Azure Active Directory 中的 groups.owners 属性。 |
| microsoft.aad.directory/groups/restore | 还原 Azure Active Directory 中的组。 |
| microsoft.aad.directory/groups/settings/update | 更新 Azure Active Directory 中的 groups.settings 属性。 |
| microsoft.aad.directory/policies/default/update | 更新 Azure Active Directory 中策略的基本属性。 |
| microsoft.aad.directory/policies/delete | 删除 Azure Active Directory 中的策略。 |
| microsoft.aad.directory/policies/owners/update | 更新 Azure Active Directory 中的 policies.owners 属性。 |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | 更新 Azure Active Directory 中的 servicePrincipals.appRoleAssignedTo 属性。 |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | 更新 Azure Active Directory 中的 servicePrincipals.appRoleAssignments 属性。 |
| microsoft.aad.directory/servicePrincipals/default/update | 更新 Azure Active Directory 中 servicePrincipals 的基本属性。 |
| microsoft.aad.directory/servicePrincipals/delete | 删除 Azure Active Directory 中的 servicePrincipals。 |
| microsoft.aad.directory/servicePrincipals/owners/update | 更新 Azure Active Directory 中的 servicePrincipals.owners 属性。 |
| microsoft.aad.directory/servicePrincipals/policies/update | 更新 Azure Active Directory 中的 servicePrincipals.policies 属性。 |
| microsoft.aad.directory/users/changePassword | 更改 Azure Active Directory 中所有用户的密码。 有关详细信息，请参阅联机文档。 |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | 使 Azure Active Directory 中的所有用户刷新令牌无效。 |
| microsoft.aad.directory/users/basicProfile/update | 更新 Azure Active Directory 中的 users.basicProfile 属性。 |
| microsoft.aad.directory/users/mobile/update | 更新 Azure Active Directory 中的 users.mobile 属性。 |
| microsoft.aad.directory/users/searchableDeviceKey/update | 更新 Azure Active Directory 中的 users.searchableDeviceKey 属性。 |


## <a name="next-steps"></a>后续步骤

- 若要详细了解如何将用户分配为 Azure 订阅的管理员，请参阅[使用 RBAC 和 Azure 门户管理访问权限](../../role-based-access-control/role-assignments-portal.md)
- 若要了解有关如何在 Azure 中控制资源访问的详细信息，请参阅[了解 Azure 中的资源访问权限](../../role-based-access-control/rbac-and-directory-admin-roles.md)
