---
title: 在 Xamarin iOS 和 Android 应用程序中使用 Microsoft Authenticator 或 Microsoft Intune 公司门户 | Azure
description: 了解如何将使用 Microsoft Authenticator 的 Xamarin iOS 应用程序从适用于 .NET 的 Azure AD 身份验证库 (ADAL.NET) 迁移到适用于 .NET 的 Microsoft 身份验证库 (MSAL.NET)
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
origin.date: 09/08/2019
ms.date: 09/30/2019
ms.author: v-junlch
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 83ab91ef41dc9432cfafe20c4d6f5d9ebc4dd0ac
ms.sourcegitcommit: 74f50c9678e190e2dbb857be530175f25da8905e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/12/2019
ms.locfileid: "72292161"
---
# <a name="use-microsoft-authenticator-or-microsoft-intune-company-portal-on-xamarin-applications"></a>在 Xamarin 应用程序中使用 Microsoft Authenticator 或 Microsoft Intune 公司门户

在 Android 和 iOS 上，Microsoft Authenticator 或 Microsoft Intune 公司门户（仅限 Android）等中介可以实现：

- 设备标识。 通过访问设备证书，该证书是在设备加入工作区时在设备上创建的。
- 应用程序标识验证。 当应用程序调用中介时，它会传递其重定向 URL，而中介会验证该 URL。

若要启用这些功能之一，应用程序开发人员需要在调用 `PublicClientApplicationBuilder.CreateApplication` 方法时使用 `WithBroker()` 参数。 `.WithBroker()` 默认设置为 true。 开发人员还需要对 [iOS](#brokered-authentication-for-ios) 或 [Android](#brokered-authentication-for-android) 应用程序执行以下步骤。

## <a name="brokered-authentication-for-ios"></a>适用于 iOS 的中介身份验证

遵循以下步骤，使 Xamarin.iOS 应用能够与 Microsoft Authenticator 应用通信。

### <a name="step-1-enable-broker-support"></a>步骤 1：启用中介支持
中介支持是按 PublicClientApplication 启用的。 此项默认禁用。 通过 PublicClientApplicationBuilder 创建 PublicClientApplication 时，请使用 `WithBroker()` 参数（默认设置为 true）。

```CSharp
var app = PublicClientApplicationBuilder
                .Create(ClientId)
                .WithBroker()
                .WithReplyUri(redirectUriOnIos) // $"msauth.{Bundle.Id}://auth" (see step 6 below)
                .Build();
```

### <a name="step-2-update-appdelegate-to-handle-the-callback"></a>步骤 2：更新 AppDelegate 以处理回调
当 MSAL.NET 调用中介时，中介将通过 `AppDelegate` 类的 `OpenUrl` 方法回调应用程序。 由于 MSAL 将等待来自中介的响应，因此应用程序需要与中介配合才能回调 MSAL.NET。 若要启用此协作，请更新 `AppDelegate.cs` 文件以重写以下方法。

```CSharp
public override bool OpenUrl(UIApplication app, NSUrl url, 
                             string sourceApplication,
                             NSObject annotation)
{
    if (AuthenticationContinuationHelper.IsBrokerResponse(sourceApplication))
    {
      AuthenticationContinuationHelper.SetBrokerContinuationEventArgs(url);
      return true;
    }
    
    else if (!AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(url))
    {               
         return false;                
    }
    
    return true;     
}           
```

每次启动应用程序都会调用此方法，你可以借此机会处理中介的响应，并完成 MSAL.NET 发起的身份验证过程。

### <a name="step-3-set-a-uiviewcontroller"></a>步骤 3：设置 UIViewController()
需要在 `AppDelegate.cs` 中设置对象窗口。 在 Xamarin iOS 中，通常不需要设置对象窗口，但若要发送请求并接收中介的响应，则需要设置对象窗口。 

为此，需要执行以下两项操作。 
1) 在 `AppDelegate.cs` 中，将 `App.RootViewController` 设置为新的 `UIViewController()`。 这种分配可确保提供一个 UIViewController 来调用中介。 如果未正确设置此参数，可能会收到以下错误：`"uiviewcontroller_required_for_ios_broker":"UIViewController is null, so MSAL.NET cannot invoke the iOS broker. See https://aka.ms/msal-net-ios-broker"`
2) 在 AcquireTokenInteractive 调用中，使用 `.WithParentActivityOrWindow(App.RootViewController)` 并传入对你要使用的对象窗口的引用。

例如： 

在 `App.cs`中：
```CSharp
   public static object RootViewController { get; set; }
```
在 `AppDelegate.cs`中：
```CSharp
   LoadApplication(new App());
   App.RootViewController = new UIViewController();
```
在“获取令牌”调用中：
```CSharp
result = await app.AcquireTokenInteractive(scopes)
             .WithParentActivityOrWindow(App.RootViewController)
             .ExecuteAsync();
```

### <a name="step-4-register-a-url-scheme"></a>步骤 4：注册 URL 方案
MSAL.NET 使用 URL 调用中介，然后将中介响应返回到应用。 若要完成往返过程，需要在 `Info.plist` 文件中注册应用的 URL 方案。

`CFBundleURLSchemes` 名称必须包含 `msauth.` 作为前缀，后接 `CFBundleURLName`。

`$"msauth.(BundleId)"`

**例如：** 
`msauth.com.yourcompany.xforms`

> [!NOTE]
> 接收中介的响应时，此内容将成为用于唯一标识应用的 RedirectUri 的一部分。

```XML
 <key>CFBundleURLTypes</key>
    <array>
      <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>com.yourcompany.xforms</string>
        <key>CFBundleURLSchemes</key>
        <array>
          <string>msauth.com.yourcompany.xforms</string>
        </array>
      </dict>
    </array>
```

### <a name="step-5-lsapplicationqueriesschemes"></a>步骤 5：LSApplicationQueriesSchemes
MSAL 使用 `-canOpenURL:` 来检查是否在设备上安装了中介。 在 iOS 9 中，Apple 锁定了应用程序可以查询的方案。 

将 **`msauthv2`** **添加**到 `Info.plist` 文件的 `LSApplicationQueriesSchemes` 节。

```XML 
<key>LSApplicationQueriesSchemes</key>
    <array>
      <string>msauthv2</string>
    </array>
```

### <a name="step-6-register-your-redirecturi-in-the-application-portal"></a>步骤 6：在应用程序门户中注册 RedirectUri
使用中介需要满足 redirectUri 方面的额外要求。 redirectUri **必须**采用以下格式： 
```CSharp
$"msauth.{BundleId}://auth"
```
例如： 
```CSharp
public static string redirectUriOnIos = "msauth.com.yourcompany.XForms://auth"; 
```
**可以看到，RedirectUri 与 `Info.plist` 文件中包含的 `CFBundleURLSchemes` 名称相匹配。**

### <a name="step-7-make-sure-the-redirect-uri-is-registered-with-your-app"></a>步骤 7：确保将重定向 URI 注册到应用

需要在应用注册门户上注册此重定向 URI （ https://portal.azure.cn) 用作应用程序的有效重定向 URI）。 

应用注册门户提供新的体验，可帮助你根据捆绑 ID 计算中介回复 URI：

1. 在应用注册中选择“身份验证”，然后选择“尝试新体验”。  
   ![尝试新的应用注册体验](./media/msal-net-use-brokers-with-xamarin-apps/60799285-2d031b00-a173-11e9-9d28-ac07a7ae894a.png)

2. 选择“添加平台”。 
   ![添加平台](./media/msal-net-use-brokers-with-xamarin-apps/60799366-4c01ad00-a173-11e9-934f-f02e26c9429e.png)

3. 如果支持平台列表，请选择“iOS”。 
   ![配置 iOS](./media/msal-net-use-brokers-with-xamarin-apps/60799411-60de4080-a173-11e9-9dcc-d39a45826d42.png)

4. 依请求输入捆绑 ID，然后按“注册”。 
   ![输入捆绑 ID](./media/msal-net-use-brokers-with-xamarin-apps/60799477-7eaba580-a173-11e9-9f8b-431f5b09344e.png)

5. 此时会为你计算重定向 URI。
   ![复制重定向 URI](./media/msal-net-use-brokers-with-xamarin-apps/60799538-9e42ce00-a173-11e9-860a-015a1840fd19.png)

## <a name="brokered-authentication-for-android"></a>适用于 Android 的中介身份验证

中介支持不适用于 Android

## <a name="next-steps"></a>后续步骤

[与 MSAL.NET 配合使用时特定于通用 Windows 平台的注意事项](msal-net-uwp-considerations.md)

