---
title: 受保护的 Web API - 应用代码配置 | Azure
description: 了解如何生成受保护的 Web API 和配置应用程序的代码。
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
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
ms.openlocfilehash: e6a222cffc91866bb7b77c8cf1ea1dd210e65361
ms.sourcegitcommit: 9d5fd3184b6a47bf3b60ffdeeee22a08354ca6b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2019
ms.locfileid: "67305916"
---
# <a name="protected-web-api---code-configuration"></a>受保护的 Web API - 代码配置

若要成功配置受保护 Web API 的代码，需要了解 API 如何受到保护、如何配置持有者令牌，以及如何验证令牌。

## <a name="what-makes-aspnetaspnet-core-apis-protected"></a>ASP.NET/ASP.NET Core API 如何受到保护？

与在 Web 应用中一样，ASP.NET/ASP.NET Core Web API 也“受到保护”，因为它们的控制器操作以 `[Authorize]` 属性为前缀。 因此，仅当使用已授权标识调用 API 时，才能调用控制器操作。

考虑以下问题：

- Web API 如何知道调用它的应用程序的标识（只有应用程序才能调用 Web API）？
- 如果应用程序代表某个用户调用 Web API，该用户的标识是什么？

## <a name="bearer-token"></a>持有者令牌

有关应用程序标识以及有关用户标识（除非 Web 应用程序接受守护程序应用程序发出的服务到服务调用）的信息保存于调用应用程序时在标头中设置的持有者令牌内。

以下 C# 代码示例演示了在使用 MSAL.NET 获取令牌后调用 API 的客户端。

```CSharp
var scopes = new[] {$"api://.../access_as_user}";
var result = await app.AcquireToken(scopes)
                      .ExecuteAsync();

httpClient = new HttpClient();
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

// Call the web API.
HttpResponseMessage response = await _httpClient.GetAsync(apiUri);
```

> [!IMPORTANT]
> 持有者令牌是客户端应用程序向 **Web API 的** Microsoft 标识平台终结点请求的。 Web API 是唯一应该验证令牌并查看其中包含的声明的应用程序。 客户端应用程序永远不应查看令牌中的声明（Web API 将来可以决定需要将令牌加密，这可以防止客户端应用程序破解访问令牌）。

## <a name="jwtbearer-configuration"></a>JwtBearer 配置

本部分介绍如何配置持有者令牌。

### <a name="config-file"></a>配置文件

```Json
{
  "AzureAd": {
    "Instance": "https://login.partner.microsoftonline.cn/",
    "ClientId": "[Client_id-of-web-api-eg-2ec40e65-ba09-4853-bcde-bcb60029e596]",
    /*
      You need specify the TenantId only if you want to accept access tokens from a single tenant
     (line of business app)
      Otherwise you can leave them set to common
      This can be:
      - A guid (Tenant ID = Directory ID)
      - 'common' (Any organization)
      - 'organizations' (any organization)
    */
    "TenantId": "common"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Warning"
    }
  },
  "AllowedHosts": "*"
}
```

### <a name="code-initialization"></a>代码初始化

在包含 `[Authorize]` 属性的控制器操作中调用应用程序时，ASP.NET/ASP.NET Core 会查找调用请求的 Authorization 标头中的持有者令牌，然后将该令牌转发到 JwtBearer 中间件，而该中间件将调用适用于 .NET 的 Microsoft 标识模型扩展。

在 ASP.NET Core 中，该中间件在 `Startup.cs` 文件中初始化。

```CSharp
using Microsoft.AspNetCore.Authentication.JwtBearer;
```

该中间件由以下指令添加到 Web API：

```CSharp
 services.AddAzureAdBearer(options => Configuration.Bind("AzureAd", options));
```

 目前，ASP.NET Core 模板会创建 Azure AD v1.0 Web API。 但是，你可以通过在 `Startup.cs` 文件中添加以下代码，轻松将这些模板更改为使用 Microsoft 标识平台终结点。

```CSharp
services.Configure<JwtBearerOptions>(AzureADDefaults.JwtBearerAuthenticationScheme, options =>
{
    // This is a Microsoft identity platform v2.0 web API
    options.Authority += "/v2.0";

    // The web API accepts as audiences are both the Client ID (options.Audience) and api://{ClientID}
    options.TokenValidationParameters.ValidAudiences = new []
    {
     options.Audience,
     $"api://{options.Audience}"
    };

    // Instead of using the default validation (validating against a single tenant,
    // as we do in line of business apps),
    // we inject our own multi-tenant validation logic (which even accepts both V1 and V2 tokens)
    options.TokenValidationParameters.IssuerValidator = AadIssuerValidator.ValidateAadIssuer;
});
```

## <a name="token-validation"></a>令牌验证

与 Web 应用中的 OpenID Connect 中间件一样，JwtBearer 中间件将由 `TokenValidationParameters` 定向以验证令牌。 将解密该令牌（如果需要），提取声明，并验证签名。 然后，通过检查以下数据来验证该令牌：

- 它面向 Web API（受众）
- 它是针对可以调用 Web API 的应用程序颁发的（使用者）
- 它是由可信的安全令牌服务器 (STS) 颁发的（颁发者）
- 令牌的生存期在有效范围内（过期时间）
- 它未被篡改（签名）

此外还可以执行特殊验证。 例如，可以验证（嵌入在令牌中的）签名密钥是否受信任，以及令牌是否未重放。 最后，某些协议需要特定的验证。

### <a name="validators"></a>验证程序

验证步骤将捕获到验证程序中，[适用于 .NET 的 Microsoft 标识模型扩展](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet)开源库在一个源文件中包含了所有这些验证程序：[Microsoft.IdentityModel.Tokens/Validators.cs](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet/blob/master/src/Microsoft.IdentityModel.Tokens/Validators.cs)

下表描述了验证程序：

| 验证程序 | 说明 |
|---------|---------|
| `ValidateAudience` | 确保该令牌适用于对它进行验证的应用程序（供我使用）。 |
| `ValidateIssuer` | 确保令牌由受信任的 STS 颁发（来自我信任的一方）。 |
| `ValidateIssuerSigningKey` | 确保验证令牌的应用程序信任用于签署令牌的密钥（适用于密钥嵌入在令牌中的特殊情况，通常不需要满足此要求）。 |
| `ValidateLifetime` | 确保令牌仍然（或已经）生效。 为此，可以检查令牌生存期（`notbefore`、`expires` 声明）是否在有效范围内。 |
| `ValidateSignature` | 确保令牌未被篡改。 |
| `ValidateTokenReplay` | 确保令牌未重放（适用于某些一次性协议的特殊情况）。 |

所有验证程序与 `TokenValidationParameters` 类的属性相关联，其本身已从 ASP.NET/ASP.NET Core 配置初始化。 在大多数情况下，无需更改参数。 非单租户应用程序除外（此类 Web 应用接受任何组织的用户）-- 在这种情况下，必须验证颁发者。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [转移到生产环境](scenario-protected-web-api-production.md)

