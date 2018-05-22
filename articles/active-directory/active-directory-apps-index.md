---
title: 有关 Azure Active Directory 中应用程序管理的文章索引 | Azure
description: 了解如何自定义联合证书的到期日期，以及如何续订即将过期的证书。
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: 5321b8e4-2afa-4dfe-8d53-4add7abb5ec8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 01/26/2018
ms.date: 05/07/2018
ms.author: v-junlch
ms.reviewer: asteen
ms.openlocfilehash: c82f1980455753d009d73972ebb5d53cb84da890
ms.sourcegitcommit: 6f08b9a457d8e23cf3141b7b80423df6347b6a88
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2018
---
# <a name="article-index-for-application-management-in-azure-active-directory"></a>有关 Azure Active Directory 中应用程序管理的文章索引
本页提供了一份完整列表，列出编写的 Azure Active Directory (Azure AD) 中各种应用程序相关功能的所有文章。

每个主要功能区都有简介，同时也根据要查找的信息指导需要阅读的文章。

## <a name="overview-articles"></a>概述文章
对于只需要 Azure AD 应用程序管理功能的简要说明的用户，以下文章是很好的起点。

| 文章指南 |  |
|:---:| --- |
| 介绍 Azure AD 解决的应用程序管理问题	 |[使用 Azure Active Directory (AD) 管理应用程序](active-directory-enable-sso-scenario.md) |
| Azure AD 中与启用单一登录、定义有权访问应用的人员，以及用户如何启动应用相关的各种功能概述 |[Azure Active Directory 中的应用程序访问和单一登录](active-directory-appssoaccess-whatis.md) |
| 探讨将应用程序集成到 Azure AD 时所涉及的不同步骤 |[将 Azure Active Directory 与应用程序集成](./active-directory-integrating-applications-getting-started.md)<br /><br />启用对 SaaS 应用的单一登录<br /><br />[管理对应用的访问](./active-directory-managing-access-to-apps.md) |
| 如何在 Azure AD 中表示应用的技术说明 |[如何以及为何将应用程序添加到 Azure AD](./develop/active-directory-how-applications-are-added.md) |

## <a name="troubleshooting-articles"></a>疑难解答文章
本部分提供相关故障排除指南的快速访问链接。 可以在本页的余下部分找到有关每个功能区的详细信息。

| 功能区 |  |
|:---:| --- |
| 联合单一登录 |[排查基于 SAML 的单一登录问题](./develop/active-directory-saml-debugging.md) |
| 基于密码的单一登录 | Internet Explorer 访问面板扩展疑难解答 |
| 本地 AD 与 Azure AD 之间的单一登录 |[排查密码哈希同步问题](connect/active-directory-aadconnectsync-implement-password-hash-synchronization.md#troubleshoot-password-hash-synchronization)|

## <a name="single-sign-on-sso"></a>单一登录 (SSO)
### <a name="federated-single-sign-on-sign-into-many-apps-using-one-identity"></a>联合单一登录：使用一个标识登录多个应用程序
单一登录可让用户只使用一组凭据，访问各种不同的应用程序和服务。 可以通过联合方法启用单一登录。 当用户尝试登录联合应用时，会被重定向到其组织的官方登录页面（由 Azure Active Directory 呈现），一旦身份验证成功，即重定向回到应用。

| 文章指南 |  |
|:---:| --- |
| 联合身份验证和其他登录类型简介 |[使用 Azure AD 进行单一登录](active-directory-appssoaccess-whatis.md) |
| 150 个以上的应用教程，介绍如何为应用配置单一登录 |
| 如何手动设置和自定义单一登录配置 |如何为不在 Azure Active Directory 应用程序库中的应用配置联合单一登录 |
| 使用 SAML 协议的联合应用的故障排除指南 |[排查基于 SAML 的单一登录问题](./develop/active-directory-saml-debugging.md) |
| 如何设置应用的证书过期日期，以及如何续订证书 | 在 Azure Active Directory 中管理用于联合单一登录的证书 |

联合单一登录适用于所有版本的 Azure AD，每个用户最多十个应用。 [Azure AD Premium](https://www.azure.cn/pricing/details/identity/) 支持无限数目的应用程序。 如果组织拥有 [Azure AD 基本版](https://www.azure.cn/pricing/details/identity/)或 [Azure AD 高级版](https://www.azure.cn/pricing/details/identity/)，则可以[使用组来分配对联合应用程序的访问权限](#managing-access-to-applications)。

### <a name="password-based-single-sign-on-account-sharing-and-sso-for-non-federated-apps"></a>基于密码的单一登录：非联合应用的帐户共享和 SSO
为了实现单一登录到不支持联合身份验证的应用程序，Azure AD 提供了密码管理功能，这些功能可安全地将密码存储到 SaaS 应用并自动将用户登录到这些应用。 可以轻松分发新建帐户的凭据，并与多人共享团队帐户。 用户无需知道他们有权访问的帐户凭据。

| 文章指南 |  |
|:---:| --- |
| 基于密码的 SSO 工作原理简介以及简要的技术概述 |[使用 Azure AD 进行基于密码的单一登录](active-directory-appssoaccess-whatis.md#password-based-single-sign-on) |
| 与共享帐户相关的方案以及 Azure AD 如何解决这些问题的摘要 |[使用 Azure AD 共享帐户](active-directory-sharing-accounts.md) |
| 自动定期更改特定应用的密码 |[自动密码滚动更新（预览版）](https://blogs.technet.microsoft.com/enterprisemobility/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview/) |
| Internet Explorer 版本的 Azure AD 密码管理扩展功能的部署和故障排除指南 |如何使用组策略部署 Internet Explorer 的访问面板扩展 <br /><br /> Internet Explorer 访问面板扩展疑难解答  |

基于密码的单一登录适用于所有版本的 Azure AD，每个用户最多十个应用。 [Azure AD Premium](https://www.azure.cn/pricing/details/identity/) 支持无限数目的应用程序。 如果组织拥有 [Azure AD Basic](https://www.azure.cn/pricing/details/identity/) 或 [Azure AD Premium](https://www.azure.cn/pricing/details/identity/)，则可以[使用组来分配对应用程序的访问权限](#managing-access-to-applications)。 自动密码滚动更新是一项 [Azure AD Premium](https://www.azure.cn/pricing/details/identity/) 功能。

### <a name="enabling-single-sign-on-between-azure-ad-and-on-premises-ad"></a>在 Azure AD 与本地 AD 之间启用单一登录
如果组织在本地维护 Windows Server Active Directory，并在云中维护 Azure Active Directory，则你可能想在这两个系统之间启用单一登录。 Azure AD Connect（将这两个系统集成在一起的工具）提供多个设置单一登录的选项：建立与 ADFS 的联合或其他联合提供程序，或启用密码同步。

| 文章指南 |  |
|:---:| --- |
| Azure AD Connect 中提供的单一登录选项概述，以及管理混合环境的相关信息 |[Azure AD Connect 中的用户登录选项](./connect/active-directory-aadconnect-user-signin.md) |
| 同时使用本地 Active Directory 和 Azure Active Directory 管理环境的一般指导 | [将本地标识与 Azure Active Directory 集成](./connect/active-directory-aadconnect.md) |
| 有关使用密码同步启用 SSO 的指导 |[实现与 Azure AD Connect 的密码同步](./connect/active-directory-aadconnectsync-implement-password-hash-synchronization.md)<br /><br />[密码同步疑难解答](https://support.microsoft.com/zh-cn/kb/2855271) |
| 有关使用第三方标识提供程序启用 SSO 的指南 |[可用于启用单一登录的兼容第三方标识提供程序列表](https://aka.ms/ssoproviders) |

Azure AD Connect 适用于[所有版本的 Azure Active Directory](https://www.azure.cn/pricing/details/identity/)。 Azure AD 自助密码重置适用于 [Azure AD Basic](https://www.azure.cn/pricing/details/identity/) 和 [Azure AD Premium](https://www.azure.cn/pricing/details/identity/)。 对本地 AD 进行密码写回是一项 [Azure AD Premium](https://www.azure.cn/pricing/details/identity/) 功能。

## <a name="apps--azure-ad"></a>应用和 Azure AD

### <a name="automatically-provision-and-deprovision-user-accounts-in-saas-apps"></a>自动设置和取消设置 SaaS 应用中的用户帐户
自动执行创建、维护和删除 SaaS 应用程序，如 Dropbox、Salesforce、ServiceNow 等中的用户标识。 在 Azure AD 与 SaaS 应用之间匹配和同步现有标识，并在用户离开组织时自动禁用帐户以控制访问。

| 文章指南 |  |
|:---:| --- |
| 配置如何在 Azure AD 与 SaaS 应用程序之间映射信息 | 自定义属性映射 <br><br> 编写属性映射表达式 |
| 如何对支持 SCIM 协议的任何应用启用自动设置 | 对启用 SCIM 的任何应用设置自动用户预配 |
| 根据应用程序的属性值限制可以设置应用程序的人员 | |

自动用户设置适用于所有版本的 Azure AD，每个用户最多十个应用。 [Azure AD Premium](https://www.azure.cn/pricing/details/identity/) 支持无限数目的应用程序。 如果组织拥有 [Azure AD Basic](https://www.azure.cn/pricing/details/identity/) 或 [Azure AD Premium](https://www.azure.cn/pricing/details/identity/)，则可以[使用组来管理要预配哪些用户](#managing-access-to-applications)。

### <a name="building-applications-that-integrate-with-azure-ad"></a>构建与 Azure AD 集成的应用程序
如果你的组织正在开发或维护业务线 (LoB) 应用程序，或者如果你是 Azure Active Directory 客户的应用开发人员，以下教程可帮助你将应用程序与 Azure AD 集成。

| 文章指南 |  |
|:---:| --- |
| 有关 IT 专业人员和应用程序开发人员集成应用程序与 Azure AD 的指南 |[针对 Azure AD 开发应用程序的 IT 专业人员指南](./active-directory-applications-guiding-developers-for-lob-applications.md)<br /><br />[Azure Active Directory 的开发人员指南](./develop/active-directory-developers-guide.md) |
| 如何使用 Azure Active Directory 管理对开发的应用程序的访问 |[如何为已开发的应用程序启用用户分配](./active-directory-applications-guiding-developers-requiring-user-assignment.md)<br /><br />[将用户分配到应用](./active-directory-applications-guiding-developers-assigning-users.md)<br /> |

## <a name="managing-access-to-applications"></a>管理对应用程序的访问


## <a name="see-also"></a>另请参阅
[什么是 Azure Active Directory？](active-directory-whatis.md)

[Azure 多重身份验证](/multi-factor-authentication/)

<!--Update_Description: wording update -->  
