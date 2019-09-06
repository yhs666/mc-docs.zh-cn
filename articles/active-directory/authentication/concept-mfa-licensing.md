---
title: Azure MFA 版本和使用计划 - Azure Active Directory
description: 有关多重身份验证客户端以及可用的方法和版本的信息。
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
origin.date: 06/03/2018
ms.date: 08/29/2019
ms.author: v-junlch
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9c3ae02788dd9018262669a796065c4c3b1c75b7
ms.sourcegitcommit: 18a0d2561c8b60819671ca8e4ea8147fe9d41feb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2019
ms.locfileid: "70134165"
---
# <a name="how-to-get-azure-multi-factor-authentication"></a>如何获取 Azure 多重身份验证

如果想要保护帐户，应该在组织中标配双重验证。 对资源拥有访问特权的帐户尤其需要此功能。 为此，Microsoft 为 Office 365 和 Azure Active Directory (Azure AD) 管理员提供了基本的双重验证功能，无需额外费用。 如果想要升级管理员的功能，或者将双重验证扩展到其他用户，可以用多种方式购买 Azure 多重身份验证。

> [!IMPORTANT]
> 本文旨在指导用户如何以不同的方式购买 Azure 多重身份验证。 有关定价和计费的具体详细信息，请始终参阅[多重身份验证定价页](https://www.azure.cn/pricing/details/multi-factor-authentication/)。
>

## <a name="available-versions-of-azure-multi-factor-authentication"></a>可用的 Azure 多重身份验证版本

下表介绍了多重身份验证的三个版本之间的差别：

| 版本 | 说明 |
| --- | --- |
| 适用于 Office 365 的多重身份验证 <br> Microsoft 365 商业版 | 此版本通过 Office 365 或 Microsoft 365 门户进行管理。 管理员可以[使用双重验证来保护 Office 365 资源](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6)。 此版本是 Office 365 或 Microsoft 365 商业版订阅的一部分。 |
| 面向 Azure AD 管理员的多重身份验证 | Azure AD 租户中被分配了 Azure AD 全局管理员角色的用户可以免费启用双重验证。 |

## <a name="feature-comparison-of-versions"></a>版本功能比较

下表提供了 Azure 多重身份验证的各个版本中可用的功能列表。

> [!NOTE]
> 此比较表讨论了每个版本的多重身份验证的部分功能。 如果拥有完整的 Azure 多重身份验证服务，某些功能可能不可用，具体取决于是否[在云中使用 MFA](howto-mfa-getstarted.md)。


| 功能 | 适用于 Office 365 的多重身份验证 | 面向 Azure AD 管理员的多重身份验证 | Azure 多重身份验证 |
| --- |:---:|:---:|:---:|
| 使用 MFA 保护 Azure AD 管理员帐户 |● |●（仅适用于 Azure AD 全局管理员帐户） |● |
| 将移动应用用作第二个因素 |● |● |● |
| 将电话呼叫用作第二个因素 |● |● |● |
| 将短信用作第二个因素 |● |● |● |
| 管理员控制验证方法 |● |● |● |
| PIN 模式 | | |● |
| 通话的自定义问候语 | | |● |
| 通话的自定义呼叫方 ID | | |● |

## <a name="how-to-turn-on-azure-multi-factor-authentication-for-azure-ad-administrators"></a>如何为 Azure AD 管理员启用 Azure 多重身份验证

Azure AD 租户中被分配了全局管理员角色的用户可以免费为其 Azure AD 全局管理员帐户启用双重验证。 如果使用的是 Microsoft 帐户，则可以使用 Microsoft 帐户支持文章[关于双重验证](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification)中的指南注册多重身份验证。 如果未使用 Microsoft 帐户，请使用文章[如何对用户或组要求双重验证](howto-mfa-userstates.md)中的指南为全局管理员启用多重身份验证。

## <a name="next-steps"></a>后续步骤

- 有关定价详细信息，请参阅 [Azure MFA 定价](https://www.azure.cn/pricing/details/multi-factor-authentication/)。

<!-- Update_Description: wording update -->
