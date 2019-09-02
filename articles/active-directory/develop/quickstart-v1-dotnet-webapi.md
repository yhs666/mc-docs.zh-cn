---
title: 使用 Azure AD 构建 .NET Web API 以进行身份验证和授权 | Microsoft Docs
description: 如何生成一个与 Azure AD 集成以进行身份验证和授权的 .NET MVC Web API。
services: active-directory
documentationcenter: .net
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 67e74774-1748-43ea-8130-55275a18320f
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: quickstart
origin.date: 07/17/2019
ms.date: 08/26/2019
ms.author: v-junlch
ms.reviewer: jmprieur, andret
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 742ca6c45dd9dbab3430aad61be04656c5bd36a1
ms.sourcegitcommit: 18a0d2561c8b60819671ca8e4ea8147fe9d41feb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2019
ms.locfileid: "70134299"
---
# <a name="quickstart-build-a-net-web-api-that-integrates-with-azure-ad-for-authentication-and-authorization"></a>快速入门：生成一个与 Azure AD 集成以进行身份验证和授权的 .NET Web API

[Microsoft 标识平台](v2-overview.md)由 Azure Active Directory (Azure AD) 开发人员平台演变而来。 开发人员可以通过它来生成应用程序，从而可以采用所有 Microsoft 标识登录，以及获取令牌来调用 Microsoft Graph 等 Microsoft API 或开发人员生成的 API。

借助 [Microsoft 身份验证库 (MSAL)](msal-overview.md)，开发人员能够从 Microsoft 标识平台终结点获取令牌，以访问受保护的 Web API。 Active Directory 身份验证库 (ADAL) 与适用于开发人员的 Azure AD (v1.0) 终结点集成，其中 MSAL 与 Microsoft 标识平台 (v2.0) 终结点集成。

对于新的 Web API，我们建议你使用 Microsoft 标识平台 (v2.0) 和 MSAL 来获取令牌并访问受保护的 Web API：[快速入门：向 ASP.NET Web 应用添加 Microsoft 登录功能](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore-v2#calling-an-aspnet-core-web-api-from-a-wpf-application-using-azure-ad-v2)

<!-- Update_Description: wording update -->