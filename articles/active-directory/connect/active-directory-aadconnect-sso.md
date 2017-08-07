---
title: "Azure AD Connect：无缝单一登录 | Microsoft Docs"
description: "本主题介绍 Azure Active Directory (Azure AD) 无缝单一登录，以及如何使用它来为企业网络中的企业桌面用户提供真正的单一登录。"
services: active-directory
keywords: "什么是 Azure AD Connect, 安装 Active Directory, Azure AD 所需的组件, SSO, 单一登录"
documentationcenter: 
author: alexchen2016
manager: digimobile
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 07/12/2017
ms.date: 07/31/2017
ms.author: v-junlch
ms.openlocfilehash: c3b2d32bf994526b6ff84356f0f928c9f2e2d2ef
ms.sourcegitcommit: cd0f14ddb0bf91c312d5ced9f38217cfaf0667f5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/04/2017
---
# <a name="azure-active-directory-seamless-single-sign-on"></a>Azure Active Directory 无缝单一登录

## <a name="what-is-azure-active-directory-seamless-single-sign-on"></a>什么是 Azure Active Directory 无缝单一登录？

Azure Active Directory 无缝单一登录（Azure AD 无缝 SSO）可在连接到企业网络的企业设备上使用户自动登录。 启用此功能后，用户无需键入其密码即可登录到 Azure AD；通常，甚至无需键入其用户名。 此功能可让用户轻松访问基于云的应用程序，而无需使用其他任何本地组件。

无缝 SSO 可与[密码哈希同步](active-directory-aadconnectsync-implement-password-synchronization.md)或[直通身份验证](active-directory-aadconnect-pass-through-authentication.md)登录方法结合使用。

![无缝单一登录](./media/active-directory-aadconnect-sso/sso1.png)

>[!NOTE]
>此功能不适用于 Active Directory 联合身份验证服务 (ADFS)，因为其中包含此功能。

## <a name="key-benefits-of-using-azure-ad-seamless-sso"></a>使用 Azure AD 无缝 SSO 的主要优点

- 更好的用户体验
  - 用户自动登录本地应用程序和基于云的应用程序。
  - 用户无需重复输入密码。
- 易于部署和管理
  - 本地不需要任何其他组件即可使其运作。
  - 可与托管身份验证的任何方法一起工作 - [密码哈希同步](active-directory-aadconnectsync-implement-password-synchronization.md)或[直通身份验证](active-directory-aadconnect-pass-through-authentication.md)。
  - 可使用组策略向某些或所有用户推出。
  - 向 Azure AD 注册非 Windows 10 设备。 这需要 2.1 版或更高版本的[加入工作区客户端](https://www.microsoft.com/download/details.aspx?id=53554)。

## <a name="feature-highlights"></a>功能特点

- 登录用户名可以是本地默认用户名 (`userPrincipalName`)，也可以是 Azure AD Connect 中配置的另一个属性 (`Alternate ID`)。
- 无缝 SSO 是机会性功能。 如果它由于任何原因失败，用户登录体验将回退到其常规行为 - 即，用户需要在登录页面上输入其密码。
- 如果应用程序在其 Azure AD 登录请求中转发 `domain_hint`（标识租户）或`login_hint`（标识用户）参数，用户会自动登录，无需输入用户名或密码。
- 可通过Azure AD Connect 启用此功能。
- 这是一项免费功能，不需要拥有任何付费版本的 Azure AD 即可使用此功能。
- 基于 Web 浏览器的客户端和 Office 客户端（在能够进行 Kerberos 身份验证的平台和浏览器中，支持[新式身份验证](https://aka.ms/modernauthga)）上支持此功能：

| 操作系统\浏览器 |Internet Explorer|Edge|Google Chrome|Mozilla Firefox|Safari|
| --- | --- |--- | --- | --- | -- 
|Windows 10|是|不支持|是|是\*|不适用
|Windows 8.1|是|不支持|是|是\*|不适用
|Windows 8|是|不支持|是|是\*|不适用
|Windows 7|是|不支持|是|是\*|不适用
|Mac OS X|不适用|不适用|是\*|是\*|不支持

\*需要[额外的配置](active-directory-aadconnect-sso-quick-start.md#browser-considerations)

## <a name="next-steps"></a>后续步骤

- [**快速入门**](active-directory-aadconnect-sso-quick-start.md) - 启动并运行 Azure AD 无缝 SSO。
- [技术深入探讨](active-directory-aadconnect-sso-how-it-works.md) - 了解此功能的工作原理。
- [常见问题解答](active-directory-aadconnect-sso-faq.md) - 常见问题的解答。
- [故障排除](active-directory-aadconnect-troubleshoot-sso.md) - 了解如何解决此功能的常见问题。
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - 用于提出新的功能请求。

<!-- Update_Description: update meta properties -->