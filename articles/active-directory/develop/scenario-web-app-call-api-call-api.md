---
title: 调用 Web API 的 Web 应用（调用 Web API）- Microsoft 标识平台
description: 了解如何构建调用 Web API 的 Web 应用（调用 Web API）
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
ms.date: 08/26/2019
ms.author: v-junlch
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0fe4e7e7974fc2183ad9772cd7477aa460439e6c
ms.sourcegitcommit: 18a0d2561c8b60819671ca8e4ea8147fe9d41feb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2019
ms.locfileid: "70134245"
---
# <a name="web-app-that-calls-web-apis---call-a-web-api"></a>调用 Web API 的 Web 应用 - 调用 Web API

现在你已有令牌，可以调用受保护的 Web API 了。

## <a name="aspnet-core"></a>ASP.NET Core

以下是 `HomeController` 操作的简化代码。 此代码获取用于调用 Microsoft Graph 的令牌。 这一次添加了代码，显示了如何将 Microsoft Graph 作为 REST API 调用。 图形 API 的 URL 在 `appsettings.json` 文件中提供，并在名为 `webOptions` 的变量中读取：

```JSon
{
  "AzureAd": {
    "Instance": "https://login.partner.microsoftonline.cn/",
    ...
  },
  ...
  "GraphApiUrl": "https://microsoftgraph.chinacloudapi.cn"
}
```

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

 // Calls the web API (here the graph)
 HttpClient httpClient = new HttpClient();
 httpClient.DefaultRequestHeaders.Authorization =
     new AuthenticationHeaderValue(Constants.BearerAuthorizationScheme,accessToken);
 var response = await httpClient.GetAsync($"{webOptions.GraphApiUrl}/beta/me");

 if (response.StatusCode == HttpStatusCode.OK)
 {
  var content = await response.Content.ReadAsStringAsync();

  dynamic me = JsonConvert.DeserializeObject(content);
  return me;
 }

 ViewData["Me"] = me;
 return View();
}
```

> [!NOTE]
> 可以使用相同的原则来调用任何 Web API。
>
> 大多数 Azure Web API 都提供了一个 SDK 来简化调用。 Microsoft Graph 也是如此。 下一篇文章介绍如何找到说明这些方面的教程。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [转移到生产环境](scenario-web-app-call-api-production.md)

<!-- Update_Description: wording update -->