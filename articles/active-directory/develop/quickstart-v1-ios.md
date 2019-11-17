---
title: 构建与 Azure AD 集成的 iOS 应用，以便使用 OAuth 2.0 登录 | Microsoft Docs
description: 了解如何从 iOS 应用将用户登录并调用 Microsoft Graph API。
services: active-directory
documentationcenter: ios
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 42303177-9566-48ed-8abb-279fcf1e6ddb
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: quickstart
origin.date: 10/25/2019
ms.date: 11/07/2019
ms.author: v-junlch
ms.custom: aaddev
ms.reviewer: brandwe
ms.collection: M365-identity-device-management
ms.openlocfilehash: b8949903e8b2dca6dd1a09451a9aa911ec12a165
ms.sourcegitcommit: a88cc623ed0f37731cb7cd378febf3de57cf5b45
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/08/2019
ms.locfileid: "73830950"
---
# <a name="quickstart-sign-in-users-and-call-the-microsoft-graph-api-from-an-ios-app-v10"></a>快速入门：从 iOS 应用 (v1.0) 将用户登录并调用 Microsoft Graph API

[Microsoft 标识平台](v2-overview.md)由 Azure Active Directory (Azure AD) 开发人员平台演变而来。 开发人员可以通过它来生成应用程序，从而可以采用所有 Microsoft 标识登录，以及获取令牌来调用 Microsoft Graph 等 Microsoft API 或开发人员生成的 API。

借助 [Microsoft 身份验证库 (MSAL)](msal-overview.md)，开发人员能够从 Microsoft 标识平台终结点获取令牌，以访问受保护的 Web API。 Active Directory 身份验证库 (ADAL) 与适用于开发人员的 Azure AD (v1.0) 终结点集成，其中 MSAL 与 Microsoft 标识平台 (v2.0) 终结点集成。

对于新的 macOS 和 iOS 应用程序，我们建议你使用 Microsoft 标识平台 (v2.0) 和 MSAL 来获取令牌并访问受保护的 Web API：[快速入门：从 iOS 或 macOS 应用将用户登录并调用 Microsoft Graph API](quickstart-v2-ios.md)。

<!-- Update_Description: wording update -->