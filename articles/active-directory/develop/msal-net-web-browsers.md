---
title: 适用于 .NET 的 Microsoft 身份验证库中的 Web 浏览器 | Azure
description: 了解将 Xamarin Android 与适用于 .NET 的 Microsoft 身份验证库 (MSAL.NET) 配合使用时的具体注意事项。
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
origin.date: 05/06/2019
ms.date: 06/18/2019
ms.author: v-junlch
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 19fa4103361bfd46d6346aa92c5c630350d2eaa1
ms.sourcegitcommit: 9d5fd3184b6a47bf3b60ffdeeee22a08354ca6b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2019
ms.locfileid: "67305864"
---
# <a name="using-web-browsers-in-msalnet"></a>使用 MSAL.NET 中的 Web 浏览器
完成交互式身份验证需要使用 Web 浏览器。 默认情况下，MSAL.NET 支持 Xamarin.iOS 和 [Xamarin.Android](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/system-browser) 上的[系统 Web 浏览器](#system-web-browser-on-xamarinios-and-xamarinandroid)。 但是，你也可以根据 [Xamarin.iOS](#choosing-between-embedded-web-browser-or-system-browser-on-xamarinios) 和 [Xamarin.Android](#choosing-between-embedded-web-browser-or-system-browser-on-xamarinandroid) 应用中的要求（UX、安全性），[启用嵌入式 Web 浏览器](#enable-embedded-webviews)。 甚至可以根据 Android 中是否存在 Chrome 或支持 Chrome 自定义标签页的浏览器，[动态选择](#detecting-the-presence-of-custom-tabs-on-xamarinandroid)要使用的 Web 浏览器。

## <a name="web-browsers-in-msalnet"></a>MSAL.NET 中的 Web 浏览器

必须知道，在以交互方式获取令牌时，对话框的内容不是由该库提供，而是由 STS（安全令牌服务）提供。 身份验证终结点将发回一些用于控制交互的 HTML 和 JavaScript 内容，这些内容在 Web 浏览器或 Web 控件中呈现。 让 STS 处理 HTML 交互可以带来诸多好处：

- 应用程序和身份验证库永远不会存储密码（如果用户键入了密码）。
- 可重定向到其他标识提供者（例如，在 Azure AD B2C 中使用工作和学校帐户或社交帐户登录）。
- 可让 STS 控制条件访问，例如，让用户在身份验证阶段执行多重身份验证 (MFA)（输入 Windows Hello PIN、在手机上接听来电，或者在手机上的身份验证应用中接收验证码）。 如果尚未设置所需的多重身份验证方法，用户可以在同一个对话框中及时设置该方法。  用户输入其手机号码后，系统会引导他们安装身份验证应用程序，然后用户扫描 QR 标记即可添加其帐户。 此服务器驱动的交互方式是一种极佳的体验！
- 用户密码过期时，可让用户在同一个对话框中更改密码（分别提供旧密码和新密码的字段）。
- 支持为 Azure AD 租户管理员/应用程序所有者控制的租户或应用程序（映像）设计品牌形象。
- 可让用户只许可应用程序在身份验证后访问资源/其名称中的范围。

## <a name="system-web-browser-on-xamarinios-and-xamarinandroid"></a>Xamarin.iOS 和 Xamarin.Android 上的系统 Web 浏览器

默认情况下，MSAL.NET 支持 Xamarin.iOS 和 Xamarin.Android 上的系统 Web 浏览器。 用于提供 UI 的所有平台（非 .NET Core），该库会提供一个嵌入了 Web 浏览器控件的对话框。 对于 .NET Desktop 以及 UWP 平台的  WAB，MSAL.NET 还会使用嵌入式 Web 视图。 但是，它默认利用 Xamarin iOS 和 Xamarin Android 应用程序的**系统 Web 浏览器**。 在 iOS 上，它甚至会根据操作系统的版本（iOS12、iOS11 和更低）选择要使用的 Web 视图。

### <a name="uwp-does-not-use-the-system-webview"></a>UWP 不使用 System Web 视图

但是，对于桌面应用程序，启动系统 Web 视图会导致欠佳的用户体验，因为用户会看到浏览器，而他们可能已在其中打开了其他标签页。 发生身份验证时，用户会看到一个页面，其中要求他们关闭此窗口。 用户可能会一不小心关闭的整个进程（包括与身份验证不相关的其他标签页）。 在桌面上利用系统浏览器还需要打开本地端口并在其上侦听，这可能需要应用程序的高级权限。 开发人员、用户或管理员可能不情愿迎合此要求。

## <a name="enable-embedded-webviews"></a>启用嵌入式 Web 视图 
也可以在 Xamarin.iOS 和 Xamarin.Android 应用中启用嵌入式 Web 视图。 从 MSAL.NET 2.0.0-preview 开始，MSAL.NET 也支持使用**嵌入式** Web 视图选项。 对于 ADAL.NET 而言，嵌入式 Web 视图是唯一支持的选项。

开发人员在使用面向 Xamarin 的 MSAL.NET 时，可以选择使用嵌入式 Web 视图或系统浏览器。 可以根据用户体验和安全考虑因素做出选择。

目前，MSAL.NET 尚不支持 Android 和 iOS 代理。 MSAL.NET 正在规划支持在嵌入式 Web 浏览器中使用代理。

### <a name="differences-between-embedded-webview-and-system-browser"></a>嵌入式 Web 视图与系统浏览器之间的差异 
MSAL.NET 中嵌入式 Web 视图与系统浏览器之间一些视觉差异。

**使用嵌入式 Web 视图在 MSAL.NET 中进行交互式登录：**

![嵌入式](./media/msal-net-web-browsers/embedded-webview.png)

**使用系统浏览器在 MSAL.NET 中进行交互式登录：**

![系统浏览器](./media/msal-net-web-browsers/system-browser.png)

### <a name="developer-options"></a>开发人员选项

开发人员在使用 MSAL.NET 时，可以通过多个选项来显示 STS 的交互式对话框：

- **系统浏览器。** 系统浏览器默认已在库中设置。 如果使用的是 Android，请阅读[系统浏览器](msal-net-system-browser-android-considerations.md)了解有关支持用于身份验证的浏览器的具体信息。 在 Android 中使用系统浏览器时，我们建议在设备中安装支持 Chrome 自定义标签页的浏览器。  否则，身份验证可能失败。
- **嵌入式 Web 视图。** 若要在 MSAL.NET 中仅使用嵌入式 Web 视图，请在 `AcquireTokenInteractively` 参数生成器中包含 `WithUseEmbeddedWebView()` 方法。

    iOS

    ```csharp
    AuthenticationResult authResult;
    authResult = app.AcquireTokenInteractively(scopes)
                    .WithUseEmbeddedWebView(useEmbeddedWebview)
                    .ExecuteAsync();
    ```

    Android：

    ```csharp
    authResult = app.AcquireTokenInteractively(scopes)
                .WithParentActivityOrWindow(activity)
                .WithUseEmbeddedWebView(useEmbeddedWebview)
                .ExecuteAsync();
    ```

#### <a name="choosing-between-embedded-web-browser-or-system-browser-on-xamarinios"></a>在 Xamarin.iOS 上的嵌入式 Web 浏览器与系统浏览器之间进行选择

在 iOS 应用中，可将 `AppDelegate.cs` 中的 `ParentWindow` 初始化为 `null`。 iOS 不使用该参数

```csharp
App.ParentWindow = null; // no UI parent on iOS
```

#### <a name="choosing-between-embedded-web-browser-or-system-browser-on-xamarinandroid"></a>在 Xamarin.Android 上的嵌入式 Web 浏览器与系统浏览器之间进行选择

在 Android 应用中，可在 `MainActivity.cs` 中设置父活动，以便将身份验证结果返回到其中：

```csharp
 App.ParentWindow = this;
```

然后在 `MainPage.xaml.cs` 中：

```csharp
authResult = await App.PCA.AcquireTokenInteractive(App.Scopes)
                      .WithParentActivityOrWindow(App.ParentWindow)
                      .WithUseEmbeddedWebView(true)
                      .ExecuteAsync();
```

## <a name="net-core-does-not-support-interactive-authentication-out-of-the-box"></a>.NET Core 不能现成地支持交互式身份验证

对于 .NET Core，无法以交互方式获取令牌。 实际上，.NET Core 目前并不提供 UI。 若要为 .NET Core 应用程序提供交互式登录，可让应用程序向用户显示一个用于以交互方式登录的代码和 URL（请参阅[设备代码流](msal-authentication-flows.md#device-code)）。

或者，可以实现 [IWithCustomUI](scenario-desktop-acquire-token.md#withcustomwebui) 接口并提供自己的浏览器

