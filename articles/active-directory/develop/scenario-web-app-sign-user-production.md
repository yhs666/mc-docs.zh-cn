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
origin.date: 05/07/2019
ms.date: 06/20/2019
ms.author: v-junlch
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: f67cb03da23f728b66f95909aaa43e93a931429e
ms.sourcegitcommit: 9d5fd3184b6a47bf3b60ffdeeee22a08354ca6b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2019
ms.locfileid: "67305902"
---
# <a name="web-app-that-signs-in-users---move-to-production"></a>用于登录用户的 Web 应用 - 移到生产环境

现在你已了解如何获取用于调用 Web API 的令牌，接下来学习如何将其移到生产环境。

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="next-steps"></a>后续步骤

### <a name="calling-web-apis-scenario"></a>调用 Web API 方案

Web 应用登录用户后，它就可以代表已登录用户调用 Web API。 从 Web 应用调用 Web API 是以下方案的目标：

> [!div class="nextstepaction"]
> [用于调用 Web API 的 Web 应用](scenario-web-app-call-api-overview.md)

### <a name="deep-dive---web-app-tutorial"></a>深入探讨 - Web 应用教程

了解使用 ASP.NET Core 登录用户的其他方法教程：[ms-identity-aspnetcore-webapp-tutorial](https://github.com/Azure-Samples/ms-identity-aspnetcore-webapp-tutorial)。 这是一个循序渐进的教程，提供了 Web 应用的生产准备代码，包括如何添加登录。

<!--- Removed the diagram as it's already shown in the above link to GitHub

![Tutorial overview](./media/scenarios/aspnetcore-webapp-tutorial.svg)

--->

