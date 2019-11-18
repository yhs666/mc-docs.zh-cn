---
title: 用于登录用户的 Web 应用（移到生产环境）- Microsoft 标识平台
description: 了解如何构建用于登录用户的 Web 应用（移到生产环境）
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
origin.date: 09/17/2019
ms.date: 11/07/2019
ms.author: v-junlch
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: d749e660976ce9ebbb67af2362719349fbe9cd76
ms.sourcegitcommit: a88cc623ed0f37731cb7cd378febf3de57cf5b45
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/08/2019
ms.locfileid: "73830908"
---
# <a name="web-app-that-signs-in-users---move-to-production"></a>用于登录用户的 Web 应用 - 移到生产环境

现在你已了解如何获取用于调用 Web API 的令牌，接下来学习如何将其移到生产环境。

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="next-steps"></a>后续步骤

### <a name="calling-web-apis-scenario"></a>调用 Web API 方案

Web 应用登录用户后，它就可以代表已登录用户调用 Web API。 从 Web 应用调用 Web API 是以下方案的目标：

> [!div class="nextstepaction"]
> [用于调用 Web API 的 Web 应用](scenario-web-app-call-api-overview.md)

### <a name="deep-dive---aspnet-core-web-app-tutorial"></a>深入了解 - ASP.NET Core Web 应用教程

了解使用 ASP.NET Core 登录用户的其他方法教程：[ms-identity-aspnetcore-webapp-tutorial](https://github.com/Azure-Samples/ms-identity-aspnetcore-webapp-tutorial)。 此示例是一个循序渐进的教程，提供了 Web 应用的生产就绪代码，包括如何添加使用以下帐户登录：

- 你组织中的帐户，
- 多个组织中的帐户，
- 工作或学校帐户，
- 使用 [Azure AD B2C](/active-directory-b2c/active-directory-b2c-overview) 的帐户，
- 或国家/地区云中的帐户。

### <a name="sample-code---java-web-app"></a>示例代码 - Java Web 应用

通过 GitHub 上的示例详细了解 Java Web 应用：[一个 Java Web 应用程序，该应用程序使用 Microsoft 标识平台登录用户并调用 Microsoft Graph](https://github.com/Azure-Samples/ms-identity-java-webapp)

<!-- Update_Description: wording update -->