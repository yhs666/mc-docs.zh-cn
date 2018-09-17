---
title: 如何配置新的多租户应用程序 | Microsoft Docs
description: 如何为通过 Azure AD 进行开发和注册的自定义应用程序配置单一登录。
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
origin.date: 07/11/2017
ms.date: 09/06/2018
ms.author: v-junlch
ms.openlocfilehash: b3f391e6f2de2ac08c5c4d52166f1086b4a86c81
ms.sourcegitcommit: fd49281c58f34de20cc310d6cefb4568992cd675
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/06/2018
ms.locfileid: "43858435"
---
# <a name="how-to-configure-a-new-multi-tenant-application"></a>如何配置新的多租户应用程序

通过用于 OpenID Connect、SAML 2.0 或 WS-Fed 的 Azure AD 进行联合身份验证时，可以在应用中自动启用联合单一登录 (SSO)。 假如最终用户已经通过 Azure AD 建立了一个会话，但仍需登录，则可能是因为应用配置错误。

- 如果使用 ADAL/MSAL，请确保将 **PromptBehavior** 设置为“自动”而不是“始终”。

- 若要生成移动应用，可能需要进行其他配置才能启用中转或非中转 SSO。

对于 Android，请参阅[在 Android 中启用跨应用 SSO](/active-directory/develop/active-directory-sso-android)。<br>

对于 iOS，请参阅[在 iOS 中启用跨应用 SSO](/active-directory/develop/active-directory-sso-ios)。

## <a name="next-steps"></a>后续步骤

[在 Android 中启用跨应用 SSO](/active-directory/develop/active-directory-sso-android)<br>

[在 iOS 中启用跨应用 SSO](/active-directory/develop/active-directory-sso-ios)<br>

[将应用与 AzureAD 集成](/active-directory/develop/active-directory-integrating-applications)<br>

[AzureAD StackOverflow](http://stackoverflow.com/questions/tagged/azure-active-directory)

<!-- Update_Description: update metedata properties -->