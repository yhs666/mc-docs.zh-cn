---
title: 调用 Web API 的 Web 应用（获取应用的令牌）- Microsoft 标识平台
description: 了解如何生成调用 Web API 的 Web 应用（获取应用的令牌）
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
origin.date: 09/09/2019
ms.date: 10/09/2019
ms.author: v-junlch
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: cf7c3dab7d1d743fa7a132ae3e383d88461ed00a
ms.sourcegitcommit: 74f50c9678e190e2dbb857be530175f25da8905e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/12/2019
ms.locfileid: "72292054"
---
# <a name="web-app-that-calls-web-apis---acquire-a-token-for-the-app"></a>调用 Web API 的 Web 应用 - 获取应用的令牌

生成客户端应用程序对象以后，你将使用它来获取令牌以调用 Web API。 然后，在 ASP.NET 或 ASP.NET Core 的控制器中调用 Web API。 具体操作是：

- 使用令牌缓存获取 Web API 的令牌。 若要获取此令牌，请调用 `AcquireTokenSilent`。
- 使用访问令牌调用受保护的 API。

## <a name="aspnet-core"></a>ASP.NET Core

控制器方法受 `[Authorize]` 属性的保护，该属性会强制经身份验证的用户使用 Web 应用。 下面是用于调用 Microsoft Graph 的代码。

```CSharp
[Authorize]
public class HomeController : Controller
{
 ...
}
```

下面是 HomeController 操作的简化代码，该操作获取调用 Microsoft Graph 所需的令牌。

```CSharp
public async Task<IActionResult> Profile()
{
 var application = BuildConfidentialClientApplication(HttpContext, HttpContext.User);
 string accountIdentifier = claimsPrincipal.GetMsalAccountId();
 string loginHint = claimsPrincipal.GetLoginHint();

 // Get the account
 IAccount account = await application.GetAccountAsync(accountIdentifier);

 // Special case for guest users as the Guest iod / tenant id are not surfaced.
 if (account == null)
 {
  var accounts = await application.GetAccountsAsync();
  account = accounts.FirstOrDefault(a => a.Username == loginHint);
 }

 AuthenticationResult result;
 result = await application.AcquireTokenSilent(new []{"https://microsoftgraph.chinacloudapi.cn/user.read"}, account)
                            .ExecuteAsync();
 var accessToken = result.AccessToken;
 ...
 // use the access token to call a web API
}
```

若要更全面地了解此方案所需的代码，请参阅 [ms-identity-aspnetcore-webapp-tutorial](https://github.com/Azure-Samples/ms-identity-aspnetcore-webapp-tutorial) 教程的阶段 2 ([2-1-Web App Calls Microsoft Graph](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/2-WebApp-graph-user/2-1-Call-MSGraph)) 步骤。

有许多其他的复杂情况，例如：

- 为 Web 应用实现令牌缓存（本教程提供多个实现）
- 当用户注销时从缓存中删除帐户
- 调用多个 API，包括设置增量许可

## <a name="aspnet"></a>ASP.NET

ASP.NET 中的情况类似：

- 受 [Authorize] 属性保护的控制器操作会提取控制器的 `ClaimsPrincipal` 成员的租户 ID 和用户 ID。 （ASP.NET 使用 `HttpContext.User`。）
- 然后，它会构建一个 MSAL.NET `IConfidentialClientApplication`。
- 最后，它调用机密客户端应用程序的 `AcquireTokenSilent` 方法。

此代码类似于为 ASP.NET Core 显示的代码。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [调用 Web API](scenario-web-app-call-api-call-api.md)

<!-- Update_Description: wording update -->