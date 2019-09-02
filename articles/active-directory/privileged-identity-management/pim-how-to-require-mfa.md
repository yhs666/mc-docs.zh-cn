---
title: 多重身份验证 (MFA) 和 PIM - Azure Active Directory | Microsoft Docs
description: 了解 Azure AD Privileged Identity Management (PIM) 如何验证多重身份验证 (MFA)。
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
origin.date: 08/31/2018
ms.date: 08/08/2019
ms.author: v-junlch
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2305c6b851d343912b87d7a4094f73d8f63d96d4
ms.sourcegitcommit: 44548f2ebec1246f6ac799f5b2640ad1b5d7c8a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/13/2019
ms.locfileid: "68973454"
---
# <a name="multi-factor-authentication-mfa-and-pim"></a>多重身份验证 (MFA) 和 PIM

我们建议要求所有管理员使用多重身份验证 (MFA)。 这可降低因密码泄露而受到攻击的风险。

可以请求用户在登录后完成 MFA 质询。 还可以要求用户在 Azure Active Directory (Azure AD) Privileged Identity Management (PIM) 中激活角色后完成 MFA 质询。 这样一来，如果用户在登录后未完成 MFA 质询，PIM 会提示他们完成此操作。

> [!IMPORTANT]
> 目前，Azure MFA 仅适用于工作或学校帐户，不适用于 Microsoft 帐户（通常用于登录 Skype、Xbox、Outlook.com 等 Microsoft 服务的个人帐户）。 因此，使用 Microsoft 帐户的任何人都不是符合条件的管理员，因为他们无法使用 MFA 激活其角色。 如果这些用户需要继续使用 Microsoft 帐户管理工作负荷，请立即将其提升到永久管理员。

## <a name="how-pim-validates-mfa"></a>PIM 如何验证 MFA

当用户激活角色时，有一个选项可用于验证 MFA。

最简单的选项是依赖于正在激活特权角色的用户的 Azure MFA。 若要执行此操作，首先检查这些用户是否已获得许可（如有必要），并且是否已注册 Azure MFA。 有关如何部署 Azure MFA 的详细信息，请参阅[部署基于云的 Azure 多重身份验证](../authentication/howto-mfa-getstarted.md)。 建议将 Azure AD 配置为在用户登录后针对这些用户强制执行 MFA。 这是因为 MFA 检查由 PIM 本身执行。

## <a name="next-steps"></a>后续步骤

- [在 PIM 中配置 Azure AD 角色设置](pim-how-to-change-default-settings.md)
- [在 PIM 中配置 Azure 资源角色设置](pim-resource-roles-configure-role-settings.md)

