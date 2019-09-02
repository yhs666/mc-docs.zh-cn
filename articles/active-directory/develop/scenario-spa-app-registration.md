---
title: 单页应用程序（应用注册）- Microsoft 标识平台
description: 了解如何生成单页应用程序（应用注册）
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
origin.date: 05/07/2019
ms.date: 06/20/2019
ms.author: v-junlch
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: e06fcfabed6bd328306c8de9e415f7d9a2c379a0
ms.sourcegitcommit: 9d5fd3184b6a47bf3b60ffdeeee22a08354ca6b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2019
ms.locfileid: "67305848"
---
# <a name="single-page-application---app-registration"></a>单页应用程序 - 应用注册

此页介绍用于单页应用程序 (SPA) 的应用注册详细信息。

按步骤[将新应用程序注册到 Microsoft 标识平台](quickstart-register-app.md)，然后为应用程序选择支持的帐户。 SPA 方案可以支持对你组织或任何组织中的帐户进行身份验证。

接下来，了解适用于单页应用程序的应用程序注册的具体方面。

## <a name="register-a-redirect-uri"></a>注册重定向 URI

隐式流将重定向中的令牌发送到在 Web 浏览器中运行的单页应用程序。 因此，必须注册一个重定向 URI，以便应用程序可以从其接收令牌，这是很重要的要求。 请确保重定向 URI 与应用程序的 URI 完全匹配。

在 [Azure 门户](https://portal.azure.cn/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview)中导航到注册的应用程序，在应用程序的“身份验证”页  中选择 **Web** 平台，然后在“重定向 URI”字段中输入应用程序的重定向 URI 的值。 

## <a name="enable-the-implicit-flow"></a>启用隐式流

在同一“身份验证”页的“高级设置”下，还必须启用“隐式授权”。    如果应用程序仅执行登录用户和获取 ID 令牌的操作，则只需启用“ID 令牌”复选框即可。 

如果应用程序还需获取访问令牌来调用 API，则请确保还要启用“访问令牌”复选框。  有关详细信息，请参阅 [ID 令牌](./id-tokens.md)和[访问令牌](./access-tokens.md)。

## <a name="api-permissions"></a>API 权限

单页应用程序可代表已登录用户调用 API。 它们需要请求委托的权限。 有关详细信息，请参阅[添加用于访问 Web API 的权限](quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis)

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [应用的代码配置](scenario-spa-app-configuration.md)

