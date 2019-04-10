---
title: 有关 Azure Active Directory B2C 的常见问题解答 | Microsoft Docs
description: 有关 Azure Active Directory B2C 的常见问题解答 (FAQ)。
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
origin.date: 11/30/2018
ms.date: 04/01/2019
ms.author: v-junlch
ms.subservice: B2C
ms.openlocfilehash: 7427c160c3dc73c49d22391aa31e970e1f3448fd
ms.sourcegitcommit: 3b05a8982213653ee498806dc9d0eb8be7e70562
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/04/2019
ms.locfileid: "59004390"
---
# <a name="azure-ad-b2c-frequently-asked-questions-faq"></a>Azure AD B2C：常见问题 (FAQ) 
此页面解答了有关 Azure Active Directory (Azure AD) B2C 的常见问题。 请随时返回查看更新信息。

### <a name="why-cant-i-access-the-azure-ad-b2c-extension-in-the-azure-portal"></a>为什么我在 Azure 门户中无法访问 Azure AD B2C 扩展？
如果 Azure AD 扩展无法正常工作，通常有两个原因。  Azure AD B2C 要求你在目录中具备全局管理员的用户角色。  如果你认为应具有访问权限，请与管理员联系。  如果你拥有全局管理员权限，请确保处于 Azure AD B2C 目录（而不是 Azure Active Directory 目录）中。  可以查看有关[创建 Azure AD B2C 租户](tutorial-create-tenant.md)的说明。

### <a name="can-i-use-azure-ad-b2c-features-in-my-existing-employee-based-azure-ad-tenant"></a>我可以在基于员工的现有 Azure AD 租户中使用 Azure AD B2C 功能吗？
Azure AD 和 Azure AD B2C 是独立的产品/服务，不能在同一租户中共存。  Azure AD 租户表示组织。  Azure AD B2C 租户表示信赖方应用使用的标识集合。  通过自定义策略（在公共预览版中），Azure AD B2C 可以联合 Azure AD，允许对组织中的员工进行身份验证。

### <a name="can-i-use-azure-ad-b2c-to-provide-social-login-into-office-365"></a>我可以使用 Azure AD B2C 提供 Office 365 的社交登录吗？
Azure AD B2C 不用于 Microsoft Office 365 用户的身份验证。  Azure AD B2C 提供用于生成 Web 和移动应用程序的标识和访问管理平台。  当 Azure AD B2C 配置为联合 Azure AD 租户时，Azure AD 租户将管理员工对依赖 Azure AD B2C 的应用程序的访问权限。

### <a name="what-are-local-accounts-in-azure-ad-b2c-how-are-they-different-from-work-or-school-accounts-in-azure-ad"></a>什么是 Azure AD B2C 中的本地帐户？ 它们与 Azure AD 中的工作或学校帐户有何不同？
在 Azure AD 租户中，属于租户的用户使用 `<xyz>@<tenant domain>` 形式的电子邮件地址登录。  `<tenant domain>` 是租户中已验证域之一或初始的 `<...>.partner.onmschina.cn` 域。 此类型的帐户是工作或学校帐户。

在 Azure AD B2C 租户中，大多数应用都希望用户使用任意电子邮件地址登录。 此类型的帐户是本地帐户。  我们还支持任意用户名作为本地帐户（例如，joe、bob、sarah 或 jim）。 在 Azure 门户中配置 Azure AD B2C 的标识提供者时，可以选择这两种本地帐户类型中的一种。 在 Azure AD B2C 租户中，单击“标识提供者”，然后选择“本地帐户”下的“用户名”。 

应用程序的用户帐户必须始终通过注册用户流、注册或登录用户流，或使用 Azure AD Graph API 创建。 在 Azure 门户中创建的用户帐户仅用于管理租户。

### <a name="which-social-identity-providers-do-you-support-now-which-ones-do-you-plan-to-support-in-the-future"></a>现在支持哪些社交标识提供者？ 计划在未来支持哪些？
我们当前支持微信（预览版）、微博（预览版）和 QQ（预览版）。 我们会根据客户需求添加对其他流行社交标志提供者的支持。

Azure AD B2C 还增加了对[自定义策略](active-directory-b2c-overview-custom.md)的支持。  这些[自定义策略](active-directory-b2c-overview-custom.md)允许开发人员使用支持 [OpenID 连接](https://openid.net/specs/openid-connect-core-1_0.html)或 SAML 的任何标识提供者创建自己的策略。 

查看我们的[自定义策略初学者包](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack)，开始使用自定义策略。

### <a name="can-i-configure-scopes-to-gather-more-information-about-consumers-from-various-social-identity-providers"></a>我可以配置范围，从各种社交标识提供者收集更多使用者的相关信息吗？
否。 一组受支持的社交标识提供者使用的默认范围是：

* LinkedIn：r_emailaddress、r_basicprofile

### <a name="does-my-application-have-to-be-run-on-azure-for-it-work-with-azure-ad-b2c"></a>必须在 Azure 上运行应用程序才能将其与 Azure AD B2C 一起使用吗？
不，可以在任何位置（在云中或本地）托管应用程序。 只要能在公共可访问的端点上发送和接收 HTTP 请求，它就可以与 Azure AD B2C 进行交互。

### <a name="i-have-multiple-azure-ad-b2c-tenants-how-can-i-manage-them-on-the-azure-portal"></a>我有多个 Azure AD B2C 租户。 如何在 Azure 门户上管理它们？
在 Azure 门户的左侧菜单中打开“Azure AD B2C”之前，必须切换到要管理的目录。  通过单击 Azure 门户右上方的标识切换目录，然后在出现的下拉列表中选择目录。

### <a name="what-password-user-flow-is-used-for-local-accounts-in-azure-ad-b2c"></a>Azure AD B2C 中的本地帐户使用什么密码用户流？
本地帐户的 Azure AD B2C 密码用户流以 Azure AD 的策略为基础。 Azure AD B2C 的注册、注册或登录和密码重置用户流使用“强”密码强度，并且不会让任何密码过期。 有关详细信息，请阅读 [Azure AD 密码策略](https://msdn.microsoft.com/library/azure/jj943764.aspx)。 有关帐户锁定和密码的信息，请参阅[管理对 Azure Active Directory B2C 中资源和数据的威胁](active-directory-b2c-reference-threat-management.md)。

### <a name="can-i-use-azure-ad-connect-to-migrate-consumer-identities-that-are-stored-on-my-on-premises-active-directory-to-azure-ad-b2c"></a>我可以使用 Azure AD Connect 将存储在本地 Active Directory 中的使用者标识迁移到 Azure AD B2C 吗？
不可以，Azure AD Connect 不是为与 Azure AD B2C 一起使用而设计的。 请考虑使用 [Azure AD 图形 API](active-directory-b2c-devquickstarts-graph-dotnet.md) 进行用户迁移。  有关详细信息，请参阅[用户迁移指南](active-directory-b2c-user-migration.md)。

### <a name="can-my-app-open-up-azure-ad-b2c-pages-within-an-iframe"></a>我的应用是否可在 iFrame 中打开 Azure AD B2C 页？
不可以，出于安全的考虑，无法在 iFrame 中打开 Azure AD B2C 页。  我们的服务将与浏览器通信以禁止 iFrame。  由于点击劫持的风险，安全社区和 OAUTH2 规范一般建议不要使用 iFrame 进行标识体验。

### <a name="does-azure-ad-b2c-work-with-crm-systems-such-as-microsoft-dynamics"></a>Azure AD B2C 是否可以与 Microsoft Dynamics 之类的 CRM 系统一起使用？
与 Microsoft Dynamics 365 门户的集成可用。  请参阅[配置 Dynamics 365 门户以使用 Azure AD B2C 进行身份验证](https://docs.microsoft.com/dynamics365/customer-engagement/portals/azure-ad-b2c)。

### <a name="can-i-localize-the-ui-of-pages-served-by-azure-ad-b2c-what-languages-are-supported"></a>我可以本地化 Azure AD B2C 所提供页面的 UI 吗？ 支持哪些语言？
能！  请阅读公共预览版中的[语言自定义](active-directory-b2c-reference-language-customization.md)。  我们提供 36 种语言的翻译版本，并且你可以根据需要替代任何字符串。

### <a name="can-i-use-my-own-urls-on-my-sign-up-and-sign-in-pages-that-are-served-by-azure-ad-b2c-for-instance-can-i-change-the-url-from-loginpartnermicrosoftonlinecn-to-logincontosocom"></a>我可以在 Azure AD B2C 提供的注册和登录页面上使用自己的 URL 吗？ 例如，可以将 URL 从 login.partner.microsoftonline.cn 更改为 login.contoso.com 吗？
目前不可以。 该功能在我们的计划之中。 在 Azure 门户上的“域”选项卡中验证域并不能实现此目标。

### <a name="how-do-i-delete-my-azure-ad-b2c-tenant"></a>如何删除 Azure AD B2C 租户？
请按照以下步骤删除 Azure AD B2C 租户：

1. 删除 Azure AD B2C 租户中的所有策略。
1. 现在，以订阅管理员身份登录到 [Azure 门户](https://portal.azure.cn/)。 （使用在注册 Azure 时使用的同一工作或学校帐户，或同一 Microsoft 帐户。）
1. 切换到要删除的 Azure AD B2C 租户。
2. 导航到左侧的 Active Directory 菜单。
3. 选择“用户和组”。
4. 依次选择每个用户（排除当前以订阅管理员身份登录的用户）。 单击页面底部的“删除”，并在出现提示时单击“是”。
5. 单击“应用注册”。
6. 选择名为“b2c-extensions-app”的应用程序。 单击“删除”，出现提示时，单击“是”。
7. 选择“概述”。
8. 单击“删除目录”。 要完成该过程，请按照屏幕上的说明进行操作。

### <a name="can-i-get-azure-ad-b2c-as-part-of-enterprise-mobility-suite"></a>我可以将 Azure AD B2C 作为企业移动性套件的一部分吗？
不，Azure AD B2C 是即用即付 Azure 服务，不是企业移动套件的一部分。

