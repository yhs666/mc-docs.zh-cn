---
title: "Azure AD Connect：无缝单一登录 — 工作原理 | Microsoft Docs"
description: "本文介绍了 Azure Active Directory 无缝单一登录功能的工作原理。"
services: active-directory
keywords: "什么是 Azure AD Connect, 安装 Active Directory, Azure AD 所需的组件, SSO, 单一登录"
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
ms.openlocfilehash: ac67596ac67093468cd7622e06130732a13be888
ms.sourcegitcommit: b1d2bd71aaff7020dfb3f7874799e03df3657cd4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
# Azure Active Directory 无缝单一登录：技术深入探讨
<a id="azure-active-directory-seamless-single-sign-on-technical-deep-dive" class="xliff"></a>

本文介绍了有关 Azure Active Directory 无缝单一登录（Azure AD 无缝 SSO）功能的工作原理的技术详细信息。

## Azure AD 无缝 SSO 的工作原理
<a id="how-does-azure-ad-seamless-sso-work" class="xliff"></a>

本节包含两部分：
1. 无缝 SSO 功能的设置。
2. 单个用户登录事务如何与无缝 SSO 一起使用。

### 设置如何工作？
<a id="how-does-set-up-work" class="xliff"></a>

无缝 SSO 是使用 Azure AD Connect 启用的，如[此处](active-directory-aadconnect-sso-quick-start.md)所示。 启用此功能时，会发生以下步骤：
- 在本地 Active Directory (AD) 中创建一个名为 `AZUREADSSOACCT`（这表示 Azure AD）的计算机帐户。
- 此计算机帐户的 Kerberos 加密密钥安全地与 Azure AD 进行共享。
- 此外，将创建两个 Kerberos 服务主体名称 (SPN) 来表示 Azure AD 登录期间使用的两个 URL。

>[!NOTE]
> 计算机帐户和 Kerberos SPN 是在与 Azure AD 同步（通过 Azure AD Connect）且要为其用户启用无缝 SSO 的每个 AD 林中创建的。 请将 `AZUREADSSOACCT` 计算机帐户移动到存储着其他计算机帐户的组织单位，以确保以相同的方式对其进行管理且不会将其删除。

### 采用无缝 SSO 的登录如何工作？
<a id="how-does-sign-in-with-seamless-sso-work" class="xliff"></a>

完成此设置后，无缝 SSO 的工作方式与其他任何使用 Windows 集成身份验证 (IWA) 的登录方式相同。 工作流如下所述：

1. 用户尝试从你的企业网络内某个已加入域的公司设备访问某个应用程序（例如 Outlook Web App - https://outlook.office365.com/owa/）。
2. 如果该用户尚未登录，则会将其重定向到 Azure AD 登录页。

    >[!NOTE]
    >如果 Azure AD 登录请求包含 `domain_hint`（标识你的租户，例如 contoso.partner.onmschina.cn）或 `login_hint`（标识用户，例如 user@contoso.partner.onmschina.cn 或 user@contoso.com）参数，则会跳过步骤 2。

3. 用户在 Azure AD 登录页中键入其用户名。
4. Azure AD 在后台使用 JavaScript，通过“401 未授权”响应质询浏览器，要求其提供 Kerberos 票证。
5. 然后，浏览器从 Active Directory 请求 `AZUREADSSOACCT` 计算机帐户（这表示 Azure AD）的票证。
6. Active Directory 查找计算机帐户，然后将使用计算机帐户机密加密的 Kerberos 票证返回给浏览器。
7. 浏览器将它从 Active Directory 获得的 Kerberos 票证转发到 Azure AD（在之前添加到浏览器的 [Intranet 区域设置的某个 Azure AD URL](active-directory-aadconnect-sso-quick-start.md#step-3-roll-out-the-feature) 上）。
8. Azure AD 使用之前共享的密钥解密 Kerberos 票证，这包括登录到公司设备的用户的标识。
9. 在评估后，Azure AD 将会向用户返回令牌或者要求用户执行其他验证，例如多重身份验证。
10. 如果用户登录成功，则可以访问应用程序。

下图演示了涉及的所有组件和步骤。

![无缝单一登录](./media/active-directory-aadconnect-sso/sso2.png)

无缝 SSO 是机会型的，这意味着，如果它失败，则登录体验将回退到其常规行为 - 即，用户需要输入其密码进行登录。

## 后续步骤
<a id="next-steps" class="xliff"></a>

- [**快速入门**](active-directory-aadconnect-sso-quick-start.md) - 启动并运行 Azure AD 无缝 SSO。
- [**常见问题**](active-directory-aadconnect-sso-faq.md) - 常见问题的解答。
- [**故障排除**](active-directory-aadconnect-troubleshoot-sso.md) - 了解如何解决此功能的常见问题。
- [**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - 用于提出新的功能请求。

