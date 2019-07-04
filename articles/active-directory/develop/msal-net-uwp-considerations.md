---
title: 通用 Windows 平台注意事项（适用于 .NET 的 Microsoft 身份验证库）| Azure
description: 了解将通用 Windows 平台与适用于 .NET 的 Microsoft 身份验证库 (MSAL.NET) 配合使用时的具体注意事项。
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
ms.openlocfilehash: a8018cc1fd0a42139f7733c582e0a2807d11b5b4
ms.sourcegitcommit: 9d5fd3184b6a47bf3b60ffdeeee22a08354ca6b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2019
ms.locfileid: "67305953"
---
# <a name="universal-windows-platform-specific-considerations-with-msalnet"></a>与 MSAL.NET 配合使用时特定于通用 Windows 平台的注意事项
在 Xamarin iOS 上使用 MSAL.NET 时必须考虑的几个注意事项。

## <a name="the-usecorporatenetwork-property"></a>UseCorporateNetwork 属性
在 WinRT 平台中，`PublicClientApplication` 具有下面的布尔属性 ``UseCorporateNetwork``。 有了此属性，Win8.1 和 UWP 应用程序就可以在用户使用联合 Azure AD 租户中的帐户登录时利用 Windows 集成身份验证。 这利用 WAB（Web 身份验证代理）。 

> [!IMPORTANT]
> 将此属性设置为 true 时，已经假定应用程序开发人员在应用程序中启用了 Windows 集成身份验证 (IWA)。 为此，请执行以下操作：
> - 在适用于 UWP 应用程序的 ``Package.appxmanifest`` 的“功能”选项卡中，启用以下功能： 
>   - 企业身份验证
>   - 专用网络(客户端和服务器)
>   - 共享用户证书

默认情况下未启用 IWA，因为请求企业身份验证或共享用户证书功能的应用程序需要 Windows 应用商店接受更高级别的验证，但并非所有开发人员都希望执行更高级别的验证。 

## <a name="next-steps"></a>后续步骤
以下示例提供了更多详细信息：

示例 | 平台 | 说明 
|------ | -------- | -----------|
|[active-directory-dotnet-native-uwp-v2](https://github.com/azure-samples/active-directory-dotnet-native-uwp-v2) | UWP | 通用 Windows 平台客户端应用程序，它使用 msal.net，访问 Microsoft Graph 来通过 Azure AD v2.0 终结点进行用户身份验证。 <br>![拓扑](./media/msal-net-uwp-considerations/topology-native-uwp.png)|
|[https://github.com/Azure-Samples/active-directory-xamarin-native-v2](https://github.com/Azure-Samples/active-directory-xamarin-native-v2) | Xamarin iOS、Android、UWP | 一个简单的 Xamarin Forms 应用，它展示了如何使用 MSAL 通过 AAD v2.0 终结点对 MSA 和 Azure AD 进行身份验证，以及如何使用生成的令牌访问 Microsoft Graph。 <br>![拓扑](./media/msal-net-uwp-considerations/topology-xamarin-native.png)|

