---
title: "如何配置新的多租户应用程序 | Microsoft Docs"
description: "如何为通过 Azure AD 进行开发和注册的自定义应用程序配置单一登录。"
services: active-directory
documentationcenter: 
author: alexchen2016
manager: digimobile
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 07/11/2017
ms.date: 07/28/2017
ms.author: v-junlch
ms.openlocfilehash: 01e7a26ddb7c633085098810914589e8e61ff093
ms.sourcegitcommit: f02cdaff1517278edd9f26f69f510b2920fc6206
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/05/2018
---
# <a name="how-to-configure-a-new-multi-tenant-application"></a>如何配置新的多租户应用程序

通过用于 OpenID Connect、SAML 2.0 或 WS-Fed 的 Azure AD 进行联合身份验证时，可以在应用中自动启用联合单一登录 (SSO)。 假如最终用户已经通过 Azure AD 建立了一个会话，但仍需登录，则可能是因为应用配置错误。

- 如果使用 ADAL/MSAL，请确保将 **PromptBehavior** 设置为“自动”而不是“始终”。

- 若要生成移动应用，可能需要进行其他配置才能启用中转或非中转 SSO。

对于 Android，请参阅[在 Android 中启用跨应用 SSO](/active-directory/develop/active-directory-sso-android)。<br>

对于 iOS，请参阅[在 iOS 中启用跨应用 SSO](/active-directory/develop/active-directory-sso-ios)。

## <a name="next-steps"></a>后续步骤

[Azure AD SSO](/active-directory/active-directory-appssoaccess-whatis)<br>

[在 Android 中启用跨应用 SSO](/active-directory/develop/active-directory-sso-android)<br>

[在 iOS 中启用跨应用 SSO](/active-directory/develop/active-directory-sso-ios)<br>


[AzureAD StackOverflow](http://stackoverflow.com/questions/tagged/azure-active-directory)

<!-- Update_Description: link update -->