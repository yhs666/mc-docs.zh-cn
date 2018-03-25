---
title: Azure AD Windows Phone 入门 | Microsoft Docs
description: 如何生成一个与 Azure AD 集成以方便登录，并使用 OAuth 调用 Azure AD 保护 API 的 Windows Phone 应用程序。
services: active-directory
documentationcenter: windows
author: jmprieur
manager: mtillman
editor: ''
ms.assetid: 66f5ac20-5e1f-4b9d-bb99-9b3305e26416
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: article
origin.date: 11/30/2017
ms.date: 01/17/2018
ms.author: v-junlch
ms.custom: aaddev
ms.openlocfilehash: cab41a28687a95936b995ddc1e54bd095f83b5be
ms.sourcegitcommit: ba39acbdf4f7c9829d1b0595f4f7abbedaa7de7d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2018
---
# <a name="azure-ad-windows-phone-getting-started"></a>Azure AD Windows Phone 入门
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

> [!NOTE]
> Visual Studio 2017 中不支持 Windows Phone 8.1 和更低版本的项目。  有关详细信息，请参阅 [Visual Studio 2017 平台目标以及兼容性](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs)。

如果要开发 Windows Phone 8.1 应用，Azure AD 可让你简单直接地使用用户的 Active Directory 帐户对其进行身份验证。  它还可以让应用程序安全地使用 Azure AD 保护的任何 Web API，例如 Office 365 API 或 Azure API。

> [!NOTE]
> 此代码示例使用 ADAL v2.0。  若要体验最新技术，可以改为尝试[使用 ADAL v3.0 的 Windows 通用教程](active-directory-devquickstarts-windowsstore.md)。  如果确实要构建适用于 Windows Phone 8.1 的应用，本文正是理想之选。  ADAL v2.0 仍受到完全支持，并且是使用 Azure AD 来针对 Windows Phone 8.1 开发应用的建议方式。
> 
> 

对于需要访问受保护资源的 .NET 本机客户端，Azure AD 提供 Active Directory 身份验证库 (ADAL)。  在本质上，ADAL 的唯一用途就是方便应用程序获取访问令牌。  为了演示操作的简单性，下面我们要生成一个“目录搜索器”Windows Phone 8.1 应用，该应用可以：

- 使用 [OAuth 2.0 身份验证协议](/active-directory/develop/active-directory-protocols-oauth-code)获取调用 Azure AD 图形 API 的访问令牌。
- 在目录中搜索具有给定 UPN 的用户。
- 将用户注销。

要生成完整的工作应用程序，需要：

1. 将应用程序注册到 Azure AD。
2. 安装并配置 ADAL。
3. 使用 ADAL 从 Azure AD 获取令牌。

若要开始，请[下载框架项目](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/skeleton.zip)或[下载已完的成示例](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip)。  每个下载项目都是 Visual Studio 2013 解决方案。  还需要一个可在其中创建用户和注册应用程序的 Azure AD 租户。  如果没有租户，请[了解如何获取租户](active-directory-howto-tenant.md)。

## <a name="1-register-the-directory-searcher-application"></a>1.注册 DirectorySearcher 应用程序
若要让应用获取令牌，首先需要在 Azure AD 租户中注册该应用，并授予它访问 Azure AD 图形 API 的权限：

1. 登录到 [Azure 门户](https://portal.azure.cn)。
2. 在顶栏上单击帐户，并在“目录”列表下选择要注册应用程序的 Active Directory 租户。
3. 单击左侧导航栏中的“所有服务”，并选择“Azure Active Directory”。
4. 单击“应用注册”并选择“新建应用程序注册”。
5. 创建新的**本机**应用程序。
    - 应用程序的“名称”向最终用户描述应用程序
    - “重定向 URI”  是 Azure AD 要用来返回令牌响应的方案与字符串组合。 暂时输入一个占位符值，例如 `http://DirectorySearcher`。  稍后我们会替换此值。
6. 完成注册后，AAD 将为应用分配唯一的应用程序 ID。  在后面的部分中会用到此值，因此，请从应用程序选项卡中复制此值。
7. 在“设置”页上，依次选择“所需权限”和“添加”。 选择“Microsoft Graph”作为 API，并在“委派的权限”下添加“读取目录数据”权限。  这样，应用程序便可以在 Graph API 中查询用户。

## <a name="2-install--configure-adal"></a>2.安装并配置 ADAL
将应用程序注册到 Azure AD 后，可以安装 ADAL 并编写标识相关的代码。  为了使 ADAL 能够与 Azure AD 通信，需要为 ADAL 提供一些有关应用注册的信息。

- 首先，使用 Package Manager Console 将 ADAL 添加到 DirectorySearcher 项目。

    ```
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
    ```

- 在 DirectorySearcher 项目中，打开 `MainPage.xaml.cs`。  替换 `Config Values` 区域中的值，以反映你在 Azure 门户中输入的值。  只要使用 ADAL，代码就会引用这些值。
  - `tenant` 是 Azure AD 租户的域，例如 contoso.partner.onmschina.cn
  - `clientId` 是从门户复制的应用程序 clientId。
- 现在需要发现 Windows Phone 应用的回调 URI。  在 `MainPage` 方法中的此行上设置一个断点：

    ```
    redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
    ```

- 运行应用程序，并在到达断点时，将 `redirectUri` 的值复制到单独的位置。  该值应类似于

    ```
    ms-app://s-1-15-2-1352796503-54529114-405753024-3540103335-3203256200-511895534-1429095407/
    ```

- 返回 Azure 门户中应用的“设置”选项卡，添加具有上述值的 **RedirectUri**。  

## <a name="3-use-adal-to-get-tokens-from-aad"></a>3.使用 ADAL 从 AAD 获取令牌
ADAL 遵守的基本原理是，每当应用程序需要访问令牌时，它只需调用 `authContext.AcquireToken(…)`，ADAL 就会负责其余的工作。  

- 第一步是初始化应用程序的 `AuthenticationContext` （ADAL 的主类）。  将在此处传递 ADAL 与 Azure AD 通信时所需的坐标，并告诉 ADAL 如何缓存令牌。

```csharp
    public MainPage()
    {
        ...

        // ADAL for Windows Phone 8.1 builds AuthenticationContext instances through a factory
        authContext = AuthenticationContext.CreateAsync(authority).GetResults();
    }
    ```

- Now locate the `Search(...)` method, which will be invoked when the user cliks the "Search" button in the app's UI.  This method makes a GET request to the Azure AD Graph API to query for users whose UPN begins with the given search term.  But in order to query the Graph API, you need to include an access_token in the `Authorization` header of the request - this is where ADAL comes in.

```csharp
private async void Search(object sender, RoutedEventArgs e)
{
    ...

        // Try to get a token without triggering any user prompt.
        // ADAL will check whether the requested token is in ADAL's token cache or can otherwise be obtained without user interaction.
        AuthenticationResult result = await authContext.AcquireTokenSilentAsync(graphResourceId, clientId);
        if (result != null && result.Status == AuthenticationStatus.Success)
        {
            // A token was successfully retrieved.
            QueryGraph(result);
        }
        else
        {
            // Acquiring a token without user interaction was not possible.
            // Trigger an authentication experience and specify that once a token has been obtained the QueryGraph method should be called
            authContext.AcquireTokenAndContinue(graphResourceId, clientId, redirectURI, QueryGraph);
        }
    }
    ```

- If interactive authentication is necessary, ADAL will use Windows Phone's Web Authentication Broker (WAB) and [continuation model](http://www.cloudidentity.com/blog/2014/06/16/adal-for-windows-phone-8-1-deep-dive/) to display the Azure AD sign in page.  When the user signs in, your app needs to pass ADAL the results of the WAB interaction.  This is as simple as implementing the `ContinueWebAuthentication` interface:

```csharp
// This method is automatically invoked when the application
// is reactivated after an authentication interaction through WebAuthenticationBroker.
public async void ContinueWebAuthentication(WebAuthenticationBrokerContinuationEventArgs args)
{
    // pass the authentication interaction results to ADAL, which will
    // conclude the token acquisition operation and invoke the callback specified in AcquireTokenAndContinue.
    await authContext.ContinueAcquireTokenAsync(args);
}
```

- 现在，可以使用 ADAL 返回给应用程序的 `AuthenticationResult` 。  在 `QueryGraph(...)` 回调中，将获取的 access_token 附加到 Authorization 标头中的 GET 请求：

```csharp
private async void QueryGraph(AuthenticationResult result)
{
    if (result.Status != AuthenticationStatus.Success)
    {
        MessageDialog dialog = new MessageDialog(string.Format("If the error continues, please contact your administrator.\n\nError: {0}\n\nError Description:\n\n{1}", result.Error, result.ErrorDescription), "Sorry, an error occurred while signing you in.");
        await dialog.ShowAsync();
    }

        // Add the access token to the Authorization Header of the call to the Graph API, and call the Graph API.
        httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

        ...
    }
    ```

- You can also use the `AuthenticationResult` object to display information about the user in your app. In the `QueryGraph(...)` method, use the result to show the user's id on the page:

```csharp
// Update the Page UI to represent the signed in user
ActiveUser.Text = result.UserInfo.DisplayableId;
```
* 最后，还可以使用 ADAL 将用户从应用程序中注销。  当用户单击“注销”按钮时，我们希望确保 `AcquireTokenSilentAsync(...)` 的后续调用失败。  使用 ADAL 时，只需清除令牌缓存即可：

```csharp
private void SignOut()
{
    // Clear session state from the token cache.
    authContext.TokenCache.Clear();

        ...
    }
    ```

Congratulations! You now have a working Windows Phone app that has the ability to authenticate users, securely call Web APIs using OAuth 2.0, and get basic information about the user.  If you haven’t already, now is the time to populate your tenant with some users.  Run your DirectorySearcher app, and sign in with one of those users.  Search for other users based on their UPN.  Close the app, and re-run it.  Notice how the user’s session remains intact.  Sign out, and sign back in as another user.

ADAL makes it easy to incorporate all of these common identity features into your application.  It takes care of all the dirty work for you - cache management, OAuth protocol support, presenting the user with a login UI, refreshing expired tokens, and more.  All you really need to know is a single API call, `authContext.AcquireToken*(…)`.

For reference, the completed sample (without your configuration values) is provided [here](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip).  You can now move on to additional identity scenarios.  You may want to try:

[Secure a .NET Web API with Azure AD >>](active-directory-devquickstarts-webapi-dotnet.md)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]


<!--Update_Description: wording update -->