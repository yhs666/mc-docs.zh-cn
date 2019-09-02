---
title: 调用 Web API 的 Web API（移到生产环境）- Microsoft 标识平台
description: 了解如何构建调用下游 Web API 的 Web API（移到生产环境）。
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
ms.openlocfilehash: 74e53572b47eb4d2b37a90f4f1a4b38991a0ee02
ms.sourcegitcommit: 9d5fd3184b6a47bf3b60ffdeeee22a08354ca6b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2019
ms.locfileid: "67305936"
---
# <a name="web-api-that-calls-web-apis---move-to-production"></a>调用 Web API 的 Web API - 移到生产环境

获得了调用 Web API 的令牌后，就可以将应用移到生产环境了。

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="learn-more"></a>了解详细信息

既然你已经了解了如何从自己的 Web API 调用 Web API 的基础知识，你可能会对本教程感兴趣，它介绍了用于构建调用 Web API 的受保护 Web API 的代码。

| 示例 | 平台 | 说明 |
|--------|----------|-------------|
| [active-directory-aspnetcore-webapi-tutorial-v2](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore-v2/tree/master/2.%20Web%20API%20now%20calls%20Microsoft%20Graph) | ASP.NET Core 2.2 Web API, 桌面 (WPF) | 调用 Microsoft Graph 的 ASP.NET Core 2.2 Web API，它本身使用 Microsoft 标识平台 (v2.0) 从 WPF 应用程序调用 |

