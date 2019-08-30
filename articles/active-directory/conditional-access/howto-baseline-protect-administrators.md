---
title: 基线策略：要求对管理员进行 MFA - Azure Active Directory
description: 要求对管理员进行多重身份验证的条件访问策略
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
origin.date: 05/16/2019
ms.date: 08/08/2019
ms.author: v-junlch
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 017b5077b0fc62c490fb972e1c3e8fe05e1165ba
ms.sourcegitcommit: 44548f2ebec1246f6ac799f5b2640ad1b5d7c8a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/13/2019
ms.locfileid: "68972957"
---
# <a name="baseline-policy-require-mfa-for-admins-preview"></a>基线策略:要求对管理员进行 MFA (预览)

对特权帐户具有访问权限的用户对你的环境具有不受限制的访问权限。 鉴于这些帐户具有的权利，应当特别小心地对待它们。 增强对特权帐户的保护的一种常用方法是要求在使用这些帐户登录时进行更强的帐户验证。 在 Azure Active Directory 中，可以通过要求进行多重身份验证 (MFA) 来实现更强的帐户验证。

**要求对管理员进行 MFA (预览)**  是一项[基线策略](concept-baseline-protection.md)，要求每次下述特权管理员角色之一登录时都进行 MFA：

* 全局管理员
* SharePoint 管理员
* Exchange 管理员
* 条件访问管理员
* 安全管理员
* 支持管理员/密码管理员
* 计费管理员
* 用户管理员

在启用“要求对管理员进行 MFA”策略后，以上九种管理员角色将需要使用 Authenticator 应用注册 MFA。 MFA 注册完成后，管理员在每次登录时都需要执行 MFA。

## <a name="deployment-considerations"></a>部署注意事项

由于**要求对管理员进行 MFA (预览)** 策略适用于所有重要的管理员，因此需进行多项考虑，确保顺利部署。 这些考虑事项包括：确定 Azure AD 中不能或不应该执行 MFA 的用户和服务主体，以及组织所使用的不支持新式身份验证的应用程序和客户端。

### <a name="legacy-protocols"></a>旧版协议

旧版身份验证协议（IMAP、SMTP、POP3 等）由邮件客户端用来进行身份验证请求。 这些协议不支持 MFA。 在 Microsoft 看来，大多数帐户入侵事件都是由攻击者对旧版协议进行攻击，尝试绕过 MFA。 为了确保在登录管理帐户时强制实施 MFA，同时确保攻击者无法绕过 MFA，此策略阻止从旧版协议进行的针对管理员帐户的所有身份验证请求。

> [!WARNING]
> 在启用此策略之前，请确保管理员使用的不是旧版身份验证协议。 请参阅[如何：使用条件访问来阻止 Azure AD 上的旧式身份验证](howto-baseline-protect-legacy-auth.md#identify-legacy-authentication-use)一文，了解详细信息。

## <a name="enable-the-baseline-policy"></a>启用基线策略

策略“基线策略:  要求对管理员进行 MFA (预览)”已预配置，当你在 Azure 门户中导航到“条件访问”边栏选项卡时，它会显示在顶部。

若要启用此策略并保护管理员，请执行以下操作：

1. 以全局管理员、安全管理员或条件访问管理员的身份登录到 **Azure 门户**。
1. 浏览到“Azure Active Directory”   > “条件访问”  。
1. 在策略列表中选择“基线策略:  要求对管理员进行 MFA (预览)”。
1. 将“启用策略”设置为“立即使用策略”。  
1. 单击“保存” **** 。

> [!WARNING]
> 当此策略为预览版时，曾经有一个“将来自动启用策略”选项  。 我们删除了此选项，尽量减少对用户的突发影响。 如果你在此选项可用时选择了它，则现在系统会自动选择“不使用策略”。  若要使用此基线策略，请查看上面的步骤，了解如何启用它。

## <a name="next-steps"></a>后续步骤

有关详细信息，请参阅：

* [条件访问基线保护策略](concept-baseline-protection.md)
* [什么是 Azure Active Directory 中的条件访问？](overview.md)

