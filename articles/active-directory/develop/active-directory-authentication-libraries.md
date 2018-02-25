---
title: "Azure Active Directory 身份验证库 | Microsoft Docs"
description: "通过 Azure AD 身份验证库 (ADAL)，客户端应用程序开发人员能够轻松地使用户通过云或本地 Active Directory (AD) 的身份验证，并获取访问令牌，以进行安全的 API 调用。"
services: active-directory
documentationcenter: 
author: alexchen2016
manager: digimobile
editor: mbaldwin
ms.assetid: 2e4fc79a-0285-40be-8c77-65edee408a22
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
origin.date: 08/25/2017
ms.date: 09/20/2017
ms.author: v-junlch
ms.custom: aaddev
ms.openlocfilehash: e3d0383cc0cf9e799338d67f2cb0a7a4b3e8829e
ms.sourcegitcommit: 0b0d3b61e91a97277de8eda8d7a8e114b7c4d8c1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/23/2018
---
# <a name="azure-active-directory-authentication-libraries"></a>Azure Active Directory 身份验证库
通过 Azure Active Directory Authentication Library (ADAL)，应用程序开发者可以让用户通过云或本地 Active Directory (AD) 的身份验证，并获取令牌来保护 API 调用。 ADAL 通过以下功能使开发者更轻松地进行身份验证：
 - 存储访问令牌和刷新令牌的可配置令牌缓存
 - 当访问令牌过期且刷新令牌可用时，自动刷新令牌
 - 支持异步方法调用
 - 更多

### <a name="client-libraries"></a>客户端库

| 平台 | 库 | 下载 | 源代码 | 示例 | 参考
| --- | --- | --- | --- | --- | --- |
| .NET 客户端、Windows 应用商店、UWP、Xamarin iOS 和 Android |ADAL .NET v3 |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet) | [桌面应用](/active-directory/develop/active-directory-devquickstarts-dotnet) |[引用](https://docs.microsoft.com/dotnet/api/?view=identitymodelclientsad-3.13.9) | 
| .NET 客户端、Windows 应用商店、Windows Phone 8.1 |ADAL .NET v2 |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/2.28.4) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/releases/tag/v2.28.4) | [桌面应用](https://github.com/AzureADQuickStarts/NativeClient-DotNet/releases/tag/v2.X) | | 
| Javascript |ADAL.js |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-js) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-js) |[单页应用](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) | |
| iOS、macOS |ADAL |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-objc/releases) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-objc) |[iOS 应用](/active-directory/develop/active-directory-devquickstarts-ios) | [引用](https://cocoapods.org/pods/ADAL)|
| Android |ADAL |[中央存储库](http://search.maven.org/remotecontent?filepath=com/microsoft/aad/adal/) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-android) |[Android 应用](/active-directory/develop/active-directory-devquickstarts-android) | [JavaDocs](http://javadoc.io/doc/com.microsoft.aad/adal/)|
| Node.js |ADAL |[npm](https://www.npmjs.com/package/adal-node) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-nodejs) | | |
| Java |ADAL4J |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-java) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-java) |[Java Web 应用](/active-directory/develop/active-directory-devquickstarts-webapp-java) | |
| Python |ADAL |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-python) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-python) | | |

### <a name="server-libraries"></a>服务器库 

| 平台 | 库 | 下载 | 源代码 | 示例 | 参考
| --- | --- | --- | --- | --- | --- |
| .NET |OWIN for AzureAD|[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.ActiveDirectory/) |[CodePlex](http://katanaproject.codeplex.com) |[MVC 应用](/active-directory/develop/active-directory-devquickstarts-webapp-dotnet) | |
| .NET |OWIN for OpenIDConnect |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect) |[CodePlex](http://katanaproject.codeplex.com) |[Web 应用](https://github.com/AzureADSamples/WebApp-OpenIDConnect-DotNet) | |
| Node.js |Azure AD Passport |[npm](https://www.npmjs.com/package/passport-azure-ad) |[GitHub](https://github.com/AzureAD/passport-azure-ad) | [Web API](/active-directory/develop/active-directory-devquickstarts-webapi-nodejs)| |
| .NET |用于 WS 联合身份验证的 OWIN |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) |[CodePlex](http://katanaproject.codeplex.com) |[MVC Web 应用](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet) | |
| .NET |适用于 .NET 4.5 的标识协议扩展 |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Protocol.Extensions) |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |
| .NET |适用于 .NET 4.5 的 JWT 处理程序 |[NuGet](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt) |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |

## <a name="scenarios"></a>方案

以下是可以使用 ADAL 对访问远程资源的客户端进行身份验证的三种常见方案：  

### <a name="authenticating-users-of-a-native-client-application-running-on-a-device"></a>对设备上运行的本机客户端应用程序的用户进行身份验证 

在此方案中，开发人员有一个 WPF 客户端应用程序需要访问由 Azure AD 保护的远程资源（如 Web API）。 他有一个 Azure 订阅，知道如何调用下游 Web API，还知道 Web API 使用哪个 Azure AD 租户。 因此，他可以将身份验证体验完全委托给 ADAL 或显式处理用户凭据，以使用 ADAL 促成通过 Azure AD 的身份验证。 使用 ADAL 可以轻松地对用户进行身份验证、从 Azure AD 获取访问令牌和刷新令牌，并使用访问令牌向 Web API 发出请求。

有关使用 Azure AD 身份验证演示此方案的代码示例，请参阅[本机客户端 WPF 应用程序到 Web API](https://github.com/azureadsamples/nativeclient-dotnet)。

### <a name="authenticating-a-confidential-client-application-running-on-a-web-server"></a>对 Web 服务器上运行的机密客户端应用程序进行身份验证

在此方案中，开发人员在服务器上有一个正在运行的应用程序需要访问由 Azure AD 保护的远程资源（如 Web API）。 他有一个 Azure 订阅，知道如何调用下游服务，还知道 Web API 使用哪个 Azure AD 租户。 因此，他可以显式处理应用程序的凭据，使用 ADAL 促成通过 Azure AD 的身份验证。 使用 ADAL 可以轻松地使用应用程序的客户端凭据从 Azure AD 中检索令牌，并使用该令牌向 Web API 发出请求。 ADAL 还通过缓存访问令牌并在必要时续订，来处理对访问令牌生存期的管理。 有关演示此方案的代码示例，请参阅[守护程序控制台应用程序到 Web API](https://github.com/AzureADSamples/Daemon-DotNet)。

### <a name="authenticating-a-confidential-client-application-running-on-a-server-on-behalf-of-a-user"></a>代表用户对服务器上运行的机密客户端应用程序进行身份验证 

在此方案中，开发人员在服务器上有一个正在运行的应用程序需要访问由 Azure AD 保护的远程资源（如 Web API）。 也需要代表 Azure AD 用户发出请求。 他有一个 Azure 订阅，知道如何调用下游 Web API，还知道服务使用哪个 Azure AD 租户。 用户通过 Web 应用程序的身份验证后，应用程序可以从 Azure AD 获取该用户的授权代码。 然后，Web 应用程序可以使用该授权代码以及与应用程序关联的客户端凭据，代表用户通过 ADAL 从 Azure AD 中获取访问令牌和刷新令牌。 Web 应用程序拥有访问令牌后，就可以调用 Web API，直到该令牌过期。 令牌过期后，Web 应用程序可以使用前面收到的刷新令牌，通过 ADAL 获取新的访问令牌。 有关演示此方案的代码示例，请参阅[本机客户端到 Web API 到 Web API](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof)。

## <a name="see-also"></a>另请参阅

- [Azure Active Directory 开发人员指南](active-directory-developers-guide.md)
- [Azure Active directory 的身份验证方案](active-directory-authentication-scenarios.md)
- [Azure Active Directory 代码示例](active-directory-code-samples.md)

<!--Update_Description: wording update -->
