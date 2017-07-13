---
title: "Azure AD Connect：直通身份验证 - 常见问题解答 | Microsoft Docs"
description: "对 Azure Active Directory 直通身份验证常见问题的解答。"
services: active-directory
keywords: "Azure AD Connect 直通身份验证, 安装 Active Directory, Azure AD 所需的组件, SSO, 单一登录"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 06/15/2017
ms.date: 06/21/2017
ms.author: v-junlch
ms.openlocfilehash: 5b9e8c3659b95697c391c57592084db9c8c1b380
ms.sourcegitcommit: b1d2bd71aaff7020dfb3f7874799e03df3657cd4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
# Azure Active Directory 直通身份验证：常见问题解答
<a id="azure-active-directory-pass-through-authentication-frequently-asked-questions" class="xliff"></a>

在本文中，我们对有关 Azure Active Directory (Azure AD) 直通身份验证的常见问题进行了解答。 请随时返回查看新内容。

## 对于 Azure AD 登录方法 - 直通身份验证、密码哈希同步和 Active Directory 联合身份验证服务 (AD FS)，应选择哪一个？
<a id="which-of-the-azure-ad-sign-in-methods---pass-through-authentication-password-hash-synchronization-and-active-directory-federation-services-ad-fs---should-i-choose" class="xliff"></a>

这取决于本地环境和组织要求。 请阅读本文，了解[各种 Azure AD 登录方法的比较](active-directory-aadconnect-user-signin.md)。

## 直通身份验证是否为免费功能?
<a id="is-pass-through-authentication-a-free-feature" class="xliff"></a>

直通身份验证是一项免费功能，不需要拥有任何付费版本的 Azure AD 即可使用此功能。 功能正式发布后，仍可免费使用。

## 直通身份验证是否支持以“备用 ID”作为用户名，而不支持以“userPrincipalName”作为用户名？
<a id="does-pass-through-authentication-support-alternate-id-as-the-username-instead-of-userprincipalname" class="xliff"></a>

是的。 在 Azure AD Connect 中配置时，直通身份验证支持以“`Alternate ID`”作为用户名，如[此处](active-directory-aadconnect-get-started-custom.md)所示。 并非所有 Office 365 应用程序都支持“`Alternate ID`”。 请参阅支持声明的特定应用程序文档。

## 密码哈希同步是否充当到直通身份验证的回退？
<a id="does-password-hash-synchronization-act-as-a-fallback-to-pass-through-authentication" class="xliff"></a>

不可以，密码哈希同步不是到直通身份验证的泛型回退。 它仅充当[直通身份验证目前不支持的方案](active-directory-aadconnect-pass-through-authentication-current-limitations.md#unsupported-scenarios)的回退。 若要避免用户登录失败，应配置[高可用性](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-4-ensure-high-availability)的直通身份验证。

## 需要哪些版本的 Azure AD Connect 和直通身份验证代理？
<a id="what-versions-of-azure-ad-connect-and-pass-through-authentication-agent-do-you-need" class="xliff"></a>

需要 1.1.486.0 版本或更高版本的 Azure AD Connect，及 1.5.58.0 版本或更高版本的直通身份验证代理，此功能才能正常运行。 所有软件应安装在 Windows Server 2012 R2 或更高版本的服务器上。

## 如果用户密码已过期，并且用户尝试使用直通身份验证进行登录，将会发生什么情况？
<a id="what-happens-if-my-users-password-has-expired-and-they-try-to-sign-in-using-pass-through-authentication" class="xliff"></a>

如果针对特定用户配置[密码写回](../active-directory-passwords-update-your-own-password.md)，则用户使用直通身份验证登录时，可以更改或重置其密码。 密码会按预期写回到本地 Active Directory。

但是，如果未配置密码写回，或者没有为用户分配有效的 Azure AD 许可证，则用户不可以在云中更新其密码。 即使用户的密码已过期，也不能更新。 用户会看到消息：“你的组织不允许你更新此站点上的密码。 请根据组织建议的方法更新密码，或者请求管理员提供帮助。” 用户或管理员必须在本地 Active Directory 中重置其密码。

## 直通身份验证代理通过端口 80 和 443 进行哪些通信？
<a id="what-do-pass-through-authentication-agents-communicate-over-ports-80-and-443" class="xliff"></a>

- 身份验证代理会通过端口 443 发出所有功能操作的 HTTPS 请求，例如启用功能，处理所用用户登录请求及其他。
- 身份验证代理通过端口 80 发出 HTTP 请求，下载 SSL 证书吊销列表。

     >[!NOTE]
     >在最新的更新过程中，我们将减少身份验证代理与 Azure AD 通信所需的端口数。 如果运行旧版 Azure AD Connect 和/或独立身份验证代理，应继续让这些附加端口（5671、8080、9090、9091、9350、9352、10100-10120）保持打开状态。

## 直通身份验证代理是否可以通过出站 Web 代理服务器通信？
<a id="can-the-pass-through-authentication-agents-communicate-over-an-outbound-web-proxy-server" class="xliff"></a>

是的。 如果在本地环境中启用了 WPAD（Web 代理自动发现），身份验证代理将自动尝试在网络上查找并使用 Web 代理服务器。

## 是否可以在同一台服务器上安装两个或多个直通身份验证代理？
<a id="can-i-install-two-or-more-pass-through-authentication-agents-on-the-same-server" class="xliff"></a>

不可以，一台服务器上只能安装一个直通身份验证代理。 若要配置高可用性的直通身份验证，请改为按照[本文](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-4-ensure-high-availability)的说明进行操作。

## 我已使用适用于 Azure AD 登录的 Active Directory 联合身份验证服务 (AD FS)。 如何切换到直通身份验证？
<a id="i-already-use-active-directory-federation-services-ad-fs-for-azure-ad-sign-in-how-do-i-switch-it-to-pass-through-authentication" class="xliff"></a>

如果已使用 Azure AD Connect 向导将 AD FS 配置为登录方法，则可将用户登录方法更改为直通身份验证。 此更改将对租户启用直通身份验证，并将所有联合域转换为托管域。 租户中的所有后续登录请求均由直通身份验证处理。 Azure AD Connect 中现有的方法不支持跨不同的域使用 AD FS 和直通身份验证的组合。

如果 AD FS 已配置为 Azure AD Connect 向导外部的登录方法，则可通过“不可配置”选项将用户登录方法更改为直通身份验证。 此更改可对租户启用直通身份验证。 但是，所有的联合域都会继续使用 AD FS 进行登录。 通过 PowerShell 可手动将这些联合域中的一部分或全部转换为托管域。 在此之后，对托管域的所有登录请求只能由直通身份验证处理。

>[!IMPORTANT]
>直通身份验证不会处理用于仅限云的 Azure AD 用户的登录。

## 是否可以在多林 AD 环境中使用直通身份验证？
<a id="can-i-use-pass-through-authentication-in-a-multi-forest-ad-environment" class="xliff"></a>

是的。 如果 AD 林之间存在信任关系并且正确配置了名称后缀路由，则支持多林环境。

## 直通身份验证代理是否提供负载均衡功能？
<a id="do-pass-through-authentication-agents-provide-load-balancing-capability" class="xliff"></a>

不提供，安装多个直通身份验证代理可以确保[高可用性](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-4-ensure-high-availability)，但不会提供负载均衡。 一个或两个身份验证代理可能已足够处理大量的登录请求。

身份验证代理需要处理的密码验证请求量比较小。 因此大多数客户的峰值负载和平均负载可以由两个或三个身份验证代理（一共）轻松处理。

建议在域控制器附近安装身份验证代理，改善登录延迟。

## 是否可以在不运行 Azure AD Connect 的服务器上安装第一个直通身份验证代理？
<a id="can-i-install-the-first-pass-through-authentication-agent-on-a-server-other-than-the-one-that-runs-azure-ad-connect" class="xliff"></a>

不可以，不支持这种情况。

## 应安装多少个直通身份验证代理？
<a id="how-many-pass-through-authentication-agents-should-i-install" class="xliff"></a>

建议：

- 总共安装两个或三个身份验证代理。 这样可以满足大多数客户的需求。
- 在域控制器上，或尽可能接近域控制器的位置安装身份验证代理，从而改善登录延迟。
- 确保将安装身份验证代理的服务器添加到密码需要验证的用户所在的 AD 林。

请注意，系统限制每个租户最多有 12 个身份验证代理。

## 如何禁用直通身份验证？
<a id="how-can-i-disable-pass-through-authentication" class="xliff"></a>

重新运行 Azure AD Connect 向导，并将用户登录方法从直通身份验证更改为另一种方法。 此更改将对租户禁用直通身份验证，并从服务器上卸载身份验证代理。 必须从其他服务器上手动卸载身份验证代理。

## 卸载直通身份验证代理时，会发生什么情况？
<a id="what-happens-when-i-uninstall-a-pass-through-authentication-agent" class="xliff"></a>

从服务器上卸载直通身份验证代理会导致它停止接受登录请求。 执行此操作前，请确保另一身份验证代理处于运行状态，避免中断用户登录租户。

## 后续步骤
<a id="next-steps" class="xliff"></a>
- [当前限制](active-directory-aadconnect-pass-through-authentication-current-limitations.md) - 此功能目前处于预览状态。 了解支持的方案和不支持的方案。
- [快速入门](active-directory-aadconnect-pass-through-authentication-quick-start.md) - 启动并运行 Azure AD 直通身份验证。
- [技术深入探讨](active-directory-aadconnect-pass-through-authentication-how-it-works.md) - 了解此功能的工作原理。
- [故障排除](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) - 了解如何解决此功能的常见问题。
- [Azure AD 无缝 SSO](active-directory-aadconnect-sso.md) - 了解有关此补充功能的详细信息。
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - 用于提出新的功能请求。

