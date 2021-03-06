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
origin.date: 09/30/2019
ms.date: 11/06/2019
ms.author: v-junlch
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2af84870aa0a5174e4614c5c87944c749d2cddab
ms.sourcegitcommit: a88cc623ed0f37731cb7cd378febf3de57cf5b45
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/08/2019
ms.locfileid: "73830912"
---
# <a name="web-app-that-calls-web-apis---sign-in"></a>用于调用 Web API 的 Web 应用 - 登录

你已经知道如何向 Web 应用添加登录。 你在[用于登录用户的 Web 应用 - 添加登录](scenario-web-app-sign-user-sign-in.md)中学习它。

这里的不同之处在于，当用户从此应用程序或任何其他应用程序中注销以后，你需要从令牌缓存中删除与用户相关联的令牌。

## <a name="intercepting-the-callback-after-sign-out---single-sign-out"></a>在注销后拦截回叫 - 单一注销

应用程序可以拦截 `logout` 后事件，例如，清除与已注销帐户相关联的令牌缓存的条目。Web 应用将在缓存中存储用户的访问令牌。 拦截 `logout` 后回叫后，Web 应用程序即可从令牌缓存中删除用户。

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

[WebAppServiceCollectionExtensions.cs#L151-L157](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/db7f74fd7e65bab9d21092ac1b98a00803e5ceb2/Microsoft.Identity.Web/WebAppServiceCollectionExtensions.cs#L151-L157) 的 `AddMsal()` 方法说明了此机制。

使用为应用程序注册的**注销 URL** 可以实现单一注销。Microsoft 标识平台 `logout` 终结点会调用注册到应用程序的**注销 URL**。 如果注销是从你的 Web 应用或者另一 Web 应用或浏览器启动的，则会发生此调用。 有关详细信息，请参阅[单一注销](v2-protocols-oidc.md#single-sign-out)。

```CSharp
public static class WebAppServiceCollectionExtensions
{
 public static IServiceCollection AddMsal(this IServiceCollection services, IConfiguration configuration, IEnumerable<string> initialScopes, string configSectionName = "AzureAd")
 {
  // Code omitted here

  services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options =>
  {
   // Code omitted here

   // Handling the sign-out: removing the account from MSAL.NET cache
   options.Events.OnRedirectToIdentityProviderForSignOut = async context =>
   {
    // Remove the account from MSAL.NET token cache
    var tokenAcquisition = context.HttpContext.RequestServices.GetRequiredService<ITokenAcquisition>();
    await tokenAcquisition.RemoveAccountAsync(context).ConfigureAwait(false);
   };
  });
  return services;
 }
}
```

RemoveAccountAsync 的代码可从 [Microsoft.Identity.Web/TokenAcquisition.cs#L264-L288](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/db7f74fd7e65bab9d21092ac1b98a00803e5ceb2/Microsoft.Identity.Web/TokenAcquisition.cs#L264-L288) 获得。

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

ASP.NET 示例不会在全局注销时删除缓存中的帐户

# <a name="javatabjava"></a>[Java](#tab/java)

Java 示例不会在全局注销时删除缓存中的帐户

# <a name="pythontabpython"></a>[Python](#tab/python)

Python 示例不会在全局注销时删除缓存中的帐户

---

## <a name="next-steps"></a>后续步骤

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

> [!div class="nextstepaction"]
> [获取 Web 应用的令牌](/active-directory/develop/scenario-web-app-call-api-acquire-token?tabs=aspnetcore)

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

> [!div class="nextstepaction"]
> [获取 Web 应用的令牌](/active-directory/develop/scenario-web-app-call-api-acquire-token?tabs=aspnet)

# <a name="javatabjava"></a>[Java](#tab/java)

> [!div class="nextstepaction"]
> [获取 Web 应用的令牌](/active-directory/develop/scenario-web-app-call-api-acquire-token?tabs=java)

# <a name="pythontabpython"></a>[Python](#tab/python)

> [!div class="nextstepaction"]
> [获取 Web 应用的令牌](/active-directory/develop/scenario-web-app-call-api-acquire-token?tabs=python)

---

<!-- Update_Description: wording update -->