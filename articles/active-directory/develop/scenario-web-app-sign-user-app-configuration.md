---
title: 用于登录用户的 Web 应用（代码配置）- Microsoft 标识平台
description: 了解如何构建用于登录用户的 Web 应用（代码配置）
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
origin.date: 10/30/2019
ms.date: 11/06/2019
ms.author: v-junlch
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9408b489469eeb762e617c37d59f04138dc2a5e4
ms.sourcegitcommit: a88cc623ed0f37731cb7cd378febf3de57cf5b45
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/08/2019
ms.locfileid: "73830915"
---
# <a name="web-app-that-signs-in-users---code-configuration"></a>用于登录用户的 Web 应用 - 代码配置

了解如何为用于登录用户的 Web 应用配置代码。

## <a name="libraries-used-to-protect-web-apps"></a>库用于保护 Web 应用

<!-- This section can be in an include for Web App and Web APIs -->
用于保护 Web 应用（和 Web API）的库为：

| 平台 | 库 | 说明 |
|----------|---------|-------------|
| ![.NET](./media/sample-v2-code/logo_net.png) | [适用于 .NET 的标识模型扩展](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet/wiki) | 在由 ASP.NET 和 ASP.NET Core 直接使用的情况下，适用于 .NET 的 Microsoft 标识扩展提议了一组在 .NET Framework 和 .NET Core 上运行的 DLL。 在 ASP.NET/ASP.NET Core Web 应用中，可以使用 **TokenValidationParameters** 类控制令牌验证（尤其适用于某些 ISV 方案） |
| ![Java](./media/sample-v2-code/small_logo_java.png) | [MSAL Java](https://github.com/AzureAD/microsoft-authentication-library-for-java/wiki) | MSAL for Java - 目前为公共预览版 |
| ![Python](./media/sample-v2-code/small_logo_python.png) | [MSAL Python](https://github.com/AzureAD/microsoft-authentication-library-for-python/wiki) | MSAL for Python - 目前为公共预览版 |

选择与所需平台对应的选项卡：

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

本文和以下内容中的代码片段摘自 [ASP.NET Core Web 应用增量教程第 1 章](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-1-MyOrg)。

你可能需要参考该教程来了解完整的实现细节。

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

本文中的代码片段及以下内容摘自 [ASP.NET Web 应用示例](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect)

你可能需要参考此示例来了解完整的实现细节。

# <a name="javatabjava"></a>[Java](#tab/java)

本文中的代码片段及以下内容摘自[调用 Microsoft Graph 的 Java Web 应用程序](https://github.com/Azure-Samples/ms-identity-java-webapp) MSAL Java Web 应用示例

你可能需要参考此示例来了解完整的实现细节。

# <a name="pythontabpython"></a>[Python](#tab/python)

本文中的代码片段及以下内容摘自[调用 Microsoft Graph 的 Python Web 应用程序](https://github.com/Azure-Samples/ms-identity-python-webapp) MSAL.Python Web 应用示例

你可能需要参考此示例来了解完整的实现细节。

---

## <a name="configuration-files"></a>配置文件

使用 Microsoft 标识平台将用户登录的 Web 应用程序通常是通过配置文件配置的。 需填充的设置为：

- 云 `Instance`（如果需要运行应用，例如，在国家云中运行）
- `tenantId` 中的受众
- 应用程序的 `clientId`，从 Azure 门户中复制。

有时，可以通过 `authority`（`instance` 与 `tenantId` 的串联）将应用程序参数化

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

在 ASP.NET Core 中，这些设置位于 [appsettings.json](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/bc564d68179c36546770bf4d6264ce72009bc65a/1-WebApp-OIDC/1-1-MyOrg/appsettings.json#L2-L8) 文件的“AzureAD”节中。

```Json
{
  "AzureAd": {
    // Azure Cloud instance among:
    // "https://login.chinacloudapi.cn/" for Azure AD China operated by 21Vianet
    "Instance": "https://login.partner.microsoftonline.cn/",

    // Azure AD Audience among:
    // - the tenant Id as a GUID obtained from the azure portal to sign-in users in your organization
    // - "organizations" to sign-in users in any work or school accounts
    // - "common" to sign-in users with any work and school account
    "TenantId": "[Enter the tenantId here]]",

    // Client Id (Application ID) obtained from the Azure portal
    "ClientId": "[Enter the Client Id]",
    "CallbackPath": "/signin-oidc",
    "SignedOutCallbackPath ": "/signout-callback-oidc"
  }
}
```

在 ASP.NET Core 中，有另一个文件 [properties\launchSettings.json](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/bc564d68179c36546770bf4d6264ce72009bc65a/1-WebApp-OIDC/1-1-MyOrg/Properties/launchSettings.json#L6-L7) 包含应用程序的 URL (`applicationUrl`) 和 SSL 端口 (`sslPort`)，此外还有各种配置文件。

```Json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iisExpress": {
      "applicationUrl": "http://localhost:3110/",
      "sslPort": 44321
    }
  },
  "profiles": {
    "IIS Express": {
      "commandName": "IISExpress",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    },
    "webApp": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      },
      "applicationUrl": "http://localhost:3110/"
    }
  }
}
```

在 Azure 门户中，需要在“身份验证”页中为应用程序注册的回复 URI  必须与这些 URL 匹配；也就是说，对于上面的两个配置文件，它们需要是 `https://localhost:44321/signin-oidc`（因为 applicationUrl 是 `http://localhost:3110`），但 `sslPort` 已指定 (44321)，且 `CallbackPath` 为 `/signin-oidc`（在 `appsettings.json` 中定义）。

注销 URI 将采用相同方式设置为 `https://localhost:44321/signout-callback-oidc`。

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

在 ASP.NET 中，应用程序通过 [Web.Config](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect/blob/a2da310539aa613b77da1f9e1c17585311ab22b7/WebApp/Web.config#L12-L15) 文件的第 12-15 行进行配置

```XML
<?xml version="1.0" encoding="utf-8"?>
<!--
  For more information on how to configure your ASP.NET application, please visit
  https://go.microsoft.com/fwlink/?LinkId=301880
  -->
<configuration>
  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:ClientId" value="[Enter your client ID, as obtained from the app registration portal]" />
    <add key="ida:ClientSecret" value="[Enter your client secret, as obtained from the app registration portal]" />
    <add key="ida:AADInstance" value="https://login.partner.microsoftonline.cn/{0}{1}" />
    <add key="ida:RedirectUri" value="https://localhost:44326/" />
    <add key="vs:EnableBrowserLink" value="false" />
  </appSettings>
```

在 Azure 门户中，需要在应用程序的“身份验证”页中注册的回复 URI（即 `https://localhost:44326/`）需与这些 URL 相匹配。 

# <a name="javatabjava"></a>[Java](#tab/java)

在 Java 中，配置位于 [application.properties](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/d55ee4ac0ce2c43378f2c99fd6e6856d41bdf144/src/main/resources/application.properties) 文件中的 `src/main/resources` 下

```Java
aad.clientId=Enter_the_Application_Id_here
aad.authority=https://login.partner.microsoftonline.cn/Enter_the_Tenant_Info_Here/
aad.secretKey=Enter_the_Client_Secret_Here
aad.redirectUriSignin=http://localhost:8080/msal4jsample/secure/aad
aad.redirectUriGraph=http://localhost:8080/msal4jsample/graph/me
```

在 Azure 门户中，需要在应用程序的“身份验证”页中注册的回复 URL（即 `http://localhost:8080/msal4jsample/secure/aad` 和 `http://localhost:8080/msal4jsample/graph/me`）需与应用程序定义的 redirectUris 相匹配。 

# <a name="pythontabpython"></a>[Python](#tab/python)

下面是 [app_config.py](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/0.1.0/app_config.py) 中的 Python 配置文件。

```Python
CLIENT_SECRET = "Enter_the_Client_Secret_Here"
AUTHORITY = "https://login.partner.microsoftonline.cn/common""
CLIENT_ID = "Enter_the_Application_Id_here"
ENDPOINT = 'https://microsoftgraph.chinacloudapi.cn/v1.0/users'
SCOPE = ["User.ReadBasic.All"]
SESSION_TYPE = "filesystem"  # So token cache will be stored in server-side session
```

> [!NOTE]
> 本快速入门建议在配置文件中存储客户端机密，以方便操作。
> 在生产应用中，建议使用其他方式来存储机密，例如，使用 KeyVault，或者根据 Flask 文档中所述使用环境变量： https://flask.palletsprojects.com/en/1.1.x/config/#configuring-from-environment-variables
>
> ```python
> CLIENT_SECRET = os.getenv("CLIENT_SECRET")
> if not CLIENT_SECRET:
>     raise ValueError("Need to define CLIENT_SECRET environment variable")
> ```

---

## <a name="initialization-code"></a>初始化代码

初始化代码因平台而异。 对于 ASP.NET Core 和 ASP.NET，用户登录将委托给 OpenIDConnect 中间件来完成。 目前，ASP.NET/ASP.NET Core 模板可为 Azure AD v1.0 终结点生成 Web 应用程序。 因此，只需进行少许配置就能使这些应用程序适应 Microsoft 标识平台 (v2.0) 终结点。 对于 Java，初始化由 Spring 在应用程序的配合下进行处理。

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

在 ASP.NET Core Web 应用（和 Web API）中，应用程序受到保护，因为控制器或控制器操作中包含 `[Authorize]` 属性。 此属性检查是否已对用户进行身份验证。 执行应用程序初始化的代码位于 `Startup.cs` 文件中。若要使用 Microsoft 标识平台（以前称为 Azure AD v2.0）添加身份验证，需添加以下代码。 代码中的注释应该浅显易懂。

  > [!NOTE]
  > 如果通过 Visual Studio 或 `dotnet new mvc` 使用默认的 ASP.NET Core Web 项目来启动项目，则可默认使用 `AddAzureAD` 方法，因为相关的包会自动加载。
  > 但是，如果从头生成一个项目并尝试使用以下代码，则建议将 NuGet 包“Microsoft.AspNetCore.Authentication.AzureAD.UI”  添加到项目，使 `AddAzureAD` 方法可用。

以下代码摘自 [Startup.cs#L33-L34](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/faa94fd49c2da46b22d6694c4f5c5895795af26d/1-WebApp-OIDC/1-1-MyOrg/Startup.cs#L33-L34)

```CSharp
public class Startup
{
 ...

  // This method gets called by the runtime. Use this method to add services to the container.
  public void ConfigureServices(IServiceCollection services)
  {
    ...
      // Sign-in users with the Microsoft identity platform
      services.AddMicrosoftIdentityPlatformAuthentication(Configuration);

      services.AddMvc(options =>
      {
          var policy = new AuthorizationPolicyBuilder()
              .RequireAuthenticatedUser()
              .Build();
            options.Filters.Add(new AuthorizeFilter(policy));
            })
        .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
    }
```

`AddMicrosoftIdentityPlatformAuthentication` 是 [Microsoft.Identity.Web/WebAppServiceCollectionExtensions.cs#L23](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/faa94fd49c2da46b22d6694c4f5c5895795af26d/Microsoft.Identity.Web/WebAppServiceCollectionExtensions.cs#L23) 中定义的扩展方法。 该方法：

- 添加身份验证服务
- 配置用于读取配置文件的选项
- 配置 OpenID Connect 选项，使所用的颁发机构是 Microsoft 标识平台（以前称为 Azure AD v2.0）终结点
- 验证令牌的颁发者
- 从 ID 令牌中的“preferred_username”声明映射对应于名称的声明

除配置以外，还可以在调用 `AddMicrosoftIdentityPlatformAuthentication` 时指定：

- 配置节的名称（默认为 AzureAD）
- 若要跟踪 OpenIdConnect 中间件事件，以便在身份验证不起作用时帮助排查 Web 应用程序的问题：将 `subscribeToOpenIdConnectMiddlewareDiagnosticsEvents` 设置为 `true`，可以显示在 ASP.NET Core 中间件集在处理 HTTP 响应后处理 `HttpContext.User`中的用户标识过程中如何阐述详细的信息。

```CSharp
/// <summary>
/// Add authentication with Microsoft identity platform.
/// This method expects the configuration file will have a section named "AzureAd" with the necessary settings to initialize authentication options.
/// </summary>
/// <param name="services">Service collection to which to add this authentication scheme</param>
/// <param name="configuration">The Configuration object</param>
/// <param name="subscribeToOpenIdConnectMiddlewareDiagnosticsEvents">
/// Set to true if you want to debug, or just understand the OpenIdConnect events.
/// </param>
/// <returns></returns>
public static IServiceCollection AddMicrosoftIdentityPlatformAuthentication(
  this IServiceCollection services,
  IConfiguration configuration,
  string configSectionName = "AzureAd",
  bool subscribeToOpenIdConnectMiddlewareDiagnosticsEvents = false)
{
  services.AddAuthentication(AzureADDefaults.AuthenticationScheme)
      .AddAzureAD(options => configuration.Bind(configSectionName, options));
  services.Configure<AzureADOptions>(options => configuration.Bind(configSectionName, options));

  services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options =>
  {
      // Per the code below, this application signs in users in any Work and School accounts.
      // If you want to direct Azure AD to restrict the users that can sign-in, change
      // the tenant value of the appsettings.json file in the following way:
      // - only Work and School accounts => 'organizations'
      // - Work and School => 'common'
      // If you want to restrict the users that can sign-in to only one tenant
      // set the tenant value in the appsettings.json file to the tenant ID
      // or domain of this organization
      options.Authority = options.Authority + "/v2.0/";

      // If you want to restrict the users that can sign-in to several organizations
      // Set the tenant value in the appsettings.json file to 'organizations', and add the
      // issuers you want to accept to options.TokenValidationParameters.ValidIssuers collection
      options.TokenValidationParameters.IssuerValidator = AadIssuerValidator.GetIssuerValidator(options.Authority).Validate;

      // Set the nameClaimType to be preferred_username.
      // This change is needed because certain token claims from Azure AD V1 endpoint
      // (on which the original .NET core template is based) are different than Microsoft identity platform endpoint.
      // For more details see [ID Tokens](/active-directory/develop/id-tokens)
      // and [Access Tokens](/active-directory/develop/access-tokens)
      options.TokenValidationParameters.NameClaimType = "preferred_username";

      // ...

      if (subscribeToOpenIdConnectMiddlewareDiagnosticsEvents)
      {
          OpenIdConnectMiddlewareDiagnostics.Subscribe(options.Events);
      }
  });
  return services;
}
  ...
```

在许多情况下，可以使用 `AadIssuerValidator` 类验证令牌的颁发者（Azure 云中的 v1.0 或 v2.0 令牌、单租户或多租户应用程序）。 [Microsoft.Identity.Web/Resource/AadIssuerValidator.cs](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/master/Microsoft.Identity.Web/Resource/AadIssuerValidator.cs) 中提供了此类

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

与 ASP.NET Web 应用/Web API 中的身份验证相关的代码位于 [App_Start/Startup.Auth.cs](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect/blob/a2da310539aa613b77da1f9e1c17585311ab22b7/WebApp/App_Start/Startup.Auth.cs#L17-L61) 文件中。

```CSharp
 public void ConfigureAuth(IAppBuilder app)
 {
  app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

  app.UseCookieAuthentication(new CookieAuthenticationOptions());

  app.UseOpenIdConnectAuthentication(
    new OpenIdConnectAuthenticationOptions
    {
     // The `Authority` represents the identity platform endpoint - https://login.partner.microsoftonline.cn/common/v2.0
     // The `Scope` describes the initial permissions that your app will need.
     //  See https://docs.azure.cn/zh-cn/active-directory/develop/v2-permissions-and-consent
     ClientId = clientId,
     Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, "common", "/v2.0"),
     RedirectUri = redirectUri,
     Scope = "openid profile",
     PostLogoutRedirectUri = redirectUri,
    });
 }
```

# <a name="javatabjava"></a>[Java](#tab/java)

该 Java 示例使用 Spring 框架。 应用程序受到保护，因为已实现一个用于截获每个 HTTP 响应的 `Filter`。 在 Java Web 应用快速入门中，此筛选器是 `src/main/java/com/microsoft/azure/msalwebsample/AuthFilter.java`中的 `AuthFilter`。 该筛选器处理 OAuth 2.0 授权代码流，因此：

- 验证是否已对用户进行身份验证（`isAuthenticated()` 方法）
- 如果用户未进行身份验证，该筛选器将计算 Azure AD 授权终结点的 URL，并将浏览器重定向到此 URI
- 包含身份验证代码的响应抵达时，它将使用 MSAL Java 获取令牌。
- 当它最终从令牌终结点收到令牌时（在重定向 URI 上），用户已登录。

有关详细信息，请参阅 [AuthFilter.java](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/master/src/main/java/com/microsoft/azure/msalwebsample/AuthFilter.java) 中的 `doFilter()` 方法

> [!NOTE]
> `doFilter()` 的代码以略微不同的顺序编写，但流与上述相同。

有关此方法触发的授权代码流的详细信息，请参阅 [Microsoft 标识平台和 OAuth 2.0 授权代码流](v2-oauth2-auth-code-flow.md)

# <a name="pythontabpython"></a>[Python](#tab/python)

该 Python 示例使用 Flask。 Flask 初始化和 MSAL.Python 在 [app.py#L1-L28](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/e03be352914bfbd58be0d4170eba1fb7a4951d84/app.py#L1-L28) 中完成

```Python
import uuid
import requests
from flask import Flask, render_template, session, request, redirect, url_for
from flask_session import Session  # https://pythonhosted.org/Flask-Session
import msal
import app_config


app = Flask(__name__)
app.config.from_object(app_config)
Session(app)
```

---

## <a name="next-steps"></a>后续步骤

下一篇文章将会介绍如何触发登录和注销。

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

> [!div class="nextstepaction"]
> [登录和注销](/active-directory/develop/scenario-web-app-sign-user-sign-in?tabs=aspnetcore)

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

> [!div class="nextstepaction"]
> [登录和注销](/active-directory/develop/scenario-web-app-sign-user-sign-in?tabs=aspnet)

# <a name="javatabjava"></a>[Java](#tab/java)

> [!div class="nextstepaction"]
> [登录和注销](/active-directory/develop/scenario-web-app-sign-user-sign-in?tabs=java)

# <a name="pythontabpython"></a>[Python](#tab/python)

> [!div class="nextstepaction"]
> [登录和注销](/active-directory/develop/scenario-web-app-sign-user-sign-in?tabs=python)

---

