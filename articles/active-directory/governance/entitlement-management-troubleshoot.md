---
title: 排查 Azure AD 权利管理（预览版）的问题 - Azure Active Directory
description: 了解应检查哪些项目来帮助排查 Azure Active Directory 权利管理（预览版）的问题。
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
origin.date: 05/30/2019
ms.date: 08/22/2019
ms.author: v-junlch
ms.reviewer: markwahl-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: 411edf194e54558089aedb2f0ebc9d642dfe3ac5
ms.sourcegitcommit: 599d651afb83026938d1cfe828e9679a9a0fb69f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/23/2019
ms.locfileid: "69993352"
---
# <a name="troubleshoot-azure-ad-entitlement-management-preview"></a>排查 Azure AD 权利管理（预览版）的问题

> [!IMPORTANT]
> Azure Active Directory (Azure AD) 权利管理目前以公共预览版提供。
> 此预览版在提供时没有附带服务级别协议，不建议将其用于生产工作负荷。 某些功能可能不受支持或者受限。
> 有关详细信息，请参阅[适用于 Azure 预览版的补充使用条款](https://www.azure.cn/support/legal/)。

本文介绍应检查哪些项目来帮助排查 Azure Active Directory (Azure AD) 权利管理的问题。

## <a name="checklist-for-entitlement-management-administration"></a>权利管理的管理功能查检表

* 如果你在配置权利管理时收到拒绝访问消息，而你是全局管理员，请确保你的目录具有 [Azure AD Premium P2（或 EMS E5）许可证](entitlement-management-overview.md#license-requirements)。  
* 如果你在创建或查看访问包时收到拒绝访问消息，而你是目录创建者组的成员，则必须在创建第一个访问包之前创建该目录。

## <a name="checklist-for-adding-a-resource"></a>有关添加资源的查检表

* 要使某个应用程序成为访问包中的资源，该应用程序必须至少有一个可分配的资源角色。 角色由应用程序本身定义，在 Azure AD 中进行管理。 请注意，Azure 门户可能还会显示不能选作应用程序的服务的服务主体。  具体而言，**Exchange Online** 和 **SharePoint Online** 是服务，而不是在目录中具有资源角色的应用程序，因此，不能将它们包含在访问包中。  应该改用基于组的许可为需要访问这些服务的用户建立相应的许可证。

* 要使某个组成为访问包中的资源，该组必须可在 Azure AD 中修改。  无法将源自于本地 Active Directory 的组分配为资源，因为无法在 Azure AD 中更改其所有者或成员属性。   在 Azure AD 中也无法将源自于 Exchange Online 的组修改为通讯组。 

* 无法将 SharePoint Online 文档库和单个文档添加为资源。  应该创建一个 Azure AD 安全组，在访问包中包含该组和站点角色，然后在 SharePoint Online 中使用该组来控制对文档库或文档的访问。

* 如果已将某些用户分配到你要使用访问包管理的资源，请确保使用相应的策略将这些用户分配到访问包。 例如，你可能想要在访问包中包含某个组，而该组中已有用户。 如果该组中的用户需要持续的访问权限，则他们必须对访问包具有相应的策略，以便不会失去对该组的访问权限。 可以通过要求用户请求包含该资源的访问包，或者通过将用户直接分配到访问包，来分配访问包。 有关详细信息，请参阅[编辑和管理现有访问包](entitlement-management-access-package-edit.md)。

## <a name="checklist-for-providing-external-users-access"></a>有关为外部用户提供访问权限的查检表

* 确保没有任何[条件访问策略](../conditional-access/require-managed-devices.md)阻止外部用户请求访问权限，或者阻止他们使用访问包中的应用程序。

## <a name="checklist-for-request-issues"></a>有关请求问题的查检表

* 当用户想要请求访问包的访问权限时，请确保他们使用访问包的 **“我的访问权限”门户链接**。 有关详细信息，请参阅[复制“我的访问权限”门户链接](entitlement-management-access-package-edit.md#copy-my-access-portal-link)。

* 当尚未位于你的目录中的用户登录到“我的访问权限”门户以请求访问包时，请确保他们使用其组织帐户进行身份验证。 组织帐户可以是资源目录中的帐户，也可以是包含在访问包的某个策略中的目录。 如果用户的帐户不是组织帐户，或者他们进行身份验证的目录未包含在策略中，则该用户将看不到访问包。 有关详细信息，请参阅[请求访问访问包](entitlement-management-request-access.md)。

* 如果阻止某个用户登录到资源目录，该用户将无法在“我的访问权限”门户中请求访问权限。 必须先从用户的配置文件中解除阻止登录，然后，该用户才能请求访问权限。 若要解除阻止登录，请在 Azure 门户中依次单击“Azure Active Directory”、“用户”、该用户、“配置文件”。    编辑“设置”部分，将“阻止登录”更改为“否”。    有关详细信息，请参阅[使用 Azure Active Directory 添加或更新用户的配置文件信息](../fundamentals/active-directory-users-profile-azure-portal.md)。  还可以检查用户是否由[标识保护策略](../identity-protection/howto-unblock-user.md)阻止。

* 在“我的访问权限”门户中，如果某个用户既是请求者又是审批者，该用户不会在“审批”页上看到自己对访问包的请求。  此行为是设计使然 - 用户不能审批自己的请求。 请确保在策略中为用户请求的访问包配置其他审批者。 有关详细信息，请参阅[编辑现有策略](entitlement-management-access-package-edit.md#edit-an-existing-policy)。

* 如果以前未曾在你的目录中登录过的新外部用户收到了包含 SharePoint Online 站点的访问包，在 SharePoint Online 中预配其帐户之前，其访问包将显示为“未完全传递”。

## <a name="next-steps"></a>后续步骤

- [查看有关用户如何在权利管理中获取访问权限的报告](entitlement-management-reports.md)

<!-- Update_Description: wording update -->