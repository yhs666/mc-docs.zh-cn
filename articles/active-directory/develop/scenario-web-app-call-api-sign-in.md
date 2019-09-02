---
title: 调用 Web API 的 Web 应用（登录）- Microsoft 标识平台
description: 了解如何构建调用 Web API 的 Web 应用（登录）
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
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
ms.openlocfilehash: 9932764f06953b23b93e32e872ffa74d5285d89a
ms.sourcegitcommit: 9d5fd3184b6a47bf3b60ffdeeee22a08354ca6b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2019
ms.locfileid: "67305988"
---
# <a name="web-app-that-calls-web-apis---sign-in"></a>用于调用 Web API 的 Web 应用 - 登录

你已经知道如何向 Web 应用添加登录。 你在[用于登录用户的 Web 应用 - 添加登录](scenario-web-app-sign-user-sign-in.md)中学习它。

这里的不同之处在于，当用户从此应用程序或任何其他应用程序中注销以后，你需要从令牌缓存中删除与用户相关联的令牌。

## <a name="intercepting-the-callback-after-sign-out---single-sign-out"></a>在注销后拦截回叫 - 单一注销

应用程序可以拦截 `logout` 后事件，例如，清除与已注销帐户相关联的令牌缓存的条目。我们会在本教程的第二部分（介绍调用 Web API 的 Web 应用）看到 Web 应用会将用户的访问令牌存储在缓存中。 拦截 `logout` 后回叫后，Web 应用程序即可从令牌缓存中删除用户。 此机制在 [StartupHelper.cs L137-143](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/b87a1d859ff9f9a4a98eb7b701e6a1128d802ec5/Microsoft.Identity.Web/StartupHelpers.cs#L137-L143) 的 `AddMsal()` 方法中进行了演示

使用为应用程序注册的**注销 URL** 可以实现单一注销。Microsoft 标识平台 `logout` 终结点会调用注册到应用程序的**注销 URL**。 如果注销是从你的 Web 应用或者另一 Web 应用或浏览器启动的，则会发生此调用。 有关详细信息，请参阅概念文档中的[单一注销](/active-directory/develop/v2-protocols-oidc#single-sign-out)。

```CSharp
public static IServiceCollection AddMsal(this IServiceCollection services, IEnumerable<string> initialScopes)
{
    services.AddTokenAcquisition();

    services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options =>
    {
     ...
        // Handling the sign-out: removing the account from MSAL.NET cache
        options.Events.OnRedirectToIdentityProviderForSignOut = async context =>
        {
            // Remove the account from MSAL.NET token cache
            var _tokenAcquisition = context.HttpContext.RequestServices.GetRequiredService<ITokenAcquisition>();
            await _tokenAcquisition.RemoveAccount(context);
        };
    });
    return services;
}
```

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [获取 Web 应用的令牌](scenario-web-app-call-api-acquire-token.md)

