---
title: 管理员角色说明和权限 - Azure Active Directory | Microsoft Docs
description: 管理员角色可以添加用户、分配管理角色、重置用户密码、管理用户许可证，或者管理域。
services: active-directory
author: curtand
manager: mtillman
search.appverid: MET150
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
origin.date: 03/27/2019
ms.date: 04/11/2019
ms.author: v-junlch
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: e9238c620a4bff65e9081e10fd7896061f3327ce
ms.sourcegitcommit: cf8ad305433d47f9a6760f7a91ee361dc01573db
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/11/2019
ms.locfileid: "59502624"
---
# <a name="administrator-role-permissions-in-azure-active-directory"></a>Azure Active Directory 中的管理员角色权限

使用 Azure Active Directory (Azure AD) 时，可以指定有限的管理员以权限较低的角色来管理标识任务。 可以分配管理员来执行各种任务，例如，添加或更改用户、分配管理角色、重置用户密码、管理用户许可证，以及管理域名。 只能在 Azure AD 的用户设置中更改默认用户权限。

全局管理员有权使用所有管理功能。 默认情况下，系统会将注册 Azure 订阅的人员指派为目录的全局管理员角色。 只有全局管理员和特权角色管理员可以委托管理员角色。 为了降低业务风险，我们建议仅将此角色分配给你的公司中的少数人。


## <a name="assign-or-remove-administrator-roles"></a>分配或删除管理员角色

若要了解如何在 Azure Active Directory 中向用户分配管理角色，请参阅[在 Azure Active Directory 中查看和分配管理员角色](directory-manage-roles-portal.md)。

## <a name="available-roles"></a>可用的角色

提供以下管理员角色：

* **[应用程序管理员](#application-administrator)**：充当此角色的用户可以创建和管理企业应用程序、应用程序注册和应用程序代理设置的所有方面。 此角色还可以同意委派权限，以及除 Microsoft Graph 和 Azure AD Graph 之外的应用程序权限。 在创建新应用程序注册或企业应用程序时，不会将分配到此角色的用户添加为所有者。

  <b>重要说明</b>：此角色授予管理应用程序凭据这一功能。 分配有此角色的用户可以将凭据添加到应用程序，并使用这些凭据模拟应用程序的标识。 如果已向应用程序的标识授予 Azure Active Directory 访问权限，如创建或更新用户或其他对象，那么分配到此角色的用户在模拟应用程序时可以执行这些操作。 这种模拟应用程序标识的能力可能是用户在 Azure AD 中角色分配的基础上的权限提升。 请务必了解，向用户分配应用程序管理员角色，会赋予其模拟应用程序标识的能力。

* **[应用程序开发人员](#application-developer)**：在将设置“用户可以注册应用程序”设置为“否”时，充当此角色的用户可以创建应用程序注册。 当“用户可以同意应用代表他们访问公司数据”设置设为“否”时，此角色还能够代表自己授权同意。 在创建新应用程序注册或企业应用程序时，会将分配到此角色的用户添加为所有者。

- **[计费管理员](#billing-administrator)**：进行采购、管理订阅、管理支持票证，以及监视服务运行状况。

* **[云应用程序管理员](#cloud-application-administrator)**：充当此角色的用户具有与应用程序管理员角色相同的权限，但不包括管理应用程序代理的权限。 此角色授予创建和管理企业应用程序和应用程序注册的所有方面的权限。 此角色还可以同意委派权限，以及除 Microsoft Graph 和 Azure AD Graph 之外的应用程序权限。 在创建新应用程序注册或企业应用程序时，不会将分配到此角色的用户添加为所有者。

  <b>重要说明</b>：此角色授予管理应用程序凭据这一功能。 分配有此角色的用户可以将凭据添加到应用程序，并使用这些凭据模拟应用程序的标识。 如果已向应用程序的标识授予 Azure Active Directory 访问权限，如创建或更新用户或其他对象，那么分配到此角色的用户在模拟应用程序时可以执行这些操作。 这种模拟应用程序标识的能力可能是用户在 Azure AD 中角色分配的基础上的权限提升。 请务必了解，向用户分配云应用程序管理员角色，会赋予其模拟应用程序标识的能力。
- **[合规性管理员](#compliance-administrator)**：具有此角色的用户具有 Office 365 安全与合规中心以及 Exchange 管理中心的管理权限。 有关详细信息，请参阅 [About Office 365 admin roles](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d)（关于 Office 365 管理员角色）。

- **[Dynamics 365 服务管理员/CRM 服务管理员](#dynamics-365-service-administrator)**：具有此角色的用户具有 Microsoft Dynamics 365 Online 内的全局权限（如果该服务存在），并且能够管理支持票证和监视服务运行状况。 有关详细信息，请参阅[使用服务管理员角色管理租户](https://docs.microsoft.com/dynamics365/customer-engagement/admin/use-service-admin-role-manage-tenant)。

- **[Exchange 管理员](#exchange-administrator)**：具有此角色的用户具有 Microsoft Exchange Online 内的全局权限（如果该服务存在）。 还能够创建和管理所有 Office 365 组、管理支持票证和监视服务运行状况。 有关详细信息，请参阅 [About Office 365 admin roles](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d)（关于 Office 365 管理员角色）。
  > [!NOTE]
  > 在 Microsoft 图形 API、Azure AD 图形 API 和 Azure AD PowerShell 中，此角色标识为“Exchange 服务管理员”。 它是 Azure 门户中的“Exchange 管理员”。

- **[全局管理员/公司管理员](#company-administrator)**：具有此角色的用户有权访问 Azure Active Directory 以及使用 Azure Active Directory 标识的服务（例如 Exchange Online、SharePoint Online 和 Skype for Business Online）中的所有管理功能。 注册 Azure Active Directory 租户的人员将成为全局管理员。 只有全局管理员才能分配其他管理员角色。 公司中可以有多个全局管理员。 全局管理员可以为任何用户和所有其他管理员重置密码。

  > [!NOTE]
  > 在 Microsoft 图形 API、Azure AD 图形 API 和 Azure AD PowerShell 中，此角色标识为“公司管理员”。 它是 [Azure 门户](https://portal.azure.cn)中的“全局管理员”。
  >
  >

- **[来宾邀请者](#guest-inviter)**：此角色的用户可在“成员可以邀请”用户设置设置为“否”时管理 Azure Active Directory B2B 来宾用户邀请。 

- **[密码管理员/支持管理员](#helpdesk-administrator)**：具有此角色的用户可以更改密码、使刷新令牌失效、管理服务请求和监视服务运行状况。 使刷新令牌失效会强制用户重新登录。 支持性管理员可以重置密码，且对于不是管理员或具有以下角色的其他用户，支持性管理员可使其刷新令牌失效：
  - 来宾邀请者

  <b>重要说明</b>：具有此角色的用户可以更改可能有权访问 Azure Active Directory 内外敏感或私有信息或关键配置的用户的密码。 更改用户的密码可能意味着假定用户标识和权限的能力。 例如：
  - 应用程序注册和企业应用程序所有者，可以管理他们拥有的应用的凭据。 这些应用程序可能在 Azure AD 或其他位置拥有未授予支持人员管理员的特权。 通过此路径，支持人员管理员可能能够假定应用程序所有者的身份，然后通过更新应用程序的凭据来进一步假定特权应用程序的标识。
  - Azure 订阅所有者，可能对 Azure 中的敏感或私有信息或关键配置拥有访问权限。
  - 安全组和 Office 365 组所有者，可以管理组成员身份。 这些组可能会授予对 Azure AD 或其他位置敏感或私有信息或关键配置的访问权限。
  - Azure AD 之外的其他服务中的管理员，如 Exchange Online、Office 安全与合规中心以及人力资源系统。
  - 高级管理人员、法律顾问和人力资源员工之类的非管理员，可能有权访问敏感或私有信息。

  > [!NOTE]
  > 在 Microsoft 图形 API、Azure AD 图形 API 和 Azure AD PowerShell 中，此角色标识为“支持管理员”。 它是 [Azure 门户](https://portal.azure.cn/)中的“密码管理员”。
  >

- **[Power BI 管理员](#power-bi-service-administrator)**：具有此角色的用户具有 Power BI 内的全局权限（如果该服务存在），并且能够管理支持票证和监视服务运行状况。 有关详细信息，请参阅[了解 Power BI 管理员角色](https://docs.microsoft.com/power-bi/service-admin-role)。
  > [!NOTE]
  > 在 Microsoft 图形 API、Azure AD 图形 API 和 Azure AD PowerShell 中，此角色标识为“Power BI 服务管理员”。 它是 [Azure 门户](https://portal.azure.cn)中的“Power BI 管理员”。

- **[特权角色管理员](#privileged-role-administrator)**：具有此角色的用户可以管理 Azure Active Directory 和 Azure AD Privileged Identity Management 中的角色分配。 此外，此角色允许 Privileged Identity Management 的所有方面。

  <b>重要说明</b>：此角色授予管理所有 Azure AD 角色成员身份（包括全局管理员角色）的能力。 此角色不包括 Azure AD 中的任何其他权限功能，如创建或更新用户。 但是，分配到此角色的用户可通过分配其他角色，授予自己或其他人额外的特权。

- **[安全管理员](#security-administrator)**：具有此角色的用户具有安全读取者角色的所有只读权限，并且能够管理安全相关服务的配置：Azure Active Directory Identity Protection、Azure 信息保护和 Office 365 安全与合规中心。 [Office 365 安全与合规中心](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1)提供了有关 Office 365 权限的详细信息。


  |                                         In                                         |                                                                                               有权执行的操作                                                                                                |
  |------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
  |                             Identity Protection Center                             |                           <ul><li>安全读取者角色的所有权限。<li>此外，还能够执行除了重置密码以外的所有 IPC 操作。                           |
  |                           Privileged Identity Management                           |                                    <ul><li>安全读取者角色的所有权限。<li><strong>不能</strong>管理 Azure AD 角色成员身份或设置。                                    |
  | <p>监视 Office 365 服务运行状况</p><p>Office 365 安全与合规中心 | <ul><li>安全读取者角色的所有权限。<li>可以配置高级威胁防护功能中的所有设置（恶意软件和病毒保护、恶意 URL 配置、URL 跟踪等）。 |


- **[安全读取者](#security-reader)**：具有此角色的用户具有全局只读访问权限，包括 Azure Active Directory、Identity Protection、Privileged Identity Management，以及能够读取 Azure Active Directory 登录报告和审核日志中的所有信息。 角色还授予 Office 365 安全与合规中心的只读权限。 [Office 365 安全与合规中心](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1)提供了有关 Office 365 权限的详细信息。


  |                                           In                                           |                                                                                                                                                                                                                                                          有权执行的操作                                                                                                                                                                                                                                                           |
  |----------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
  |                               Identity Protection Center                               |                                                                                                                                                     读取安全功能的所有安全报告和设置信息<ul><li>反垃圾邮件<li>加密<li>数据丢失防护<li>反恶意软件<li>高级威胁防护<li>防网络钓鱼<li>邮件流规则                                                                                                                                                      |
  |                             Privileged Identity Management                             | <p>以只读方式访问 Azure AD PIM 中显示的所有信息：Azure AD 角色分配的策略和报告、安全审阅，以及在未来还可通过读取来访问 Azure AD 角色分配以外的方案的策略数据和报告。<p><strong>不能</strong>注册 Azure AD PIM 或对其进行任何更改。 担任此角色的人员可以在 PIM 的门户中或通过 PowerShell，为其他角色（例如，全局管理员或特权角色管理员）的候选用户激活角色。 |
  | <p>监视 Office 365 服务运行状况</p><p>Office 365 安全与合规中心</p> |                                                                                                                                                                          <ul><li>读取和管理警报<li>读取安全策略<li>读取威胁情报、云应用发现以及搜索和调查中的隔离区<li>读取所有报告                                                                                                                                                                          |


- **[服务支持管理员](#service-support-administrator)**：具有此角色的用户可为 Azure 和 Office 365 服务打开 Microsoft 支持请求，还可在 Azure 门户和 Office 365 管理门户中查看访问仪表板和消息中心。 有关详细信息，请参阅 [About Office 365 admin roles](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d)（关于 Office 365 管理员角色）。

- **[SharePoint 管理员](#sharepoint-administrator)**：具有此角色的用户在 Microsoft SharePoint Online（如果存在此服务）中拥有全局权限，并且能够创建和管理所有 Office 365 组、管理支持票证和监视服务运行状况。 有关详细信息，请参阅 [About Office 365 admin roles](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d)（关于 Office 365 管理员角色）。
  > [!NOTE]
  > 在 Microsoft 图形 API、Azure AD 图形 API 和 Azure AD PowerShell 中，此角色标识为“SharePoint 服务管理员”。 它是 Azure 门户中的“SharePoint 管理员”。

- **[Skype for Business/Lync 管理员](#skype-for-business-administrator)**：具有此角色的用户具有 Microsoft Skype for Business 中的全局权限，以及管理 Azure Active Directory 中的特定于 Skype 的用户属性。 此外，此角色可授予管理支持票证、监视服务运行状况以及访问 Teams 和 Skype for Business 管理中心的能力。 帐户必须获取 Teams 许可证，否则无法运行 Teams PowerShell cmdlet。 有关详细信息，请参阅[关于 Skype for Business 管理员角色](https://support.office.com/article/about-the-skype-for-business-admin-role-aeb35bda-93fc-49b1-ac2c-c74fbeb737b5)；有关 Teams 许可信息，请参阅 [Skype for Business 和 Microsoft Teams 附加许可](https://docs.microsoft.com/skypeforbusiness/skype-for-business-and-microsoft-teams-add-on-licensing/skype-for-business-and-microsoft-teams-add-on-licensing)

  > [!NOTE]
  > 在 Microsoft 图形 API、Azure AD 图形 API 和 Azure AD PowerShell 中，此角色标识为“Lync 服务管理员”。 它是 [Azure 门户](https://portal.azure.cn/)中的“Skype for Business 管理员”。

- **[Teams 通信管理员](#teams-communications-administrator)**：充当此角色的用户可以管理 Microsoft Teams 工作负荷的语音与电话相关方面。 这包括用于分配电话号码的管理工具、语音和会议策略，以及通话分析工具集的完全访问权限。

- **[Teams 通信支持工程师](#teams-communications-support-engineer)**：充当此角色的用户可以使用 Microsoft Teams 和 Skype for Business 管理中心的用户通话故障排除工具，来排查 Microsoft Teams 和 Skype for Business 中的通信问题。 充当此角色的用户可以查看所有参与方的完整通话记录信息。

- **[Teams 通信支持专家](#teams-communications-support-specialist)**：充当此角色的用户可以使用 Microsoft Teams 和 Skype for Business 管理中心的用户通话故障排除工具，来排查 Microsoft Teams 和 Skype for Business 中的通信问题。 充当此角色的用户只能查看他们所查找的特定用户的通话中的用户详细信息。

- **[Teams 管理员](#teams-administrator)**：充当此角色的用户可以通过 Microsoft Teams 和 Skype for Business 管理中心以及相应的 PowerShell 模块来管理 Microsoft Teams 工作负荷的所有方面。 这包括（但不限于）与电话、消息、会议和 Teams 自身相关的所有管理工具。 此外，此角色还能够创建和管理所有 Office 365 组、管理支持票证和监视服务运行状况。
  > [!NOTE]
  > 在 Microsoft 图形 API、Azure AD 图形 API 和 Azure AD PowerShell 中，此角色标识为“Teams 服务管理员”。 它是 [Azure 门户](https://portal.azure.cn)中的“Teams 管理员”。

- **[用户管理员](#user-administrator)**：具有此角色的用户可以创建用户和管理用户的所有方面，但具有一定限制（参阅下文）。 此外，具有此角色的用户可以创建和管理所有组。 此角色还能够创建和管理用户视图、管理支持票证和监视服务运行状况。

  | | |
  | --- | --- |
  |常规权限|<p>创建用户和组</p><p>创建和管理用户视图</p><p>管理 Office 支持票证|
  |<p>适用于所有用户，包括所有管理员</p>|<p>管理许可证</p><p>管理除用户主体名称之外的所有用户属性</p>
  |仅适用于不是管理员或具有以下任一管理员角色（权限有限）的用户：<ul><li>目录读者<li>来宾邀请者<li>支持管理员<li>消息中心读取者<li>报告读者<li>用户帐户管理员|<p>删除和还原</p><p>禁用和启用</p><p>使刷新令牌失效</p><p>管理包括用户主体名称在内的所有用户属性</p><p>重置密码</p><p>更新 (FIDO) 设备密钥</p>

  <b>重要说明</b>：具有此角色的用户可以更改可能有权访问 Azure Active Directory 内外敏感或私有信息或关键配置的用户的密码。 更改用户的密码可能意味着假定用户标识和权限的能力。 例如：
  - 应用程序注册和企业应用程序所有者，可以管理他们拥有的应用的凭据。 这些应用程序可能在 Azure AD 或其他位置拥有未授予用户管理员的特权。 通过此路径，用户管理员可能能够假定应用程序所有者的身份，然后通过更新应用程序的凭据来进一步假定特权应用程序的标识。
  - Azure 订阅所有者，可能对 Azure 中的敏感或私有信息或关键配置拥有访问权限。
  - 安全组和 Office 365 组所有者，可以管理组成员身份。 这些组可能会授予对 Azure AD 或其他位置敏感或私有信息或关键配置的访问权限。
  - Azure AD 之外的其他服务中的管理员，如 Exchange Online、Office 安全与合规中心以及人力资源系统。
  - 高级管理人员、法律顾问和人力资源员工之类的非管理员，可能有权访问敏感或私有信息。

## <a name="role-permissions"></a>角色权限
下表描述 Azure Active Directory 中授予每个角色的特定权限。 某些角色可能在 Azure Active Directory 外部的 Microsoft 服务中拥有其他权限。

### <a name="application-administrator"></a>应用程序管理员
可以创建和管理应用注册和企业应用的所有方面。

| **操作** | **说明** |
| --- | --- |
| microsoft.aad.directory/applications/audience/update | 更新 Azure Active Directory 中的 applications.audience 属性。 |
| microsoft.aad.directory/applications/authentication/update | 更新 Azure Active Directory 中的 applications.authentication 属性。 |
| microsoft.aad.directory/applications/basic/update | 更新 Azure Active Directory 中应用程序的基本属性。 |
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
| microsoft.aad.directory/auditLogs/allProperties/read | 读取 Azure Active Directory 中 auditLogs 上的所有属性（包括特权属性）。 |
| microsoft.aad.directory/policies/applicationConfiguration/basic/read | 读取 Azure Active Directory 中的 policies.applicationConfiguration 属性。 |
| microsoft.aad.directory/policies/applicationConfiguration/basic/update | 更新 Azure Active Directory 中的 policies.applicationConfiguration 属性。 |
| microsoft.aad.directory/policies/applicationConfiguration/create | 在 Azure Active Directory 中创建策略。 |
| microsoft.aad.directory/policies/applicationConfiguration/delete | 删除 Azure Active Directory 中的策略。 |
| microsoft.aad.directory/policies/applicationConfiguration/owners/read | 读取 Azure Active Directory 中的 policies.applicationConfiguration 属性。 |
| microsoft.aad.directory/policies/applicationConfiguration/owners/update | 更新 Azure Active Directory 中的 policies.applicationConfiguration 属性。 |
| microsoft.aad.directory/policies/applicationConfiguration/policyAppliedTo/read | 读取 Azure Active Directory 中的 policies.applicationConfiguration 属性。 |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | 更新 Azure Active Directory 中的 servicePrincipals.appRoleAssignedTo 属性。 |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | 更新 Azure Active Directory 中的 servicePrincipals.appRoleAssignments 属性。 |
| microsoft.aad.directory/servicePrincipals/audience/update | 更新 Azure Active Directory 中的 servicePrincipals.audience 属性。 |
| microsoft.aad.directory/servicePrincipals/authentication/update | 更新 Azure Active Directory 中的 servicePrincipals.authentication 属性。 |
| microsoft.aad.directory/servicePrincipals/basic/update | 更新 Azure Active Directory 中 servicePrincipals 的基本属性。 |
| microsoft.aad.directory/servicePrincipals/create | 在 Azure Active Directory 中创建 servicePrincipals。 |
| microsoft.aad.directory/servicePrincipals/credentials/update | 更新 Azure Active Directory 中的 servicePrincipals.credentials 属性。 |
| microsoft.aad.directory/servicePrincipals/delete | 删除 Azure Active Directory 中的 servicePrincipals。 |
| microsoft.aad.directory/servicePrincipals/owners/update | 更新 Azure Active Directory 中的 servicePrincipals.owners 属性。 |
| microsoft.aad.directory/servicePrincipals/permissions/update | 更新 Azure Active Directory 中的 servicePrincipals.permissions 属性。 |
| microsoft.aad.directory/servicePrincipals/policies/update | 更新 Azure Active Directory 中的 servicePrincipals.policies 属性。 |
| microsoft.aad.directory/signInReports/allProperties/read | 读取 Azure Active Directory 中 signInReports 上的所有属性（包括特权属性）。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 读取和配置 Azure 服务运行状况。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 创建和管理 Azure 支持票证。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 读取和配置 Office 365 服务运行状况。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 创建和管理 Office 365 支持票证。 |

### <a name="application-developer"></a>应用程序开发人员
可以创建独立于“用户可注册应用程序”设置的应用程序注册。

| **操作** | **说明** |
| --- | --- |
| microsoft.aad.directory/applications/createAsOwner | 在 Azure Active Directory 中创建应用程序。 添加创建者作为第一个所有者，创建的对象根据创建者的 250 个创建对象配额计数。 |
| microsoft.aad.directory/appRoleAssignments/createAsOwner | 在 Azure Active Directory 中创建 appRoleAssignments。 添加创建者作为第一个所有者，创建的对象根据创建者的 250 个创建对象配额计数。 |
| microsoft.aad.directory/oAuth2PermissionGrants/createAsOwner | 在 Azure Active Directory 中创建 oAuth2PermissionGrants。 添加创建者作为第一个所有者，创建的对象根据创建者的 250 个创建对象配额计数。 |
| microsoft.aad.directory/servicePrincipals/createAsOwner | 在 Azure Active Directory 中创建 servicePrincipals。 添加创建者作为第一个所有者，创建的对象根据创建者的 250 个创建对象配额计数。 |

### <a name="billing-administrator"></a>计费管理员
可以执行与常见计费相关的任务，例如更新付款信息。

  > [!NOTE]
  > 此角色拥有 Azure Active Directory 之外的其他权限。 有关详细信息，请参阅上面的角色说明。
  >
  >

| **操作** | **说明** |
| --- | --- |
| microsoft.aad.directory/organization/basic/update | 更新 Azure Active Directory 中组织的基本属性。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 读取和配置 Azure 服务运行状况。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 创建和管理 Azure 支持票证。 |
| microsoft.commerce.billing/allEntities/allTasks | 管理 Office 365 计费的各个方面。 |
| microsoft.office365.webPortal/allEntities/basic/read | 读取 microsoft.office365.webPortal 中所有资源的基本属性。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 读取和配置 Office 365 服务运行状况。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 创建和管理 Office 365 支持票证。 |

| microsoft.office365.serviceHealth/allEntities/allTasks | 读取和配置 Office 365 服务运行状况。 | | microsoft.office365.supportTickets/allEntities/allTasks | 创建和管理 Office 365 支持票证。 |

### <a name="cloud-application-administrator"></a>云应用管理员
可以创建和管理应用注册和企业应用的所有方面，应用代理除外。

| **操作** | **说明** |
| --- | --- |
| microsoft.aad.directory/applications/audience/update | 更新 Azure Active Directory 中的 applications.audience 属性。 |
| microsoft.aad.directory/applications/authentication/update | 更新 Azure Active Directory 中的 applications.authentication 属性。 |
| microsoft.aad.directory/applications/basic/update | 更新 Azure Active Directory 中应用程序的基本属性。 |
| microsoft.aad.directory/applications/create | 在 Azure Active Directory 中创建应用程序。 |
| microsoft.aad.directory/applications/credentials/update | 更新 Azure Active Directory 中的 applications.credentials 属性。 |
| microsoft.aad.directory/applications/delete | 删除 Azure Active Directory 中的应用程序。 |
| microsoft.aad.directory/applications/owners/update | 更新 Azure Active Directory 中的 applications.owners 属性。 |
| microsoft.aad.directory/applications/permissions/update | 更新 Azure Active Directory 中的 applications.permissions 属性。 |
| microsoft.aad.directory/applications/policies/update | 更新 Azure Active Directory 中的 applications.policies 属性。 |
| microsoft.aad.directory/appRoleAssignments/create | 在 Azure Active Directory 中创建 appRoleAssignments。 |
| microsoft.aad.directory/appRoleAssignments/update | 更新 Azure Active Directory 中的 appRoleAssignments。 |
| microsoft.aad.directory/appRoleAssignments/delete | 删除 Azure Active Directory 中的 appRoleAssignments。 |
| microsoft.aad.directory/auditLogs/allProperties/read | 读取 Azure Active Directory 中 auditLogs 上的所有属性（包括特权属性）。 |
| microsoft.aad.directory/policies/applicationConfiguration/create | 在 Azure Active Directory 中创建策略。 |
| microsoft.aad.directory/policies/applicationConfiguration/basic/read | 读取 Azure Active Directory 中的 policies.applicationConfiguration 属性。 |
| microsoft.aad.directory/policies/applicationConfiguration/basic/update | 更新 Azure Active Directory 中的 policies.applicationConfiguration 属性。 |
| microsoft.aad.directory/policies/applicationConfiguration/delete | 删除 Azure Active Directory 中的策略。 |
| microsoft.aad.directory/policies/applicationConfiguration/owners/read | 读取 Azure Active Directory 中的 policies.applicationConfiguration 属性。 |
| microsoft.aad.directory/policies/applicationConfiguration/owners/update | 更新 Azure Active Directory 中的 policies.applicationConfiguration 属性。 |
| microsoft.aad.directory/policies/applicationConfiguration/policyAppliedTo/read | 读取 Azure Active Directory 中的 policies.applicationConfiguration 属性。 |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | 更新 Azure Active Directory 中的 servicePrincipals.appRoleAssignedTo 属性。 |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | 更新 Azure Active Directory 中的 servicePrincipals.appRoleAssignments 属性。 |
| microsoft.aad.directory/servicePrincipals/audience/update | 更新 Azure Active Directory 中的 servicePrincipals.audience 属性。 |
| microsoft.aad.directory/servicePrincipals/authentication/update | 更新 Azure Active Directory 中的 servicePrincipals.authentication 属性。 |
| microsoft.aad.directory/servicePrincipals/basic/update | 更新 Azure Active Directory 中 servicePrincipals 的基本属性。 |
| microsoft.aad.directory/servicePrincipals/create | 在 Azure Active Directory 中创建 servicePrincipals。 |
| microsoft.aad.directory/servicePrincipals/credentials/update | 更新 Azure Active Directory 中的 servicePrincipals.credentials 属性。 |
| microsoft.aad.directory/servicePrincipals/delete | 删除 Azure Active Directory 中的 servicePrincipals。 |
| microsoft.aad.directory/servicePrincipals/owners/update | 更新 Azure Active Directory 中的 servicePrincipals.owners 属性。 |
| microsoft.aad.directory/servicePrincipals/permissions/update | 更新 Azure Active Directory 中的 servicePrincipals.permissions 属性。 |
| microsoft.aad.directory/servicePrincipals/policies/update | 更新 Azure Active Directory 中的 servicePrincipals.policies 属性。 |
| microsoft.aad.directory/signInReports/allProperties/read | 读取 Azure Active Directory 中 signInReports 上的所有属性（包括特权属性）。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 读取和配置 Azure 服务运行状况。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 创建和管理 Azure 支持票证。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 读取和配置 Office 365 服务运行状况。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 创建和管理 Office 365 支持票证。 |

### <a name="company-administrator"></a>公司管理员
可以管理 Azure AD 和使用 Azure AD 标识的 Microsoft 服务的所有方面。

  > [!NOTE]
  > 此角色拥有 Azure Active Directory 之外的其他权限。 有关详细信息，请参阅上面的角色说明。
  >
  >

| **操作** | **说明** |
| --- | --- |
| microsoft.aad.cloudAppSecurity/allEntities/allTasks | 创建和删除所有资源，然后读取并更新 microsoft.aad.cloudAppSecurity 中的标准属性。 |
| microsoft.aad.directory/administrativeUnits/allProperties/allTasks | 创建和删除 administrativeUnits，然后读取和更新 Azure Active Directory 中的所有属性。 |
| microsoft.aad.directory/applications/allProperties/allTasks | 创建和删除应用程序，然后读取和更新 Azure Active Directory 中的所有属性。 |
| microsoft.aad.directory/appRoleAssignments/allProperties/allTasks | 创建和删除 appRoleAssignments，然后读取和更新 Azure Active Directory 中的所有属性。 |
| microsoft.aad.directory/auditLogs/allProperties/read | 读取 Azure Active Directory 中 auditLogs 上的所有属性（包括特权属性）。 |
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
| microsoft.aad.directory/signInReports/allProperties/read | 读取 Azure Active Directory 中 signInReports 上的所有属性（包括特权属性）。 |
| microsoft.aad.directory/subscribedSkus/allProperties/allTasks | 创建和删除 subscribedSkus，然后读取和更新 Azure Active Directory 中的所有属性。 |
| microsoft.aad.directory/users/allProperties/allTasks | 创建和删除用户，然后读取和更新 Azure Active Directory 中的所有属性。 |
| microsoft.aad.directorySync/allEntities/allTasks | 在 Azure AD Connect 中执行所有操作。 |
| microsoft.aad.identityProtection/allEntities/allTasks | 创建和删除所有资源，然后读取和更新 microsoft.aad.identityProtection 中的标准属性。 |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | 读取 microsoft.aad.privilegedIdentityManagement 中的所有资源。 |
| microsoft.azure.advancedThreatProtection/allEntities/read | 读取 microsoft.azure.advancedThreatProtection 中的所有资源。 |
| microsoft.azure.informationProtection/allEntities/allTasks | 管理 Azure 信息保护的各个方面。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 读取和配置 Azure 服务运行状况。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 创建和管理 Azure 支持票证。 |
| microsoft.commerce.billing/allEntities/allTasks | 管理 Office 365 计费的各个方面。 |
| microsoft.intune/allEntities/allTasks | 管理 Intune 的各个方面。 |
| microsoft.office365.complianceManager/allEntities/allTasks | 管理 Office 365 合规性管理器的各个方面 |
| microsoft.office365.desktopAnalytics/allEntities/allTasks | 管理桌面分析的各个方面。 |
| microsoft.office365.exchange/allEntities/allTasks | 管理 Exchange Online 的各个方面。 |
| microsoft.office365.lockbox/allEntities/allTasks | 管理 Office 365 客户密码箱的各个方面 |
| microsoft.office365.messageCenter/messages/read | 读取 microsoft.office365.messageCenter 中的消息。 |
| microsoft.office365.messageCenter/securityMessages/read | 读取 microsoft.office365.messageCenter 中的安全消息。 |
| microsoft.office365.protectionCenter/allEntities/allTasks | 管理 Office 365 防护中心的各个方面。 |
| microsoft.office365.securityComplianceCenter/allEntities/allTasks | 创建和删除所有资源，然后读取和更新 microsoft.office365.securityComplianceCenter 中的标准属性。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 读取和配置 Office 365 服务运行状况。 |
| microsoft.office365.sharepoint/allEntities/allTasks | 创建和删除所有资源，然后读取和更新 microsoft.office365.sharepoint 中的标准属性。 |
| microsoft.office365.skypeForBusiness/allEntities/allTasks | 管理 Skype for Business Online 的各个方面。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 创建和管理 Office 365 支持票证。 |
| microsoft.office365.usageReports/allEntities/read | 阅读 Office 365 使用情况报告。 |
| microsoft.office365.webPortal/allEntities/basic/read | 读取 microsoft.office365.webPortal 中所有资源的基本属性。 |
| microsoft.powerApps.dynamics365/allEntities/allTasks | 管理 Dynamics 365 的各个方面。 |
| microsoft.powerApps.powerBI/allEntities/allTasks | 管理 Power BI 的各个方面。 |
| microsoft.windows.defenderAdvancedThreatProtection/allEntities/read | 读取 microsoft.windows.defenderAdvancedThreatProtection 中的所有资源。 |

### <a name="compliance-administrator"></a>符合性管理员
可以读取和管理 Azure AD 和 Office 365 中的符合性配置和报表。

  > [!NOTE]
  > 此角色拥有 Azure Active Directory 之外的其他权限。 有关详细信息，请参阅上面的角色说明。
  >
  >

| **操作** | **说明** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | 读取和配置 Azure 服务运行状况。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 创建和管理 Azure 支持票证。 |
| microsoft.office365.webPortal/allEntities/basic/read | 读取 microsoft.office365.webPortal 中所有资源的基本属性。 |
| microsoft.office365.complianceManager/allEntities/allTasks | 管理 Office 365 合规性管理器的各个方面 |
| microsoft.office365.exchange/allEntities/allTasks | 管理 Exchange Online 的各个方面。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 读取和配置 Office 365 服务运行状况。 |
| microsoft.office365.sharepoint/allEntities/allTasks | 创建和删除所有资源，然后读取和更新 microsoft.office365.sharepoint 中的标准属性。 |
| microsoft.office365.skypeForBusiness/allEntities/allTasks | 管理 Skype for Business Online 的各个方面。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 创建和管理 Office 365 支持票证。 |

### <a name="crm-service-administrator"></a>CRM 服务管理员
可以管理 Dynamics 365 产品的所有方面。

  > [!NOTE]
  > 此角色拥有 Azure Active Directory 之外的其他权限。 有关详细信息，请参阅上面的角色说明。
  >
  >

| **操作** | **说明** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | 读取和配置 Azure 服务运行状况。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 创建和管理 Azure 支持票证。 |
| microsoft.powerApps.dynamics365/allEntities/allTasks | 管理 Dynamics 365 的各个方面。 |
| microsoft.office365.webPortal/allEntities/basic/read | 读取 microsoft.office365.webPortal 中所有资源的基本属性。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 读取和配置 Office 365 服务运行状况。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 创建和管理 Office 365 支持票证。 |

### <a name="customer-lockbox-access-approver"></a>客户密码箱访问审批者
可以批准 Microsoft 支持人员访问客户组织数据的请求。 此角色无权查看、创建或管理支持票证。

  > [!NOTE]
  > 此角色拥有 Azure Active Directory 之外的其他权限。 有关详细信息，请参阅上面的角色说明。
  >
  >

| **操作** | **说明** |
| --- | --- |
| microsoft.office365.webPortal/allEntities/basic/read | 读取 microsoft.office365.webPortal 中所有资源的基本属性。 |
| microsoft.office365.lockbox/allEntities/allTasks | 管理 Office 365 客户密码箱的各个方面 |

### <a name="exchange-service-administrator"></a>Exchange 服务管理员
可以管理 Exchange 产品的所有方面。

  > [!NOTE]
  > 此角色拥有 Azure Active Directory 之外的其他权限。 有关详细信息，请参阅上面的角色说明。
  >
  >

| **操作** | **说明** |
| --- | --- |
| microsoft.aad.directory/groups/unified/appRoleAssignments/update | 更新 Azure Active Directory 中的 groups.unified 属性。 |
| microsoft.aad.directory/groups/unified/basic/update | 更新 Office 365 组的基本属性。 |
| microsoft.aad.directory/groups/unified/create | 创建 Office 365 组。 |
| microsoft.aad.directory/groups/unified/delete | 删除 Office 365 组。 |
| microsoft.aad.directory/groups/unified/members/update | 更新 Office 365 组的成员身份。 |
| microsoft.aad.directory/groups/unified/owners/update | 更新 Office 365 组的所有权。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 读取和配置 Azure 服务运行状况。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 创建和管理 Azure 支持票证。 |
| microsoft.office365.webPortal/allEntities/basic/read | 读取 microsoft.office365.webPortal 中所有资源的基本属性。 |
| microsoft.office365.exchange/allEntities/allTasks | 管理 Exchange Online 的各个方面。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 读取和配置 Office 365 服务运行状况。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 创建和管理 Office 365 支持票证。 |

### <a name="guest-inviter"></a>来宾邀请者
可以邀请与“成员可邀请来宾”设置无关的来宾用户。

| **操作** | **说明** |
| --- | --- |
| microsoft.aad.directory/users/appRoleAssignments/read | 读取 Azure Active Directory 中的 users.appRoleAssignments 属性。 |
| microsoft.aad.directory/users/basic/read | 读取 Azure Active Directory 中用户的基本属性。 |
| microsoft.aad.directory/users/directReports/read | 读取 Azure Active Directory 中的 users.directReports 属性。 |
| microsoft.aad.directory/users/inviteGuest | 邀请 Azure Active Directory 中的来宾用户。 |
| microsoft.aad.directory/users/manager/read | 读取 Azure Active Directory 中的 users.manager 属性。 |
| microsoft.aad.directory/users/memberOf/read | 读取 Azure Active Directory 中的 users.memberOf 属性。 |
| microsoft.aad.directory/users/oAuth2PermissionGrants/basic/read | 读取 Azure Active Directory 中的 users.oAuth2PermissionGrants 属性。 |
| microsoft.aad.directory/users/ownedDevices/read | 读取 Azure Active Directory 中的 users.ownedDevices 属性。 |
| microsoft.aad.directory/users/ownedObjects/read | 读取 Azure Active Directory 中的 users.ownedObjects 属性。 |
| microsoft.aad.directory/users/registeredDevices/read | 读取 Azure Active Directory 中的 users.registeredDevices 属性。 |

### <a name="helpdesk-administrator"></a>支持管理员
可以重置非管理员和支持理员的密码。

| **操作** | **说明** |
| --- | --- |
| microsoft.aad.directory/devices/bitLockerRecoveryKeys/read | 读取 Azure Active Directory 中的 devices.bitLockerRecoveryKeys 属性。 |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | 使 Azure Active Directory 中的所有用户刷新令牌无效。 |
| microsoft.aad.directory/users/password/update | 更新 Azure Active Directory 中所有用户的密码。 有关详细信息，请参阅联机文档。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 读取和配置 Azure 服务运行状况。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 创建和管理 Azure 支持票证。 |
| microsoft.office365.webPortal/allEntities/basic/read | 读取 microsoft.office365.webPortal 中所有资源的基本属性。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 读取和配置 Office 365 服务运行状况。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 创建和管理 Office 365 支持票证。 |

### <a name="intune-service-administrator"></a>Intune 服务管理员
可以管理 Intune 产品的所有方面。

  > [!NOTE]
  > 此角色拥有 Azure Active Directory 之外的其他权限。 有关详细信息，请参阅上面的角色说明。
  >
  >

| **操作** | **说明** |
| --- | --- |
| microsoft.aad.directory/contacts/basic/update | 更新 Azure Active Directory 中联系人的基本属性。 |
| microsoft.aad.directory/contacts/create | 在 Azure Active Directory 中创建联系人。 |
| microsoft.aad.directory/contacts/delete | 删除 Azure Active Directory 中的联系人。 |
| microsoft.aad.directory/devices/basic/update | 更新 Azure Active Directory 中设备的基本属性。 |
| microsoft.aad.directory/devices/bitLockerRecoveryKeys/read | 读取 Azure Active Directory 中的 devices.bitLockerRecoveryKeys 属性。 |
| microsoft.aad.directory/devices/create | 在 Azure Active Directory 中创建设备。 |
| microsoft.aad.directory/devices/delete | 删除 Azure Active Directory 中的设备。 |
| microsoft.aad.directory/devices/registeredOwners/update | 更新 Azure Active Directory 中的 devices.registeredOwners 属性。 |
| microsoft.aad.directory/devices/registeredUsers/update | 更新 Azure Active Directory 中的 devices.registeredUsers 属性。 |
| microsoft.aad.directory/groups/appRoleAssignments/update | 更新 Azure Active Directory 中的 groups.appRoleAssignments 属性。 |
| microsoft.aad.directory/groups/basic/update | 更新 Azure Active Directory 中组的基本属性。 |
| microsoft.aad.directory/groups/create | 在 Azure Active Directory 中创建组。 |
| microsoft.aad.directory/groups/createAsOwner | 在 Azure Active Directory 中创建组。 添加创建者作为第一个所有者，创建的对象根据创建者的 250 个创建对象配额计数。 |
| microsoft.aad.directory/groups/delete | 删除 Azure Active Directory 中的组。 |
| microsoft.aad.directory/groups/hiddenMembers/read | 读取 Azure Active Directory 中的 groups.hiddenMembers 属性。 |
| microsoft.aad.directory/groups/members/update | 更新 Azure Active Directory 中的 groups.members 属性。 |
| microsoft.aad.directory/groups/owners/update | 更新 Azure Active Directory 中的 groups.owners 属性。 |
| microsoft.aad.directory/groups/restore | 还原 Azure Active Directory 中的组。 |
| microsoft.aad.directory/groups/settings/update | 更新 Azure Active Directory 中的 groups.settings 属性。 |
| microsoft.aad.directory/users/appRoleAssignments/update | 更新 Azure Active Directory 中的 users.appRoleAssignments 属性。 |
| microsoft.aad.directory/users/basic/update | 更新 Azure Active Directory 中用户的基本属性。 |
| microsoft.aad.directory/users/manager/update | 更新 Azure Active Directory 中的 users.manager 属性。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 创建和管理 Azure 支持票证。 |
| microsoft.intune/allEntities/allTasks | 管理 Intune 的各个方面。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 创建和管理 Office 365 支持票证。 |
| microsoft.office365.webPortal/allEntities/basic/read | 读取 microsoft.office365.webPortal 中所有资源的基本属性。 |

### <a name="license-administrator"></a>许可证管理员
可以管理用户和组的产品许可证。

| **操作** | **说明** |
| --- | --- |
| microsoft.aad.directory/users/assignLicense | 管理 Azure Active Directory 中用户的许可证。 |
| microsoft.aad.directory/users/usageLocation/update | 更新 Azure Active Directory 中的 users.usageLocation 属性。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 读取和配置 Azure 服务运行状况。 |
| microsoft.office365.webPortal/allEntities/basic/read | 读取 microsoft.office365.webPortal 中所有资源的基本属性。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 读取和配置 Office 365 服务运行状况。 |

### <a name="lync-service-administrator"></a>Lync 服务管理员
可以管理 Skype for Business 产品的所有方面。

  > [!NOTE]
  > 此角色拥有 Azure Active Directory 之外的其他权限。 有关详细信息，请参阅上面的角色说明。
  >
  >

| **操作** | **说明** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | 读取和配置 Azure 服务运行状况。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 创建和管理 Azure 支持票证。 |
| microsoft.office365.webPortal/allEntities/basic/read | 读取 microsoft.office365.webPortal 中所有资源的基本属性。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 读取和配置 Office 365 服务运行状况。 |
| microsoft.office365.skypeForBusiness/allEntities/allTasks | 管理 Skype for Business Online 的各个方面。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 创建和管理 Office 365 支持票证。 |

### <a name="power-bi-service-administrator"></a>Power BI 服务管理员
可以管理 Power BI 产品的所有方面。

  > [!NOTE]
  > 此角色拥有 Azure Active Directory 之外的其他权限。 有关详细信息，请参阅上面的角色说明。
  >
  >

| **操作** | **说明** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | 读取和配置 Azure 服务运行状况。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 创建和管理 Azure 支持票证。 |
| microsoft.powerApps.powerBI/allEntities/allTasks | 管理 Power BI 的各个方面。 |
| microsoft.office365.webPortal/allEntities/basic/read | 读取 microsoft.office365.webPortal 中所有资源的基本属性。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 读取和配置 Office 365 服务运行状况。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 创建和管理 Office 365 支持票证。 |

### <a name="privileged-role-administrator"></a>特权角色管理员
可以管理 Azure AD 中的角色分配和 Privileged Identity Management 的所有方面。

  > [!NOTE]
  > 此角色拥有 Azure Active Directory 之外的其他权限。 有关详细信息，请参阅上面的角色说明。
  >
  >

| **操作** | **说明** |
| --- | --- |
| microsoft.aad.directory/directoryRoles/update | 更新 Azure Active Directory 中的 directoryRoles。 |
| microsoft.aad.privilegedIdentityManagement/allEntities/allTasks | 创建和删除所有资源，然后读取和更新 microsoft.aad.privilegedIdentityManagement 中的标准属性。 |

### <a name="reports-reader"></a>报告读者
可以读取登录和审核报告。

  > [!NOTE]
  > 此角色拥有 Azure Active Directory 之外的其他权限。 有关详细信息，请参阅上面的角色说明。
  >
  >

| **操作** | **说明** |
| --- | --- |
| microsoft.aad.directory/auditLogs/allProperties/read | 读取 Azure Active Directory 中 auditLogs 上的所有属性（包括特权属性）。 |
| microsoft.aad.directory/signInReports/allProperties/read | 读取 Azure Active Directory 中 signInReports 上的所有属性（包括特权属性）。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 读取和配置 Azure 服务运行状况。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 读取和配置 Office 365 服务运行状况。 |
| microsoft.office365.usageReports/allEntities/read | 阅读 Office 365 使用情况报告。 |

### <a name="security-administrator"></a>安全管理员
可以读取安全信息和报表，以及管理 Azure AD 和 Office 365 中的配置。

  > [!NOTE]
  > 此角色拥有 Azure Active Directory 之外的其他权限。 有关详细信息，请参阅上面的角色说明。
  >
  >

| **操作** | **说明** |
| --- | --- |
| microsoft.aad.directory/applications/policies/update | 更新 Azure Active Directory 中的 applications.policies 属性。 |
| microsoft.aad.directory/auditLogs/allProperties/read | 读取 Azure Active Directory 中 auditLogs 上的所有属性（包括特权属性）。 |
| microsoft.aad.directory/devices/bitLockerRecoveryKeys/read | 读取 Azure Active Directory 中的 devices.bitLockerRecoveryKeys 属性。 |
| microsoft.aad.directory/policies/basic/update | 更新 Azure Active Directory 中策略的基本属性。 |
| microsoft.aad.directory/policies/create | 在 Azure Active Directory 中创建策略。 |
| microsoft.aad.directory/policies/delete | 删除 Azure Active Directory 中的策略。 |
| microsoft.aad.directory/policies/owners/update | 更新 Azure Active Directory 中的 policies.owners 属性。 |
| microsoft.aad.directory/policies/tenantDefault/update | 更新 Azure Active Directory 中的 policies.tenantDefault 属性。 |
| microsoft.aad.directory/servicePrincipals/policies/update | 更新 Azure Active Directory 中的 servicePrincipals.policies 属性。 |
| microsoft.aad.directory/signInReports/allProperties/read | 读取 Azure Active Directory 中 signInReports 上的所有属性（包括特权属性）。 |
| microsoft.aad.identityProtection/allEntities/read | 读取 microsoft.aad.identityProtection 中的所有资源。 |
| microsoft.aad.identityProtection/allEntities/update | 更新 microsoft.aad.identityProtection 中的所有资源。 |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | 读取 microsoft.aad.privilegedIdentityManagement 中的所有资源。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 读取和配置 Azure 服务运行状况。 |
| microsoft.office365.webPortal/allEntities/basic/read | 读取 microsoft.office365.webPortal 中所有资源的基本属性。 |
| microsoft.office365.protectionCenter/allEntities/read | 读取 Office 365 防护中心的各个方面。 |
| microsoft.office365.protectionCenter/allEntities/update | 更新 microsoft.office365.protectionCenter 中的所有资源。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 读取和配置 Office 365 服务运行状况。 |

### <a name="security-reader"></a>安全读取者
可以读取 Azure AD 和 Office 365 中的安全信息和报表。

  > [!NOTE]
  > 此角色拥有 Azure Active Directory 之外的其他权限。 有关详细信息，请参阅上面的角色说明。
  >
  >

| **操作** | **说明** |
| --- | --- |
| microsoft.aad.directory/auditLogs/allProperties/read | 读取 Azure Active Directory 中 auditLogs 上的所有属性（包括特权属性）。 |
| microsoft.aad.directory/devices/bitLockerRecoveryKeys/read | 读取 Azure Active Directory 中的 devices.bitLockerRecoveryKeys 属性。 |
| microsoft.aad.directory/signInReports/allProperties/read | 读取 Azure Active Directory 中 signInReports 上的所有属性（包括特权属性）。 |
| microsoft.aad.identityProtection/allEntities/read | 读取 microsoft.aad.identityProtection 中的所有资源。 |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | 读取 microsoft.aad.privilegedIdentityManagement 中的所有资源。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 读取和配置 Azure 服务运行状况。 |
| microsoft.office365.webPortal/allEntities/basic/read | 读取 microsoft.office365.webPortal 中所有资源的基本属性。 |
| microsoft.office365.protectionCenter/allEntities/read | 读取 Office 365 防护中心的各个方面。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 读取和配置 Office 365 服务运行状况。 |

### <a name="service-support-administrator"></a>服务支持管理员
可以读取服务运行状况信息和管理支持票证。

  > [!NOTE]
  > 此角色拥有 Azure Active Directory 之外的其他权限。 有关详细信息，请参阅上面的角色说明。
  >
  >

| **操作** | **说明** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | 读取和配置 Azure 服务运行状况。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 创建和管理 Azure 支持票证。 |
| microsoft.office365.webPortal/allEntities/basic/read | 读取 microsoft.office365.webPortal 中所有资源的基本属性。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 读取和配置 Office 365 服务运行状况。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 创建和管理 Office 365 支持票证。 |

### <a name="sharepoint-service-administrator"></a>SharePoint 服务管理员
可以管理 SharePoint 服务的所有方面。

  > [!NOTE]
  > 此角色拥有 Azure Active Directory 之外的其他权限。 有关详细信息，请参阅上面的角色说明。
  >
  >

| **操作** | **说明** |
| --- | --- |
| microsoft.aad.directory/groups/unified/appRoleAssignments/update | 更新 Azure Active Directory 中的 groups.unified 属性。 |
| microsoft.aad.directory/groups/unified/basic/update | 更新 Office 365 组的基本属性。 |
| microsoft.aad.directory/groups/unified/create | 创建 Office 365 组。 |
| microsoft.aad.directory/groups/unified/delete | 删除 Office 365 组。 |
| microsoft.aad.directory/groups/unified/members/update | 更新 Office 365 组的成员身份。 |
| microsoft.aad.directory/groups/unified/owners/update | 更新 Office 365 组的所有权。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 读取和配置 Azure 服务运行状况。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 创建和管理 Azure 支持票证。 |
| microsoft.office365.webPortal/allEntities/basic/read | 读取 microsoft.office365.webPortal 中所有资源的基本属性。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 读取和配置 Office 365 服务运行状况。 |
| microsoft.office365.sharepoint/allEntities/allTasks | 创建和删除所有资源，然后读取和更新 microsoft.office365.sharepoint 中的标准属性。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 创建和管理 Office 365 支持票证。 |

### <a name="teams-communications-administrator"></a>Teams 通信管理员
可以管理 Microsoft Teams 服务中的通话和会议功能。

  > [!NOTE]
  > 此角色拥有 Azure Active Directory 之外的其他权限。 有关详细信息，请参阅上面的角色说明。
  >
  >

| **操作** | **说明** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | 读取和配置 Azure 服务运行状况。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 创建和管理 Azure 支持票证。 |
| microsoft.office365.webPortal/allEntities/basic/read | 读取 microsoft.office365.webPortal 中所有资源的基本属性。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 读取和配置 Office 365 服务运行状况。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 创建和管理 Office 365 支持票证。 |
| microsoft.office365.usageReports/allEntities/read | 阅读 Office 365 使用情况报告。 |

### <a name="teams-communications-support-engineer"></a>Teams 通信支持工程师
可以使用高级工具排查 Teams 中的通信问题。

  > [!NOTE]
  > 此角色拥有 Azure Active Directory 之外的其他权限。 有关详细信息，请参阅上面的角色说明。
  >
  >

| **操作** | **说明** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | 读取和配置 Azure 服务运行状况。 |
| microsoft.office365.webPortal/allEntities/basic/read | 读取 microsoft.office365.webPortal 中所有资源的基本属性。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 读取和配置 Office 365 服务运行状况。 |

### <a name="teams-communications-support-specialist"></a>Teams 通信支持专家
可以使用基本工具排查 Teams 中的通信问题。

  > [!NOTE]
  > 此角色拥有 Azure Active Directory 之外的其他权限。 有关详细信息，请参阅上面的角色说明。
  >
  >

| **操作** | **说明** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | 读取和配置 Azure 服务运行状况。 |
| microsoft.office365.webPortal/allEntities/basic/read | 读取 microsoft.office365.webPortal 中所有资源的基本属性。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 读取和配置 Office 365 服务运行状况。 |

### <a name="teams-service-administrator"></a>Teams 服务管理员
可以管理 Microsoft Teams 服务。 

  > [!NOTE]
  > 此角色拥有 Azure Active Directory 之外的其他权限。 有关详细信息，请参阅上面的角色说明。
  >
  >

| **操作** | **说明** |
| --- | --- |
| microsoft.aad.directory/groups/hiddenMembers/read | 读取 Azure Active Directory 中的 groups.hiddenMembers 属性。 |
| microsoft.aad.directory/groups/unified/appRoleAssignments/update | 更新 Azure Active Directory 中的 groups.unified 属性。 |
| microsoft.aad.directory/groups/unified/basic/update | 更新 Office 365 组的基本属性。 |
| microsoft.aad.directory/groups/unified/create | 创建 Office 365 组。 |
| microsoft.aad.directory/groups/unified/delete | 删除 Office 365 组。 |
| microsoft.aad.directory/groups/unified/members/update | 更新 Office 365 组的成员身份。 |
| microsoft.aad.directory/groups/unified/owners/update | 更新 Office 365 组的所有权。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 读取和配置 Azure 服务运行状况。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 创建和管理 Azure 支持票证。 |
| microsoft.office365.webPortal/allEntities/basic/read | 读取 microsoft.office365.webPortal 中所有资源的基本属性。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 读取和配置 Office 365 服务运行状况。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 创建和管理 Office 365 支持票证。 |
| microsoft.office365.usageReports/allEntities/read | 阅读 Office 365 使用情况报告。 |

### <a name="user-administrator"></a>用户管理员
可以管理用户和组的所有方面，包括重置有限管理员的密码。

| **操作** | **说明** |
| --- | --- |
| microsoft.aad.directory/appRoleAssignments/create | 在 Azure Active Directory 中创建 appRoleAssignments。 |
| microsoft.aad.directory/appRoleAssignments/delete | 删除 Azure Active Directory 中的 appRoleAssignments。 |
| microsoft.aad.directory/appRoleAssignments/update | 更新 Azure Active Directory 中的 appRoleAssignments。 |
| microsoft.aad.directory/contacts/basic/update | 更新 Azure Active Directory 中联系人的基本属性。 |
| microsoft.aad.directory/contacts/create | 在 Azure Active Directory 中创建联系人。 |
| microsoft.aad.directory/contacts/delete | 删除 Azure Active Directory 中的联系人。 |
| microsoft.aad.directory/groups/appRoleAssignments/update | 更新 Azure Active Directory 中的 groups.appRoleAssignments 属性。 |
| microsoft.aad.directory/groups/basic/update | 更新 Azure Active Directory 中组的基本属性。 |
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
| microsoft.aad.directory/users/basic/update | 更新 Azure Active Directory 中用户的基本属性。 |
| microsoft.aad.directory/users/create | 在 Azure Active Directory 中创建用户。 |
| microsoft.aad.directory/users/delete | 删除 Azure Active Directory 中的用户。 |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | 使 Azure Active Directory 中的所有用户刷新令牌无效。 |
| microsoft.aad.directory/users/manager/update | 更新 Azure Active Directory 中的 users.manager 属性。 |
| microsoft.aad.directory/users/password/update | 更新 Azure Active Directory 中所有用户的密码。 有关详细信息，请参阅联机文档。 |
| microsoft.aad.directory/users/restore | 还原 Azure Active Directory 中已删除的用户。 |
| microsoft.aad.directory/users/userPrincipalName/update | 更新 Azure Active Directory 中的 users.userPrincipalName 属性。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 读取和配置 Azure 服务运行状况。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 创建和管理 Azure 支持票证。 |
| microsoft.office365.webPortal/allEntities/basic/read | 读取 microsoft.office365.webPortal 中所有资源的基本属性。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 读取和配置 Office 365 服务运行状况。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 创建和管理 Office 365 支持票证。 |

## <a name="deprecated-roles"></a>已弃用的角色

不应使用以下角色。 这些角色已弃用，并将从 Azure AD 中删除。

- 即席许可证管理员
- 设备联接
- 设备管理器
- 设备用户
- 经电子邮件验证的用户创建者
- 邮箱管理员
- 工作区设备联接

## <a name="next-steps"></a>后续步骤

- 若要详细了解如何将用户分配为 Azure 订阅的管理员，请参阅[使用 RBAC 和 Azure 门户管理访问权限](../../role-based-access-control/role-assignments-portal.md)
- 若要了解有关如何在 Azure 中控制资源访问的详细信息，请参阅[了解 Azure 中的资源访问权限](../../role-based-access-control/rbac-and-directory-admin-roles.md)

<!-- Update_Description: wording update -->

