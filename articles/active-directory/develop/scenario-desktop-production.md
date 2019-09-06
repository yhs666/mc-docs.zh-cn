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
origin.date: 04/18/2019
ms.date: 08/26/2019
ms.author: v-junlch
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: b723146bee56d876dd1d7ae1bb0c39717e31da70
ms.sourcegitcommit: 18a0d2561c8b60819671ca8e4ea8147fe9d41feb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2019
ms.locfileid: "70134112"
---
# <a name="desktop-app-that-calls-web-apis---move-to-production"></a>调用 Web API 的桌面应用 - 移到生产环境

本文提供了进一步改进应用程序并将其移到生产环境的详细信息。

## <a name="handling-errors-in-desktop-applications"></a>在桌面应用程序中处理错误

你已了解，在不同的流中如何处理静默流的错误（如代码片段所示）。 你还了解，有些情况下需要交互（增量许可和条件访问）。

## <a name="how-to-have--the-user-consent-upfront-for-several-resources"></a>如何让用户提前许可多个资源

> [!NOTE]
> 获得多个资源的同意适用于 Microsoft 标识平台，但不适用于 Azure Active Directory (Azure AD) B2C。 Azure AD B2C 仅支持管理员同意，不支持用户同意。

Microsoft 标识平台 (v2.0) 终结点不允许你一次获取多个资源的令牌。 因此，`scopes` 参数只能包含单个资源的范围。 可以使用 `extraScopesToConsent` 参数确保用户预先同意多个资源。

例如，如果你有两个资源（每个资源有两个范围）：

- `https://mytenant.partner.onmschina.cn/customerapi` - 具有 2 个范围 `customer.read` 和 `customer.write`
- `https://mytenant.partner.onmschina.cn/vendorapi` - 具有 2 个范围 `vendor.read` 和 `vendor.write`

应使用具有 `extraScopesToConsent` 参数的 `.WithAdditionalPromptToConsent` 修饰符。

例如：

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

此调用将为你获得第一个 Web API 的访问令牌。

需要调用第二个 Web API 时，可以调用：

```CSharp
AcquireTokenSilent(scopesForVendorApi, accounts.FirstOrDefault()).ExecuteAsync();
```

## <a name="next-steps"></a>后续步骤

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]

<!-- Update_Description: wording update -->