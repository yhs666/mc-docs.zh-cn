---
title: 调用下游 Web API 的 Web API（概述）- Microsoft 标识平台
description: 了解如何构建调用下游 Web API 的 Web API（概述）
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
origin.date: 05/07/2019
ms.date: 06/20/2019
ms.author: v-junlch
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0938ae9e7cac1c48533b27fbe1c06b8054bfec49
ms.sourcegitcommit: 9d5fd3184b6a47bf3b60ffdeeee22a08354ca6b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2019
ms.locfileid: "67305903"
---
# <a name="scenario-web-api-that-calls-web-apis"></a>方案：用于调用 Web API 的 Web API

了解构建调用 Web API 的 Web API 所需的一切。

## <a name="prerequisites"></a>先决条件

此方案（调用 Web API 的受保护 Web API）基于“保护 Web API”方案。 若要详细了解此基本方案，请先参阅[受保护 Web API - 方案](scenario-protected-web-api-overview.md)。

## <a name="overview"></a>概述

- 客户端（Web、桌面、移动或单页应用程序）- 未在下图中表示 - 调用受保护 Web API 并在其“Authorization”Http 标头中提供 JWT 持有者令牌。
- 受保护 Web API 对令牌进行验证，并使用 MSAL `AcquireTokenOnBehalfOf` 方法从 Azure AD 中请求另一令牌，这样它就能自行代表用户调用另一个 Web API（名为下游 Web API）。
- 受保护的 Web API 使用此令牌调用下游 API。 它也可以稍后调用 `AcquireTokenSilent` 来请求其他下游 API 的令牌（但仍代表同一用户）。 `AcquireTokenSilent` 在需要时刷新令牌。

![调用 Web API 的 Web API](./media/scenarios/web-api.svg)

## <a name="specifics"></a>详情

与 API 权限相关的应用注册部分很经典。 应用程序配置涉及使用 OAuth 2.0 代理流将 JWT 持有者令牌与下游 API 的令牌进行交换。 此令牌会添加到令牌缓存中，在缓存中可供 Web API 的控制器使用，并可以无提示方式获取令牌来调用下游 API。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [应用注册](scenario-web-api-call-api-app-registration.md)

