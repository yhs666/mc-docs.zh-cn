---
title: 调用 Web API 的桌面应用（移到生产环境）- Microsoft 标识平台
description: 了解如何构建调用 Web API 的桌面应用（移到生产环境）
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
origin.date: 10/30/2019
ms.date: 11/07/2019
ms.author: v-junlch
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 352bfc868c610747201e9efe9db9dffe964a1bbe
ms.sourcegitcommit: a88cc623ed0f37731cb7cd378febf3de57cf5b45
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/08/2019
ms.locfileid: "73830922"
---
# <a name="desktop-app-that-calls-web-apis---move-to-production"></a>调用 Web API 的桌面应用 - 移到生产环境

本文提供了进一步改进应用程序并将其移到生产环境的详细信息。

## <a name="handling-errors-in-desktop-applications"></a>在桌面应用程序中处理错误

你已了解，在不同的流中如何处理静默流的错误（如代码片段所示）。 你还了解，有些情况下需要交互（增量许可）。

## <a name="how-to-have--the-user-consent-upfront-for-several-resources"></a>如何让用户提前许可多个资源

> [!NOTE]
> 获得多个资源的同意适用于 Microsoft 标识平台，但不适用于 Azure Active Directory (Azure AD) B2C。 Azure AD B2C 仅支持管理员同意，不支持用户同意。

Microsoft 标识平台 (v2.0) 终结点不允许你一次获取多个资源的令牌。 因此，`scopes` 参数只能包含单个资源的范围。 可以使用 `extraScopesToConsent` 参数确保用户预先同意多个资源。

例如，如果你有两个资源（每个资源有两个范围）：

- `https://mytenant.partner.onmschina.cn/customerapi` - 具有 2 个范围 `customer.read` 和 `customer.write`
- `https://mytenant.partner.onmschina.cn/vendorapi` - 具有 2 个范围 `vendor.read` 和 `vendor.write`

应使用具有 `extraScopesToConsent` 参数的 `.WithAdditionalPromptToConsent` 修饰符。

例如：

### <a name="in-msalnet"></a>在 MSAL.NET 中

```CSharp
string[] scopesForCustomerApi = new string[]
{
  "https://mytenant.partner.onmschina.cn/customerapi/customer.read",
  "https://mytenant.partner.onmschina.cn/customerapi/customer.write"
};
string[] scopesForVendorApi = new string[]
{
 "https://mytenant.partner.onmschina.cn/vendorapi/vendor.read",
 "https://mytenant.partner.onmschina.cn/vendorapi/vendor.write"
};

var accounts = await app.GetAccountsAsync();
var result = await app.AcquireTokenInteractive(scopesForCustomerApi)
                     .WithAccount(accounts.FirstOrDefault())
                     .WithExtraScopeToConsent(scopesForVendorApi)
                     .ExecuteAsync();
```

### <a name="in-msal-for-ios-and-macos"></a>在适用于 iOS 和 macOS 的 MSAL 中

Objective-C：

```objc
NSArray *scopesForCustomerApi = @[@"https://mytenant.partner.onmschina.cn/customerapi/customer.read",
                                @"https://mytenant.partner.onmschina.cn/customerapi/customer.write"];

NSArray *scopesForVendorApi = @[@"https://mytenant.partner.onmschina.cn/vendorapi/vendor.read",
                              @"https://mytenant.partner.onmschina.cn/vendorapi/vendor.write"]

MSALInteractiveTokenParameters *interactiveParams = [[MSALInteractiveTokenParameters alloc] initWithScopes:scopesForCustomerApi webviewParameters:[MSALWebviewParameters new]];
interactiveParams.extraScopesToConsent = scopesForVendorApi;
[application acquireTokenWithParameters:interactiveParams completionBlock:^(MSALResult *result, NSError *error) { /* handle result */ }];
```

Swift：

```swift
let scopesForCustomerApi = ["https://mytenant.partner.onmschina.cn/customerapi/customer.read",
                            "https://mytenant.partner.onmschina.cn/customerapi/customer.write"]

let scopesForVendorApi = ["https://mytenant.partner.onmschina.cn/vendorapi/vendor.read",
                          "https://mytenant.partner.onmschina.cn/vendorapi/vendor.write"]

let interactiveParameters = MSALInteractiveTokenParameters(scopes: scopesForCustomerApi, webviewParameters: MSALWebviewParameters())
interactiveParameters.extraScopesToConsent = scopesForVendorApi
application.acquireToken(with: interactiveParameters, completionBlock: { (result, error) in /* handle result */ })
```

此调用将为你获得第一个 Web API 的访问令牌。

需要调用第二个 Web API 时，可以调用 `AcquireTokenSilent` API：

```CSharp
AcquireTokenSilent(scopesForVendorApi, accounts.FirstOrDefault()).ExecuteAsync();
```

## <a name="next-steps"></a>后续步骤

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]

<!-- Update_Description: wording update -->