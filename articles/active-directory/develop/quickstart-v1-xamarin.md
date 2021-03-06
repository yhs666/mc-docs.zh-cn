---
title: Azure AD Xamarin 入门 | Microsoft Docs
description: 构建一个与 Azure AD 集成以方便登录，并使用 OAuth 调用受 Azure AD 保护的 API 的 Xamarin 应用程序。
services: active-directory
documentationcenter: xamarin
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 198cd2c3-f7c8-4ec2-b59d-dfdea9fe7d95
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: quickstart
origin.date: 07/17/2019
ms.date: 11/07/2019
ms.author: v-junlch
ms.reviewer: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: b36b449f18110efa851a0781010886bdeac3b749
ms.sourcegitcommit: a88cc623ed0f37731cb7cd378febf3de57cf5b45
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/08/2019
ms.locfileid: "73830945"
---
# <a name="quickstart-build-a-xamarin-app-that-integrates-microsoft-sign-in"></a>快速入门：构建集成 Microsoft 登录的 Xamarin 应用

[Microsoft 标识平台](v2-overview.md)由 Azure Active Directory (Azure AD) 开发人员平台演变而来。 开发人员可以通过它来生成应用程序，从而可以采用所有 Microsoft 标识登录，以及获取令牌来调用 Microsoft Graph 等 Microsoft API 或开发人员生成的 API。

借助 [Microsoft 身份验证库 (MSAL)](msal-overview.md)，开发人员能够从 Microsoft 标识平台终结点获取令牌，以访问受保护的 Web API。 Active Directory 身份验证库 (ADAL) 与适用于开发人员的 Azure AD (v1.0) 终结点集成，其中 MSAL 与 Microsoft 标识平台 (v2.0) 终结点集成。

## <a name="next-steps"></a>后续步骤

对于新的 Xamarin 应用程序，我们建议你使用 Microsoft 标识平台 (v2.0) 和 MSAL 来获取令牌并访问受保护的 Web API。 请参阅[使用 MSAL 将 Microsoft 标识和 Microsoft Graph 集成到 Xamarin Forms 应用](https://github.com/azure-samples/active-directory-xamarin-native-v2#integrate-microsoft-identity-and-the-microsoft-graph-into-a-xamarin-forms-app-using-msal)（没有可选步骤）以开始使用。

<!-- Update_Description: wording update -->