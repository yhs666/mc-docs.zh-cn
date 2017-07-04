---
title: "如何配置新的多租户应用程序 | Microsoft Docs"
description: "如何为通过 Azure AD 进行开发和注册的自定义应用程序配置单一登录。"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: v-junlch
ms.openlocfilehash: 1b7550e0bdce8296c38f72531846bec3cd16fac2
ms.sourcegitcommit: 6728c686935e3cdfaa93a7a364b959ab2ebad361
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
# <a name="how-to-configure-a-new-multi-tenant-application"></a>如何配置新的多租户应用程序

通过用于 OpenID Connect、SAML 2.0 或 WS-Fed 的 Azure AD 进行联合身份验证时，可以在应用中自动启用联合单一登录 (SSO)。 假如最终用户已经通过 Azure AD 建立了一个会话，但仍需登录，则可能是因为应用配置错误。

- 如果使用 ADAL/MSAL，请确保将 **PromptBehavior** 设置为“自动”而非“始终”。

- 若要生成移动应用，可能需要进行其他配置才能启用中转或非中转 SSO。

对于 Android，请参阅[在 Android 中启用跨应用 SSO](develop/active-directory-sso-android.md)。<br>

对于 iOS，请参阅[在 iOS 中启用跨应用 SSO](develop/active-directory-sso-ios.md)。

## <a name="next-steps"></a>后续步骤

[Azure AD SSO](active-directory-appssoaccess-whatis.md)<br>

[在 Android 中启用跨应用 SSO](develop/active-directory-sso-android.md)<br>

[在 iOS 中启用跨应用 SSO](develop/active-directory-sso-ios.md)<br>

[AzureAD v2.0 聚合应用的许可和权限](develop/active-directory-v2-scopes.md)<br>

[AzureAD StackOverflow](http://stackoverflow.com/questions/tagged/azure-active-directory)
