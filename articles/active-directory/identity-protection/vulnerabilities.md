---
title: “Azure Active Directory 标识保护”检测到的漏洞
description: “Azure Active Directory 标识保护”检测到的漏洞概述。
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: article
origin.date: 04/09/2019
ms.date: 08/09/2019
ms.author: v-junlch
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 933e1e42a97b7be944b5a2d3ba0f72091ed58c0b
ms.sourcegitcommit: 44548f2ebec1246f6ac799f5b2640ad1b5d7c8a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/13/2019
ms.locfileid: "68973288"
---
# <a name="vulnerabilities-detected-by-azure-active-directory-identity-protection"></a>“Azure Active Directory 标识保护”检测到的漏洞

漏洞是环境中可能由攻击者利用的弱点。 我们建议管理员解决这些漏洞，以改善其组织的安全状况。

![标识保护报告的漏洞](./media/vulnerabilities/identity-protection-vulnerabilities.png)

以下部分概述了由“标识保护”报告的漏洞。

## <a name="multi-factor-authentication-registration-not-configured"></a>多重身份验证注册未配置

此漏洞有助于评估组织中 Azure 多重身份验证的部署。

Azure 多重身份验证为用户身份验证提供了第二层保护。 它可帮助保护对数据和应用程序的访问，同时满足用户对简单登录过程的需求。 Azure 多重身份验证提供易于使用的验证选项，如下所示：

* 电话呼叫
* 短信
* 移动应用通知
* OTP 验证码

我们建议将 Azure 多重身份验证用于用户登录。多重身份验证在通过“标识保护”提供的基于风险的条件访问策略中起重要作用。

有关详细信息，请参阅[什么是 Azure 多重身份验证？](../authentication/multi-factor-authentication.md)

## <a name="unmanaged-cloud-apps"></a>非托管的云应用

此漏洞可帮助你标识组织中的非托管云应用。

IT 人员通常不了解其组织中的所有云应用程序。 很容易理解为什么管理员对未经授权访问公司数据、可能的数据泄漏和其他安全风险有所顾虑。

## <a name="security-alerts-from-privileged-identity-management"></a>来自 Privileged Identity Management 的安全警报

此漏洞可帮助你发现和解决有关组织中特权标识的警报。  

为了允许用户执行特权操作，组织需要向用户授予 Azure AD、Azure 或 Office 365 资源或者其他 SaaS 应用的临时或永久访问特权。 其中每个特权用户都会增加组织的攻击面。 此漏洞可帮助你标识具有不必要访问特权的用户，并采取相应操作降低或消除他们带来的风险。

我们建议组织使用 Azure AD Privileged Identity Management 管理、控制和监视 Azure AD 以及 Office 365 或 Microsoft Intune 等其他 Microsoft Online Services 中的特权标识。

有关详细信息，请参阅 [Azure AD Privileged Identity Management](../privileged-identity-management/pim-configure.md)。

## <a name="see-also"></a>另请参阅

[Azure Active Directory 标识保护](/active-directory/identity-protection/overview)

