---
title: "Azure AD Connect：直通身份验证 - 当前限制 | Microsoft Docs"
description: "本文介绍如何对 Azure Active Directory (Azure AD) 直通身份验证当前的限制。"
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
origin.date: 06/05/2017
ms.date: 06/21/2017
ms.author: v-junlch
ms.openlocfilehash: 39db142a2f0d8cd71d27a0ff7ed3e481e8ff94c6
ms.sourcegitcommit: b1d2bd71aaff7020dfb3f7874799e03df3657cd4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
# Azure Active Directory 直通身份验证：当前限制
<a id="azure-active-directory-pass-through-authentication-current-limitations" class="xliff"></a>

>[!NOTE]
>Azure AD 直通身份验证目前为预览版。 这是一项免费功能，不需要拥有任何付费版本的 Azure AD 即可使用此功能。

## 支持的方案
<a id="supported-scenarios" class="xliff"></a>

预览版完全支持以下方案：

- 所有基于 Web 浏览器的应用程序
- 支持[新式身份验证](https://aka.ms/modernauthga)的 Office 365 客户端应用程序

## 不支持的方案
<a id="unsupported-scenarios" class="xliff"></a>

预览版_不_支持以下方案：

- 旧式 Office 客户端应用程序和 Exchange ActiveSync（即，移动设备上的本机电子邮件应用程序）。 我们建议组织在可能的情况下改用新式验证。 借助新式验证，不仅可以获得直通身份验证支持，而且还有助于使用条件性访问功能（例如多重身份验证，MFA）来保护身份。
- 适用于 Windows 10 设备的 Azure AD Join。
- PowerShell v1.0。 建议改为使用 PowerShell v2.0。

>[!IMPORTANT]
>现在还不支持作为方案的解决方法，启用直通身份验证时，默认也将启用密码哈希同步。 密码哈希同步仅充当前面方案的回退，而不充当到直通身份验证的泛型回退。 如果不需要这些方案，可以在 Azure AD Connect 向导中的[可选功能](active-directory-aadconnect-get-started-custom.md#optional-features)页上关闭密码哈希同步。

## 后续步骤
<a id="next-steps" class="xliff"></a>
- [快速入门](active-directory-aadconnect-pass-through-authentication-quick-start.md) - 启动并运行 Azure AD 直通身份验证。
- [技术深入探讨](active-directory-aadconnect-pass-through-authentication-how-it-works.md) - 了解此功能的工作原理。
- [常见问题解答](active-directory-aadconnect-pass-through-authentication-faq.md) - 常见问题的解答。
- [故障排除](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) - 了解如何解决此功能的常见问题。
- [Azure AD 无缝 SSO](active-directory-aadconnect-sso.md) - 了解有关此补充功能的详细信息。
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - 用于提出新的功能请求。

