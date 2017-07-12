---
title: "Azure AD Connect：直通身份验证 - 工作原理 | Microsoft Docs"
description: "本文介绍 Azure Active Directory 直通身份验证的工作原理。"
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
origin.date: 06/12/2017
ms.date: 06/21/2017
ms.author: v-junlch
ms.openlocfilehash: 362cfc949a247b8a48d05b594bd1dd35419c7d8c
ms.sourcegitcommit: b1d2bd71aaff7020dfb3f7874799e03df3657cd4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
<a id="azure-active-directory-pass-through-authentication-technical-deep-dive" class="xliff"></a>

# Azure Active Directory 直通身份验证：技术深入探讨

<a id="how-does-azure-active-directory-pass-through-authentication-work" class="xliff"></a>

## Azure Active Directory 直通身份验证的工作原理

当用户尝试登录到受 Azure Active Directory (Azure AD) 保护的应用程序时，如果已在租户中启用直通身份验证，将执行以下步骤：

1. 用户尝试访问某个应用程序（例如 Outlook Web 应用 - https://outlook.office365.com/owa/）。
2. 如果该用户尚未登录，则会重定向到 Azure AD 登录页。
3. 用户在 Azure AD 登录页中输入其用户名和密码，然后单击“登录”按钮。
4. Azure AD 收到登录请求后，会将该用户名和密码（已使用公钥加密）排入队列。
5. 本地直通身份验证代理向队列发出出站调用，并检索该用户名和已加密的密码。
6. 代理使用密码私钥解密密码。
7. 然后，代理使用标准 Windows API（类似于 Active Directory 联合身份验证服务使用的机制）针对 Active Directory 验证该用户名和密码。 用户名可以是本地默认用户名（通常为 `userPrincipalName`），也可以是 Azure AD Connect 中配置的另一个属性（称为 `Alternate ID`）。
8. 然后，本地 Active Directory 域控制器 (DC) 评估请求，并向代理返回相应的响应（成功、失败、密码已过期或用户已锁定）。
9. 代理随后将此响应返回给 Azure AD。
10. Azure AD 评估响应并相应地向用户做出响应 - 例如，立即将用户登录，或请求多重身份验证 (MFA)。
11. 如果用户登录成功，则可以访问应用程序。

下图演示了涉及的所有组件和步骤。

![直通身份验证](./media/active-directory-aadconnect-pass-through-authentication/pta2.png)

<a id="next-steps" class="xliff"></a>

## 后续步骤
- [**当前限制**](active-directory-aadconnect-pass-through-authentication-current-limitations.md) - 此功能目前以预览版提供。 了解支持的方案和不支持的方案。
- [**快速入门**](active-directory-aadconnect-pass-through-authentication-quick-start.md) - 启动并运行 Azure AD 直通身份验证。
- [**常见问题**](active-directory-aadconnect-pass-through-authentication-faq.md) - 常见问题的解答。
- [**故障排除**](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) - 了解如何解决此功能的常见问题。
- [**Azure AD 无缝 SSO**](active-directory-aadconnect-sso.md) - 了解有关此补充功能的详细信息。
- [**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - 针对提出新的功能请求。

