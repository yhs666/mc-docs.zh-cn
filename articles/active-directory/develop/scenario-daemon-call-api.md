---
title: 调用 Web API 的守护程序应用（调用 Web API）- Microsoft 标识平台
description: 了解如何构建调用 Web API 的守护程序应用（调用 Web API）
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
origin.date: 05/07/2019
ms.date: 06/20/2019
ms.author: v-junlch
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: f7396dd1ab43d9113b1bfa934ace9f1a52433b97
ms.sourcegitcommit: 9d5fd3184b6a47bf3b60ffdeeee22a08354ca6b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2019
ms.locfileid: "67305929"
---
# <a name="daemon-app-that-calls-web-apis---call-a-web-api-from-the-app"></a>调用 Web API 的守护程序应用 - 从应用调用 Web API

守护程序应用可以从 .NET 守护程序应用程序调用 Web API，也可以调用多个预先批准的 Web API。

## <a name="calling-a-web-api-from-a-net-daemon-application"></a>从 .NET 守护程序应用程序调用 Web API

[!INCLUDE [Call web API in .NET](../../../includes/active-directory-develop-scenarios-call-apis-dotnet.md)]

<!--
More includes will come later for Python and Java
-->

## <a name="calling-several-apis"></a>调用多个 API

对于守护程序应用，需要预先批准调用的 Web API。 守护程序应用不会有任何增量许可（没有用户交互）。 租户管理员需要预先同意应用程序和所有 API 权限。 如果要调用多个 API，则每次调用 `AcquireTokenForClient` 时都需要为每个资源获取一个令牌。 MSAL 将使用应用程序令牌缓存来避免不必要的服务调用。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [守护程序应用 - 移到生产环境](./scenario-daemon-production.md)

