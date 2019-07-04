---
title: Xamarin iOS 注意事项（适用于 .NET 的 Microsoft 身份验证库）| Azure
description: 了解将 Xamarin iOS 与适用于 .NET 的 Microsoft 身份验证库 (MSAL.NET) 配合使用时的具体注意事项。
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
origin.date: 04/24/2019
ms.date: 06/18/2019
ms.author: v-junlch
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 82b30ee0e361a9f0d95f50be1692cf235bbd6cf6
ms.sourcegitcommit: 9d5fd3184b6a47bf3b60ffdeeee22a08354ca6b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2019
ms.locfileid: "67305985"
---
# <a name="xamarin-ios-specific-considerations-with-msalnet"></a>与 MSAL.NET 配合使用时特定于 Xamarin iOS 的注意事项
在 Xamarin iOS 上使用 MSAL.NET 时必须考虑的几个注意事项

- [iOS 12 和身份验证的已知问题](#known-issues-with-ios-12-and-authentication)
- [重写并实现 `AppDelegate` 中的 `OpenUrl` 函数](#implement-openurl)
- [启用密钥链组](#enable-keychain-groups)
- [启用令牌缓存共享](#enable-token-cache-sharing-across-ios-applications)
- [启用密钥链访问](#enable-keychain-access)

## <a name="known-issues-with-ios-12-and-authentication"></a>iOS 12 和身份验证的已知问题
Microsoft 已发布[安全公告](https://github.com/aspnet/AspNetCore/issues/4647)，其中提供了有关 iOS12 与某些身份验证类型之间的不兼容性的信息。 这种不兼容性会中断社交、WSFed 和 OIDC 登录。 此公告还提供了相关指导，让开发人员采取措施来消除 ASP.NET 当前在其应用程序中施加的安全限制，以便与 iOS12 兼容。  

在 Xamarin iOS 上开发 MSAL.NET 应用程序的过程中，尝试从 iOS 12 登录到网站时，你可能会看到无限循环（类似于此 [ADAL 问题](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/issues/1329)）。 

此外，你还可能会看到 iOS 12 Safari 中发生 ASP.NET Core OIDC 身份验证中断，如此 [WebKit 问题](https://bugs.webkit.org/show_bug.cgi?id=188165)中所述。

## <a name="implement-openurl"></a>实现 OpenUrl

首先需要重写 `FormsApplicationDelegate` 派生类的 `OpenUrl` 方法，并调用 `AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs`。

```csharp
public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
{
    AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(url);
    return true;
}
```

此外，需要定义 URL 方案，让应用获取所需的权限来调用另一个应用，对重定向 URL 采用特定的格式，并在 [Azure 门户](https://portal.azure.cn)中注册此重定向 URL。

## <a name="enable-keychain-groups"></a>启用密钥链组

若要正常使用令牌缓存和 `AcquireTokenSilentAsync` 方法，必须执行多个步骤：
1. 在 *`* Entitlements.plist* 文件中启用密钥链访问，并在捆绑包标识符中指定**密钥链组**。
2. 在 iOS 项目选项窗口的“捆绑包签名视图”中的“自定义权利”字段内选择 *`*Entitlements.plist*`* 文件。  
3. 为证书签名时，请确保 XCode 使用相同的 Apple ID。

## <a name="enable-token-cache-sharing-across-ios-applications"></a>启用 iOS 应用程序之间的令牌缓存共享

从 MSAL 2.x 开始，可以指定一个密钥链安全组，用于在多个应用程序之间保留令牌缓存。 这样，就可以在使用相同密钥链安全组的多个应用程序之间共享令牌缓存，其中包括使用 [ADAL.NET](https://aka.ms/adal-net) 开发的应用程序、MSAL.NET Xamarin.iOS 应用程序，以及使用 [ADAL.objc](https://github.com/AzureAD/azure-activedirectory-library-for-objc) 或 [MSAL.objc](https://github.com/AzureAD/microsoft-authentication-library-for-objc) 开发的本机 iOS 应用程序。

共享令牌缓存可以在使用相同密钥链安全组的所有应用程序之间实现单一登录 (SSO)。

若要启用单一登录，需将所有应用程序中的 `PublicClientApplication.iOSKeychainSecurityGroup` 属性设置为相同的值。

使用 MSAL v3.x 的示例如下：
```csharp
var builder = PublicClientApplicationBuilder
     .Create(ClientId)
     .WithIosKeychainSecurityGroup("com.microsoft.msalrocks")
     .Build();
```

使用 MSAL v2.7.x 的示例如下：

```csharp
PublicClientApplication.iOSKeychainSecurityGroup = "com.microsoft.msalrocks";
```

> [!NOTE]
> `KeychainSecurityGroup` 属性已弃用。 以前，在 MSAL 2.x 中，开发人员在使用 `KeychainSecurityGroup` 属性时必须包含 TeamId 前缀。 
> 
> 从 MSAL 2.7.x 开始，使用 `iOSKeychainSecurityGroup` 属性时，MSAL 会在运行时期间解析 TeamId 前缀。 使用此属性时，其值不应包含 TeamId 前缀。 
> 
> 请使用新的 `iOSKeychainSecurityGroup` 属性，开发人员无需在其中提供 TeamId。 `KeychainSecurityGroup` 属性现已过时。 

## <a name="enable-keychain-access"></a>启用密钥链访问

在 MSAL 2.x 和 ADAL 4.x 中，TeamId 用于访问密钥链，使身份验证库能够在同一发布者的应用程序之间提供单一登录 (SSO)。 

什么是 [TeamIdentifierPrefix](https://docs.microsoft.com/xamarin/ios/deploy-test/provisioning/entitlements?tabs=vsmac) (TeamId)？ 它是 App Store 中的唯一标识符。 每个应用的 AppId 是唯一的。 如果你有多个应用，所有这些应用的 TeamId 是相同的，但 AppId 是不同的。 系统自动为每个密钥链访问组加上 TeamId 前缀。 OS 根据此前缀，使得同一发布者的应用可以访问共享的密钥链。 

初始化 `PublicClientApplication` 时，如果收到 `MsalClientException` 和消息 `TeamId returned null from the iOS keychain...`，则需要在 iOS Xamarin 应用中执行以下操作：

1. 在 VS 中的“调试”选项卡下，转到“nameOfMyApp.iOS 属性...”
2. 然后转到“iOS 捆绑包签名” 
3. 在“自定义权利”下，单击“...”并选择应用中的“Entitlements.plist”文件
4. 在 iOS 应用的 csproj 文件中，现在应会包含以下行：`<CodesignEntitlements>Entitlements.plist</CodesignEntitlements>`
5. **重新生成**项目。

除此之外，也可以使用以下访问组或你自己的访问组，在 `Entitlements.plist` 文件中启用密钥链访问： 

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>keychain-access-groups</key>
  <array>
    <string>$(AppIdentifierPrefix)com.microsoft.adalcache</string>
  </array>
</dict>
</plist>
```

## <a name="next-steps"></a>后续步骤

以下示例的 readme.md 文件的[特定于 iOS 的注意事项](https://github.com/azure-samples/active-directory-xamarin-native-v2#ios-specific-considerations)段落中提供了更多详细信息：

示例 | 平台 | 说明 
------ | -------- | -----------
[https://github.com/Azure-Samples/active-directory-xamarin-native-v2](https://github.com/azure-samples/active-directory-xamarin-native-v2) | Xamarin iOS、Android、UWP | 一个简单的 Xamarin Forms 应用，演示了如何使用 MSAL 通过 AAD V2.0 终结点对 MSA 和 Azure AD 进行身份验证，以及如何使用生成的令牌访问 Microsoft Graph。 <br>![拓扑](./media/msal-net-xamarin-ios-considerations/topology.png)

