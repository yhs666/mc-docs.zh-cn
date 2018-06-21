---
title: Azure AD v1 ASP.NET Web 服务器入门 | Microsoft Docs
description: 通过基于传统 Web 浏览器的使用 OpenID Connect 标准的应用程序，对 ASP.NET 解决方案实现 Microsoft 登录
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
origin.date: 01/31/2018
ms.date: 01/31/2018
ms.author: v-junlch
ms.openlocfilehash: 7bd2271badadff83b3f361e0ec68f6117eb82ad4
ms.sourcegitcommit: 3629fd4a81f66a7d87a4daa00471042d1f79c8bb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2018
ms.locfileid: "29286105"
---
<!--start-intro-->
# <a name="add-sign-in-with-microsoft-to-an-aspnet-web-app"></a>向 ASP.NET Web 应用添加 Microsoft 登录

本指南演示如何通过基于传统 Web 浏览器的使用 OpenID Connect 的应用程序，使用 ASP.NET MVC 解决方案实现 Microsoft 登录。 

在本指南结束时，应用程序可接受与 Azure Active Directory 集成的组织的工作和学校帐户登录。

> [!NOTE]
> 本设置指南可帮助用户在 ASP.NET 应用程序中实现通过工作和学校帐户登录。 
<br/><br/>

<!--separator-->

> 本指南需要 Visual Studio 2015 Update 3 或 Visual Studio 2017。  尚未安装？  [免费下载 Visual Studio 2017](https://www.visualstudio.com/downloads/)

## <a name="how-this-guide-works"></a>本指南的工作原理

![本指南的工作原理](./media/active-directory-aspnetwebapp-v1/aspnet-intro.png)

本指南基于这样的情景：浏览器访问 ASP.NET 网站，要求用户通过登录按钮进行身份验证。 在此方案中，呈现网页的大部分工作在服务器端完成。

> [!NOTE]
> 本设置指南演示了如何从一个空模板开始，使用户登录 ASP.NET Web 应用程序，包括添加登录按钮以及每个控制器和方法的步骤，同时解释了一些概念。 或者，还可以通过使用 [Visual Studio Web 模板](https://docs.microsoft.com/aspnet/visual-studio/overview/2013/creating-web-projects-in-visual-studio#organizational-account-authentication-options)并选择“组织帐户”和云选项之一（该选项使用包含其他控制器、方法和视图的更丰富的模板），创建使 Azure Active Directory 用户（工作和学校帐户）登录的项目。

## <a name="libraries"></a>库

本指南使用以下包：

|库|说明|
|---|---|
|[Microsoft.Owin.Security.OpenIdConnect](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/)|让应用程序可使用 OpenIdConnect 进行身份验证的中间件|
|[Microsoft.Owin.Security.Cookies](https://www.nuget.org/packages/Microsoft.Owin.Security.Cookies)|让应用程序可使用 Cookie 维持用户会话的中间件|
|[Microsoft.Owin.Host.SystemWeb](https://www.nuget.org/packages/Microsoft.Owin.Host.SystemWeb)|让基于 OWIN 的应用程序可使用 ASP.NET 请求管道在 IIS 上运行|


<!--end-intro-->

<!--start-setup-->

## <a name="set-up-your-project"></a>设置项目

本部分介绍通过 OWIN 中间件在使用 OpenID Connect 的 ASP.NET 项目上安装和配置身份验证管道的步骤。 

> 想要改为下载此示例的 Visual Studio 项目？ [下载项目](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/GuidedSetup.zip)并跳到[配置步骤](#configure-your-webconfig-and-register-an-application)，在执行前配置代码示例。

## <a name="create-your-aspnet-project"></a>创建 ASP.NET 项目
1. 在 Visual Studio 中：`File` > `New` > `Project`<br/>
2. 在 *Visual C#\Web* 下，选择 `ASP.NET Web Application (.NET Framework)`。
3. 命名应用程序，然后单击“确定”
4. 选择 `Empty` 并选中复选框，添加 `MVC` 引用

## <a name="add-authentication-components"></a>添加身份验证组件

1. 在 Visual Studio 中：`Tools` > `Nuget Package Manager` > `Package Manager Console`
2. 在包管理器控制台窗口中键入以下命令，添加 *OWIN 中间件 NuGet 包*：

    ```powershell
    Install-Package Microsoft.Owin.Security.OpenIdConnect
    Install-Package Microsoft.Owin.Security.Cookies
    Install-Package Microsoft.Owin.Host.SystemWeb
    ```

<!--start-collapse-->
> ### <a name="about-these-packages"></a>关于这些包
>上述库通过基于 Cookie 的身份验证使用 OpenID Connect 启用单一登录 (SSO)。 完成身份验证后，代表用户的令牌会发送到应用程序，OWIN 中间件会创建会话 Cookie。 浏览器随后对后续请求使用此 cookie，这样一来，用户就无需重新验证，也不需要任何其他验证。
<!--end-collapse-->

## <a name="configure-the-authentication-pipeline"></a>配置身份验证管道
下面的步骤用于创建 OWIN 中间件 Startup 类，以配置 OpenID Connect 身份验证。 此类自动执行。

> [!TIP]
> 如果项目的根文件夹中没有 `Startup.cs` 文件，请执行以下操作：<br/>
> 1. 右键单击项目的根文件夹：> `Add` > `New Item...` > `OWIN Startup class`<br/>
> 2. 将其命名为 `Startup.cs`<br/>
>
>> 确保选择的类是 OWIN Startup 类，而不是标准 C# 类。 通过检查是否在命名空间上看到 `[assembly: OwinStartup(typeof({NameSpace}.Startup))]` 来进行确认。


1. 将 OWIN 和 Microsoft.IdentityModel 命名空间添加到 `Startup.cs`：

    ```C#
    using Microsoft.Owin;
    using Owin;
    using Microsoft.Owin.Security;
    using Microsoft.Owin.Security.Cookies;
    using Microsoft.Owin.Security.OpenIdConnect;
    using Microsoft.Owin.Security.Notifications;
    using Microsoft.IdentityModel.Protocols;
    using System;
    using System.Threading.Tasks;
    ```

2. 使用以下代码替换 Startup 类：

    ```C#
    public class Startup
    {
        // The Client ID (a.k.a. Application ID) is used by the application to uniquely identify itself to Azure AD
        string clientId = System.Configuration.ConfigurationManager.AppSettings["ClientId"];

        // RedirectUri is the URL where the user will be redirected to after they sign in
        string redirectUrl = System.Configuration.ConfigurationManager.AppSettings["redirectUrl"];

        // Tenant is the tenant ID (e.g. contoso.partner.onmschina.cn, or 'common' for multi-tenant)
        static string tenant = System.Configuration.ConfigurationManager.AppSettings["Tenant"];

        // Authority is the URL for authority, composed by Azure Active Directory endpoint and the tenant name (e.g. https://login.partner.microsoftonline.cn/contoso.partner.onmschina.cn)
        string authority = String.Format(System.Globalization.CultureInfo.InvariantCulture, System.Configuration.ConfigurationManager.AppSettings["Authority"], tenant);

        /// <summary>
        /// Configure OWIN to use OpenIdConnect 
        /// </summary>
        /// <param name="app"></param>
        public void Configuration(IAppBuilder app)
        {
            app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

            app.UseCookieAuthentication(new CookieAuthenticationOptions());
            app.UseOpenIdConnectAuthentication(
                new OpenIdConnectAuthenticationOptions
                {
                    // Sets the ClientId, authority, RedirectUri as obtained from web.config
                    ClientId = clientId,
                    Authority = authority,
                    RedirectUri = redirectUrl,
                    
                    // PostLogoutRedirectUri is the page that users will be redirected to after sign-out. In this case, it is using the home page
                    PostLogoutRedirectUri = redirectUrl,
                    
                    //Scope is the requested scope: OpenIdConnectScopes.OpenIdProfileis equivalent to the string 'openid profile': in the consent screen, this will result in 'Sign you in and read your profile'
                    Scope = OpenIdConnectScopes.OpenIdProfile,
                    
                    // ResponseType is set to request the id_token - which contains basic information about the signed-in user
                    ResponseType = OpenIdConnectResponseTypes.IdToken,
                    
                    // ValidateIssuer set to false to allow work accounts from any organization to sign in to your application
                    // To only allow users from a single organizations, set ValidateIssuer to true and 'tenant' setting in web.config to the tenant name or Id (example: contoso.partner.onmschina.cn)
                    // To allow users from only a list of specific organizations, set ValidateIssuer to true and use ValidIssuers parameter
                    TokenValidationParameters = new System.IdentityModel.Tokens.TokenValidationParameters() { ValidateIssuer = false },

                    // OpenIdConnectAuthenticationNotifications configures OWIN to send notification of failed authentications to OnAuthenticationFailed method
                    Notifications = new OpenIdConnectAuthenticationNotifications
                    {
                        AuthenticationFailed = OnAuthenticationFailed
                    }
                }
            );
        }

        /// <summary>
        /// Handle failed authentication requests by redirecting the user to the home page with an error in the query string
        /// </summary>
        /// <param name="context"></param>
        /// <returns></returns>
        private Task OnAuthenticationFailed(AuthenticationFailedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> context)
        {
            context.HandleResponse();
            context.Response.Redirect("/?errormessage=" + context.Exception.Message);
            return Task.FromResult(0);
        }
    }
    ```
    
<!--start-collapse-->
> [!NOTE]
> 在 *OpenIDConnectAuthenticationOptions* 中提供的参数将充当应用程序与 Azure AD 通信时使用的坐标。 OpenID Connect 中间件会使用 Cookie，因此，还需要设置 Cookie 身份验证，如以上代码所示。 *ValidateIssuer* 值告知 OpenIdConnect 不要限制某个特定组织的访问权限。
<!--end-collapse-->

<!--end-setup-->

<!--start-use-->

## <a name="add-a-controller-to-handle-sign-in-and-sign-out-requests"></a>添加控制器来处理登录和注销请求

此步骤演示如何创建新控制器来公开登录和注销方法。

1.  右键单击 `Controllers` 文件夹，然后选择 `Add` > `Controller`
2.  选择 `MVC (.NET version) Controller - Empty`。
3.  单击“添加”
4.  将其命名为 `HomeController`，然后单击“添加”
5.  向该类添加 OWIN 命名空间：

    ```C#
    using Microsoft.Owin.Security;
    using Microsoft.Owin.Security.Cookies;
    using Microsoft.Owin.Security.OpenIdConnect;
    ```

6. 通过代码启动身份验证质询，添加下面的方法来处理控制器登录和注销：

    ```C#
    /// <summary>
    /// Send an OpenID Connect sign-in request.
    /// Alternatively, you can just decorate the SignIn method with the [Authorize] attribute
    /// </summary>
    public void SignIn()
    {
        if (!Request.IsAuthenticated)
        {
            HttpContext.GetOwinContext().Authentication.Challenge(
                new AuthenticationProperties { RedirectUri = "/" },
                OpenIdConnectAuthenticationDefaults.AuthenticationType);
        }
    }

    /// <summary>
    /// Send an OpenID Connect sign-out request.
    /// </summary>
    public void SignOut()
    {
        HttpContext.GetOwinContext().Authentication.SignOut(
            OpenIdConnectAuthenticationDefaults.AuthenticationType,
            CookieAuthenticationDefaults.AuthenticationType);
    }
    ```
    
## <a name="create-the-apps-home-page-to-sign-in-users-via-a-sign-in-button"></a>创建应用的主页，通过登录按钮来登录用户

在 Visual Studio 中，创建新视图来添加登录按钮并在身份验证后显示用户信息：

1.  右键单击 `Views\Home` 文件夹，然后选择 `Add View`
2.  将它命名为 `Index`。
3.  向文件添加以下 HTML，其中包括登录按钮：

    ```html
    @{
        Layout = null;
    }
    <html>
    <head>
        <meta name="viewport" content="width=device-width" />
        <title>Sign-In with Microsoft Guided Setup (Work Accounts)</title>
    </head>
    <body>
    @if (!Request.IsAuthenticated)
    {
        <!-- If the user is not authenticated, display the sign-in button -->
        <br /><a href="@Url.Action("SignIn", "Home")" style="text-decoration: none;">
            <svg xmlns="http://www.w3.org/2000/svg" xml:space="preserve" width="300px" height="50px" viewBox="0 0 3278 522" class="SignInButton">
                <style type="text/css">.fil0:hover {fill: #4B4B4B;} .fnt0 {font-size: 260px;font-family: 'Segoe UI Semibold', 'Segoe UI'; text-decoration: none;}</style>
                <rect class="fil0" x="2" y="2" width="3174" height="517" fill="black" /><rect x="150" y="129" width="122" height="122" fill="#F35325" /><rect x="284" y="129" width="122" height="122" fill="#81BC06" /><rect x="150" y="263" width="122" height="122" fill="#05A6F0" /><rect x="284" y="263" width="122" height="122" fill="#FFBA08" /><text x="470" y="357" fill="white" class="fnt0">Sign in with Microsoft</text>
            </svg>
        </a>
    }
    else
    {
        <span><br/>Hello @((User.Identity as System.Security.Claims.ClaimsIdentity)?.FindFirst("name")?.Value)</span>
        <br /><br />
        @Html.ActionLink("See Your Claims", "Index", "Claims")
        <br /><br />
        @Html.ActionLink("Sign out", "SignOut", "Home")
    }
    @if (!string.IsNullOrWhiteSpace(Request.QueryString["errormessage"]))
    {
        <div style="background-color:red;color:white;font-weight: bold;">Error: @Request.QueryString["errormessage"]</div>
    }
    </body>
    </html>
    ```

<!--start-collapse-->
> [!NOTE]
> 此页以 SVG 格式添加登录按钮，背景为黑色：<br/>![使用 Microsoft 登录](./media/active-directory-aspnetwebapp-v1/aspnetsigninbuttonsample.png)<br/> 如需多个登录按钮，请转到[此页](/active-directory/develop/active-directory-branding-guidelines)。
<!--end-collapse-->

## <a name="display-users-claims-by-adding-a-controller"></a>添加控制器来显示用户声明
此控制器演示如何使用 `[Authorize]` 属性来保护控制器。 此属性只允许通过身份验证的用户，从而限制对控制器的访问。 下面的代码使用该属性来显示作为登录的一部分被检索的用户声明。

1.  右键单击 `Controllers` 文件夹：`Add` > `Controller`
2.  选择 `MVC {version} Controller - Empty`。
3.  单击“添加”
4.  将其命名为 `ClaimsController`
5.  将控制器类的代码替换为下面的代码，这将 `[Authorize]` 属性添加到类：

    ```c#
    [Authorize]
    public class ClaimsController : Controller
    {
        /// <summary>
        /// Add user's claims to viewbag
        /// </summary>
        /// <returns></returns>
        public ActionResult Index()
        {
            var userClaims = User.Identity as System.Security.Claims.ClaimsIdentity;

            //You get the user’s first and last name below:
            ViewBag.Name = userClaims?.FindFirst("name")?.Value;

            // The 'Name' claim can be used for showing the username
            ViewBag.Username = userClaims?.FindFirst(System.IdentityModel.Claims.ClaimTypes.Name)?.Value;

            // The subject/ NameIdentifier claim can be used to uniquely identify the user across the web
            ViewBag.Subject = userClaims?.FindFirst(System.Security.Claims.ClaimTypes.NameIdentifier)?.Value;

            // TenantId is the unique Tenant Id - which represents an organization in Azure AD
            ViewBag.TenantId = userClaims?.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid")?.Value;

            return View();
        }
    }
    ```

<!--start-collapse-->
> [!NOTE]
> 因为使用 `[Authorize]` 属性，仅当用户通过身份验证后，才能执行此控制器的所有方法。 如果用户未通过身份验证，并尝试访问控制器，OWIN 会启动身份验证质询，并强制用户进行身份验证。 上面的代码查看用户令牌中特定属性的用户的声明集合。 这些属性包括用户的全名和用户名，以及全局用户标识符使用者。 它还包含*租户 ID*，表示用户的组织的 ID。 
<!--end-collapse-->

## <a name="create-a-view-to-display-the-users-claims"></a>创建视图来显示用户的声明

在 Visual Studio 中创建新视图，以在网页上显示用户的声明：

1.  右键单击 `Views\Claims` 文件夹，然后选择 `Add View`
2.  将它命名为 `Index`。
3.  将以下 HTML 添加到文件：

    ```html
    <html>
    <head>
        <meta name="viewport" content="width=device-width" />
        <title>Sign-In with Microsoft Sample</title>
        <link href="@Url.Content("~/Content/bootstrap.min.css")" rel="stylesheet" type="text/css" />
    </head>
    <body style="padding:50px">
    <h3>Main Claims:</h3>
    <table class="table table-striped table-bordered table-hover">
        <tr><td>Name</td><td>@ViewBag.Name</td></tr>
        <tr><td>Username</td><td>@ViewBag.Username</td></tr>
        <tr><td>Subject</td><td>@ViewBag.Subject</td></tr>
        <tr><td>TenantId</td><td>@ViewBag.TenantId</td></tr>
    </table>
    <br />
    <h3>All Claims:</h3>
    <table class="table table-striped table-bordered table-hover table-condensed">
        @foreach (var claim in ((System.Security.Claims.ClaimsIdentity) User.Identity).Claims)
        {
            <tr><td>@claim.Type</td><td>@claim.Value</td></tr>
        }
    </table>
    <br />
    <br />
    @Html.ActionLink("Sign out", "SignOut", "Home", null, new { @class = "btn btn-primary" })
    </body>
    </html>
    ```
    
<!--end-use-->

<!--start-configure-->
## <a name="configure-your-webconfig-and-register-an-application"></a>配置 web.config 并注册应用程序

1. 在 Visual Studio 中，将以下内容添加到 `configuration\appSettings` 部分下的 `web.config`（位于根文件夹中）：

    ```xml
    <add key="ClientId" value="Enter_the_Application_Id_here" />
    <add key="RedirectUrl" value="Enter_the_Redirect_Url_here" />
    <add key="Tenant" value="common" />
    <add key="Authority" value="https://login.partner.microsoftonline.cn/{0}" /> 
    ```
2. 在解决方案资源管理器中，选择项目并查看“属性”<i></i> 窗口（如果看不到“属性”窗口，请按 F4）
3. 将“已启用 SSL”更改为 <code>True</code>
4. 将项目的 SSL URL 复制到剪贴板：<br/><br/>![项目属性](./media/active-directory-aspnetwebapp-v1/visual-studio-project-properties.png)<br />
5. 在 <code>web.config</code> 中，用项目的 SSL URL替换 <code>Enter_the_Redirect_URL_here</code> 

### <a name="register-your-application-in-the-azure-portal-then-add-its-information-to-webconfig"></a>在 Azure 门户中注册应用程序，然后将其信息添加到 web.config

1. 转到 [Azure 门户 - 应用注册](https://portal.azure.cn/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps)，注册应用程序
2. 选择 `New application registration`
3. 输入应用程序的名称
4. 在`Sign-on URL` 中粘贴 Visual Studio 项目的 SSL URL（此 URL 还会自动添加到正在注册的应用程序的回复 URL 列表）
5. 单击 `Create` 注册应用程序。 执行此操作后会返回到应用程序列表
6. 现在，搜索并/或选择刚刚创建的应用程序，打开其属性
7. 将 `Application ID` 下的 guid 复制到剪贴板
8. 返回到 Visual Studio，在 `web.config` 中，用刚刚注册的应用程序 ID 替换 `Enter_the_Application_Id_here`

> [!TIP]
> 如果帐户配置为可访问多个目录，请确保为要向其注册应用程序的组织选择了正确的目录，方法是单击 Azure 门户右上角的帐户名称，然后按照指示验证所选目录：<br/>![选择正确的目录](./media/active-directory-aspnetwebapp-v1/tenantselector.png)

## <a name="configure-sign-in-options"></a>配置登录选项

可以将应用程序配置为只允许某个组织的 Azure Active Directory 实例中的用户登录，或者接受任何组织中的用户登录。 请按照以下选项之一的说明进行操作：

### <a name="configure-your-application-to-allow-sign-ins-of-work-and-school-accounts-from-any-company-or-organization-multi-tenant"></a>将应用程序配置为允许任何公司或组织（多租户）的工作和学校帐户登录

如果想接受任何已经与 Azure Active Directory 集成的公司或组织的工作和学校帐户登录，请执行以下步骤。 这是 SaaS 应用程序 的常见方案：

1. 返回到 [Azure 门户 - 应用注册](https://portal.azure.cn/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps)，查找刚注册的应用程序
2. 在 `All Settings` 下选择 `Properties`
3. 将 `Multi-tenanted` 属性更改为 `Yes`，并单击 `Save`

有关此设置和多租户应用程序概念的详细信息，请参阅[此文](../active-directory-devhowto-multi-tenant-overview.md)。

### <a name="restrict-users-from-only-one-organizations-active-directory-instance-to-sign-in-to-your-application-single-tenant"></a>限制某个组织的 Active Directory 实例的用户登录应用程序（单租户）

此选项是 LOB 应用程序的常见方案：如果希望应用程序仅接受属于特定 Azure Active Directory 实例（包括该实例的来宾帐户）的用户登录，请使用组织的租户名称（例如 contoso.partner.onmschina.cn） 替换 `Common` 的 web.config 中的 `Tenant` 参数。 之后将 [OWIN 启动类](#configure-the-authentication-pipeline) 中的 `ValidateIssuer` 参数更改为 `true`。

若要允许来自特定组织列表的用户，将 `ValidateIssuer` 设置为 true，并使用 `ValidIssuers` 参数来指定组织的列表。

还可通过 IssuerValidator 参数实现自定义方法来验证颁发者。 有关 `TokenValidationParameters` 的详细信息，请参阅[此 MSDN 文章](https://msdn.microsoft.com/library/system.identitymodel.tokens.tokenvalidationparameters.aspx)。

<!--end-configure-->

<!--start-configure-arp-->
<!--
## Configure your ASP.NET Web App with the application's registration information

In this step, you will configure your project to use SSL, and then use the SSL URL to configure your application’s registration information. After this, add the application’ registration information to your solution via *web.config*.

1.  In Solution Explorer, select the project and look at the `Properties` window (if you don’t see a Properties window, press F4)
2.  Change `SSL Enabled` to `True`
3.  Copy the value from `SSL URL` above and paste it in the `Redirect URL` field on the top of this page, then click *Update*:<br/><br/>![Project properties](./media/active-directory-aspnetwebapp-v1/vsprojectproperties.png)<br />
4.  Add the following in `web.config` file located in root’s folder, under section `configuration\appSettings`:

```xml
<add key="ClientId" value="[Enter the application Id here]" />
<add key="RedirectUri" value="[Enter the Redirect URL here]" />
<add key="Tenant" value="common" />
<add key="Authority" value="https://login.partner.microsoftonline.cn/{0}" /> 
```
-->
<!--end-configure-arp-->
<!--start-test-->
## <a name="test-your-code"></a>测试代码

按 `F5` 在 Visual Studio 中运行项目。 浏览器随即打开，并定向到 http://localhost:{port}，可在其中看到“使用 Microsoft 登录”按钮。 继续操作并单击它以登录。

准备好测试后，请使用工作帐户 (Azure Active Directory) 登录。 

![使用 Microsoft 浏览器窗口登录](./media/active-directory-aspnetwebapp-v1/aspnetbrowsersignin.png)

![使用 Microsoft 浏览器窗口登录](./media/active-directory-aspnetwebapp-v1/aspnetbrowsersignin2.png)

#### <a name="expected-results"></a>预期结果
登录后，用户会重定向到网站主页，该网站是 Microsoft 应用程序注册门户上应用程序注册信息中指定的 HTTPS URL。 此页现在显示“Hello {用户}”、注销链接，以及查看用户声明的链接（即指向之前创建的 Authorize 控制器的链接）。

### <a name="see-users-claims"></a>查看用户的声明
选择超链接，查看用户的声明。 此操作将用户引至控制器和视图，仅供通过身份验证的用户访问。

#### <a name="expected-results"></a>预期结果
 此时应会显示一个表，其中包含已登录用户的基本属性：

| 属性 | 值 | 说明|
|---|---|---|
| Name | {用户全名} | 用户的名字和姓氏
|用户名 | <span>user@domain.com</span>| 用于标识已登录用户的用户名
| 使用者| {使用者}|一个唯一标识 Web 上用户登录名的字符串|
| 租户 ID| {Guid}| *guid* 唯一表示用户的 Azure Active Directory 组织。|

此外还可看到一个表格，其中包含身份验证请求中的所有用户声明。 有关 ID 令牌中所有声明的列表及其说明，请参阅[此文](/active-directory/develop/active-directory-token-and-claims)。


### <a name="test-accessing-a-method-that-has-an-authorize-attribute-optional"></a>对访问具有 *[Authorize]* 属性的方法进行测试（可选）
此步骤测试作为匿名用户对 Claims 控制器的访问：<br/>
选择注销用户的链接并完成注销过程。<br/>
现在在浏览器中键入 http://localhost:{port}/claims，访问受 `[Authorize]` 属性保护的控制器

#### <a name="expected-results"></a>预期结果
应会出现提示，要求进行身份验证以查看视图。

## <a name="additional-information"></a>其他信息

<!--start-collapse-->
### <a name="protect-your-entire-web-site"></a>保护整个网站
若要保护整个网站，请在 `Global.asax` `Application_Start` 方法中将 `AuthorizeAttribute` 添加到 `GlobalFilters`：

```csharp
GlobalFilters.Filters.Add(new AuthorizeAttribute());
```
<!--end-collapse-->

<div></div>
<br/>

<!--end-test-->


