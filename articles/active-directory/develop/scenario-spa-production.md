---
title: 单页应用程序（移到生产环境）-Microsoft 标识平台
description: 了解如何生成单页应用程序（移到生产环境）
services: active-directory
documentationcenter: dev-center-name
author: navyasric
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
ms.openlocfilehash: 6a053e538f9153e6c0f58e1ac98eb018754dacb1
ms.sourcegitcommit: 9d5fd3184b6a47bf3b60ffdeeee22a08354ca6b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2019
ms.locfileid: "67305861"
---
# <a name="single-page-application---move-to-production"></a>单页应用程序 - 移到生产环境

现在你已了解如何获取用于调用 Web API 的令牌，接下来学习如何移到生产环境。

## <a name="improve-your-app"></a>改进应用

按照使应用生产准备就绪所需的步骤进行操作。

- 在你的应用程序中[启用日志记录](msal-logging.md)。

## <a name="test-your-integration"></a>测试集成

- 按照 [Microsoft 标识平台集成清单](identity-platform-integration-checklist.md)测试你的集成。

## <a name="next-steps"></a>后续步骤

下面是几个其他示例/教程：

- 深入了解快速入门示例，该示例说明了如何使用 MSAL.js 登录用户并获取用于调用 MS Graph API 的访问令牌

    > [!div class="nextstepaction"]
    > [JavaScript SPA 教程](./tutorial-v2-javascript-spa.md)

- 此示例演示如何使用 MSAL.js 为自己的后端 Web API 获取令牌

     > [!div class="nextstepaction"]
     > [使用 ASP.NET 后端的 SPA](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2)

- 此示例演示如何使用 MSAL.js 登录向 Azure AD B2C 注册的应用

    > [!div class="nextstepaction"]
    > [使用 Azure AD B2C 的 SPA](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp)

