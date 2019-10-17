---
title: 如何在 Azure Active Directory 标识保护中配置多重身份验证注册策略| Microsoft Docs
description: 了解如何配置“Azure AD 标识保护”多重身份验证注册策略。
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
origin.date: 05/01/2019
ms.date: 10/10/2019
author: MicrosoftGuyJFlo
manager: daveba
ms.author: v-junlch
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: b915def7b4bbc9738823e502ac13f213c062e4f5
ms.sourcegitcommit: 74f50c9678e190e2dbb857be530175f25da8905e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/12/2019
ms.locfileid: "72292088"
---
# <a name="how-to-configure-the-azure-multi-factor-authentication-registration-policy"></a>如何：配置 Azure 多重身份验证注册策略

Azure AD 标识保护通过配置条件访问策略，实现无论你登录到哪个新式身份验证应用，都需要注册 MFA，从而帮助你管理多重身份验证 (MFA) 注册的实施。 本文将说明策略的用途以及如何配置它。



## <a name="what-is-the-azure-multi-factor-authentication-registration-policy"></a>什么是 Azure 多重身份验证注册策略？

Azure 多重身份验证提供了一种方法来实现不只使用用户名和密码验证用户的身份。 它为用户登录提供了第二层保护。为了让用户能够响应 MFA 提示，他们必须首先注册 Azure 多重身份验证。

我们建议你要求用户登录使用 Azure 多重身份验证，因为这种身份验证方法可以：

- 提供强式身份验证和一系列简单的验证选项
- 在准备组织以在“标识保护”中进行保护以及从风险检测中恢复方面起关键作用

有关 MFA 的更多详细信息，请参阅[什么是 Azure 多重身份验证？](../authentication/howto-mfa-getstarted.md)

## <a name="how-do-i-access-the-registration-policy"></a>如何访问注册策略？

MFA 注册策略位于[“Azure AD 标识保护”页](https://portal.azure.cn/#blade/Microsoft_AAD_ProtectionCenter/IdentitySecurityDashboardMenuBlade/SignInPolicy)上的“配置”  部分中。

![MFA 策略](./media/howto-mfa-policy/1014.png)

## <a name="policy-settings"></a>策略设置

配置 MFA 注册策略时，需要进行以下配置更改：

- 策略应用到的用户和组。 请记住排除组织的[紧急访问帐户](../users-groups-roles/directory-emergency-access.md)。

    ![用户和组](./media/howto-mfa-policy/11.png)

- 要强制实施的控制 - **需要 Azure MFA 注册**

    ![访问](./media/howto-mfa-policy/12.png)

- “强制实施策略”应设置为“开”  。

    ![强制实施策略](./media/howto-mfa-policy/14.png)

- **保存**策略

## <a name="user-experience"></a>用户体验

Azure Active Directory 标识保护将在用户下次以交互方式登录时提示他们注册。

如需相关用户体验的概述，请参阅：

- [多重身份验证注册流程](flows.md#multi-factor-authentication-registration)。  
- [Azure AD 标识保护中的登录体验](flows.md)。  

## <a name="next-steps"></a>后续步骤

若要获取“Azure AD 标识保护”的概述，请参阅 [Azure AD 标识保护概述](overview.md)。

<!-- Update_Description: wording update -->