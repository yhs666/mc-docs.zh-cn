---
title: 单页应用程序（调用 Web API）-Microsoft 标识平台
description: 了解如何生成单页应用程序（调用 Web API）
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
origin.date: 05/06/2019
ms.date: 06/20/2019
ms.author: v-junlch
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: b2c8f54703dbb01470416b615132694c4ae3b358
ms.sourcegitcommit: 9d5fd3184b6a47bf3b60ffdeeee22a08354ca6b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2019
ms.locfileid: "67305877"
---
# <a name="single-page-application---call-a-web-api"></a>单页应用程序 - 调用 Web API

我们建议你在调用 Web API 之前调用 `acquireTokenSilent` 方法获取或续订访问令牌。 有了令牌后，就可以调用受保护的 Web API 了。

## <a name="call-a-web-api"></a>调用 Web API

### <a name="javascript"></a>Javascript

在 HTTP 请求中使用获得的访问令牌作为持有者令牌来调用任何 Web API，例如 Microsoft Graph API。 例如：

```javascript
    var headers = new Headers();
    var bearer = "Bearer " + access_token;
    headers.append("Authorization", bearer);
    var options = {
         method: "GET",
         headers: headers
    };
    var graphEndpoint = "https://microsoftgraph.chinacloudapi.cn/v1.0/me";

    fetch(graphEndpoint, options)
        .then(function (response) {
             //do something with response
        }
```

### <a name="angular"></a>Angular

正如在[获取令牌部分](scenario-spa-acquire-token.md)中所述，MSAL Angular 包装器利用 HTTP 拦截器自动无提示获取访问令牌，并将它们附加到对 API 的 HTTP 请求中。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [移到生产环境](scenario-spa-production.md)

