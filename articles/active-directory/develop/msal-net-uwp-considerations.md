---
title: 通用 Windows 平台注意事项（适用于 .NET 的 Microsoft 身份验证库）| Azure
description: 了解将通用 Windows 平台与适用于 .NET 的 Microsoft 身份验证库 (MSAL.NET) 配合使用时的具体注意事项。
services: active-directory
documentationcenter: dev-center-name
author: TylerMSFT
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
origin.date: 07/16/2019
ms.date: 08/23/2019
ms.author: v-junlch
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 507e214298e3b57f2b3ae286d7c3ede401cf8d58
ms.sourcegitcommit: 599d651afb83026938d1cfe828e9679a9a0fb69f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/23/2019
ms.locfileid: "69993256"
---
# <a name="universal-windows-platform-specific-considerations-with-msalnet"></a>与 MSAL.NET 配合使用时特定于通用 Windows 平台的注意事项
在 UWP 上使用 MSAL.NET 时必须考虑的几个注意事项。

## <a name="the-usecorporatenetwork-property"></a>UseCorporateNetwork 属性
在 WinRT 平台中，`PublicClientApplication` 具有下面的布尔属性 ``UseCorporateNetwork``。 有了此属性，Win8.1 和 UWP 应用程序就可以在用户使用联合 Azure AD 租户中的帐户登录时利用 Windows 集成身份验证。 设置此属性后，MSAL.NET 会利用 WAB（Web 身份验证代理）。

> [!IMPORTANT]
> 将此属性设置为 true 时，已经假定应用程序开发人员在应用程序中启用了 Windows 集成身份验证 (IWA)。 为此，请执行以下操作：
> - 在适用于 UWP 应用程序的 ``Package.appxmanifest`` 的“功能”选项卡中，启用以下功能： 
>   - 企业身份验证
>   - 专用网络(客户端和服务器)
>   - 共享用户证书

默认情况下未启用 IWA，因为请求企业身份验证或共享用户证书功能的应用程序需要 Windows 应用商店接受更高级别的验证，但并非所有开发人员都希望执行更高级别的验证。

在 UWP 平台上的基础实现 (WAB) 在启用了条件访问的企业方案中无法正确使用。 症状是，用户尝试使用 Windows hello 登录，系统提议其选择一个证书，但是

- 找不到 pin 的证书，
- 或者用户选择了它，但系统却不提示其输入 Pin.

解决办法是使用替代方法（用户名/密码 + 对话身份验证），但体验不好。

## <a name="troubleshooting"></a>故障排除

某些客户报告，在某些特定的企业环境中，出现以下登录错误：

```Text
We can't connect to the service you need right now. Check your network connection or try this again later
```

而这些客户知道他们有 Internet 连接，在公共网络中可以使用。

解决办法是确保 WAB（Windows 基础组件）允许专用网络。 为此，可以设置一个注册表项：

```Text
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\authhost.exe\EnablePrivateNetwork = 00000001
```

有关详细信息，请参阅 [Web 身份验证代理 - Fiddler](https://docs.microsoft.com/windows/uwp/security/web-authentication-broker#fiddler)。

## <a name="next-steps"></a>后续步骤
以下示例提供了更多详细信息：

示例 | 平台 | 说明 
|------ | -------- | -----------|
|[active-directory-dotnet-native-uwp-v2](https://github.com/azure-samples/active-directory-dotnet-native-uwp-v2) | UWP | 通用 Windows 平台客户端应用程序，它使用 msal.net，访问 Microsoft Graph 来通过 Azure AD v2.0 终结点进行用户身份验证。 <br>![拓扑](./media/msal-net-uwp-considerations/topology-native-uwp.png)|
|[https://github.com/Azure-Samples/active-directory-xamarin-native-v2](https://github.com/Azure-Samples/active-directory-xamarin-native-v2) | Xamarin iOS、Android、UWP | 一个简单的 Xamarin Forms 应用，它展示了如何使用 MSAL 通过 AAD v2.0 终结点对 MSA 和 Azure AD 进行身份验证，以及如何使用生成的令牌访问 Microsoft Graph。 <br>![拓扑](./media/msal-net-uwp-considerations/topology-xamarin-native.png)|

<!-- Update_Description: wording update -->