---
title: "Azure AD Connect：直通身份验证 | Microsoft Docs"
description: "本文介绍 Azure Active Directory (Azure AD) 直通身份验证，以及它如何针对本地 Active Directory 来验证用户密码，从而实现 Azure AD 登录。"
services: active-directory
keywords: "什么是 Azure AD Connect 直通身份验证, 安装 Active Directory, Azure AD 所需的组件, SSO, 单一登录"
documentationcenter: 
author: swkrish
manager: digimobile
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 06/12/2017
ms.date: 06/21/2017
ms.author: v-junlch
ms.openlocfilehash: 41a4fc24981d3f63915c7bee20464d2a32b43350
ms.sourcegitcommit: b1d2bd71aaff7020dfb3f7874799e03df3657cd4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
<a id="user-sign-in-with-azure-active-directory-pass-through-authentication" class="xliff"></a>

# 使用 Azure Active Directory 直通身份验证进行用户登录

<a id="what-is-azure-active-directory-pass-through-authentication" class="xliff"></a>

## 什么是 Azure Active Directory 直通身份验证？

Azure Active Directory (Azure AD) 直通身份验证可让用户使用相同的密码登录到在本地应用程序和基于云的应用程序。 此功能为用户提供更好的体验 - 用户可以少记住一个密码，同时可以降低 IT 支持成本，因为用户不太可能会忘记登录方法。 当用户使用 Azure AD 登录时，此功能可针对本地 Active Directory 直接验证用户的密码。

此功能可替代 [Azure AD 密码哈希同步](active-directory-aadconnectsync-implement-password-synchronization.md)，为组织提供相同的云身份验证优势。 但是，某些组织的安全和合规性策略不允许在其内部边界之外发送用户的密码，即使采用哈希格式。 直通身份验证就是这种组织的理想解决方案。

![Azure AD 直通身份验证](./media/active-directory-aadconnect-pass-through-authentication/pta1.png)

可将直通身份验证与[无缝单一登录](active-directory-aadconnect-sso.md)功能结合使用。 这样一来，当用户在其企业计算机上访问位于你的企业网络中的应用程序时，不需要键入密码即可登录。

<a id="key-benefits-of-using-azure-ad-pass-through-authentication" class="xliff"></a>

## 使用 Azure AD 直通身份验证的重要优势

- *更好的用户体验*
  - 用户使用相同的密码登录到本地应用程序和基于云的应用程序。
  - 在解决密码相关的问题时，用户可以花费更少的时间来与 IT 支持人员沟通。

- *易于部署和管理*
  - 无需复杂的本地部署或网络配置。
  - 只需在本地安装一个轻型代理。
  - 无管理开销。 代理会自动接收改进和 Bug 修复。
- *安全*
  - 本地密码永远不会以任何形式存储在云中。
  - 代理只从你的网络内部建立出站连接。 因此，无需在外围网络（也称 DMZ）中安装代理。
  - 无缝使用 Azure AD 条件访问策略（包括多重身份验证 (MFA)）保护用户帐户。
- *高度可用*
  - 可在多台本地服务器上安装其他代理，提供登录请求的高可用性。

<a id="feature-highlights" class="xliff"></a>

## 功能特点

- 支持用户登录到所有基于 Web 浏览器的应用程序和使用[新式身份验证](https://aka.ms/modernauthga)的 Microsoft Office 客户端应用程序。
- 登录用户名可以是本地默认用户名 (`userPrincipalName`)，也可以是 Azure AD Connect 中配置的另一个属性（称为 `Alternate ID`）。
- 该功能与多重身份验证 (MFA) 等条件访问功能无缝配合，帮助保护用户。
- 如果 AD 林之间存在信任关系并且正确配置了名称后缀路由，则支持多林环境。
- 这是一项免费功能，不需要拥有任何付费版本的 Azure AD 即可使用此功能。
- 可通过 [Azure AD Connect](active-directory-aadconnect.md) 启用此功能。
- 此功能使用轻型本地代理侦听和响应密码验证请求。
- 安装多个代理可提供登录请求的高可用性。

<a id="next-steps" class="xliff"></a>

## 后续步骤

- [**快速入门**](active-directory-aadconnect-pass-through-authentication-quick-start.md) - 启动并运行 Azure AD 直通身份验证。
- [**当前限制**](active-directory-aadconnect-pass-through-authentication-current-limitations.md) - 此功能目前以预览版提供。 了解支持的方案和不支持的方案。
- [**技术深入探讨**](active-directory-aadconnect-pass-through-authentication-how-it-works.md) - 了解此功能的工作原理。
- [**常见问题**](active-directory-aadconnect-pass-through-authentication-faq.md) - 常见问题的解答。
- [**故障排除**](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) - 了解如何解决此功能的常见问题。
- [**Azure AD 无缝 SSO**](active-directory-aadconnect-sso.md) - 了解有关此补充功能的详细信息。
- [**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - 针对提出新的功能请求。

