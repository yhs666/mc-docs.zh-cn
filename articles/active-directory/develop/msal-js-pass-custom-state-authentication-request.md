---
title: 在身份验证请求中传递自定义状态（适用于 JavaScript 的 Microsoft 身份验证库）| Azure
description: 了解如何使用适用于 JavaScript 的 Microsoft 身份验证库 (MSAL.js) 在身份验证请求中传递自定义状态参数值。
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
origin.date: 05/29/2019
ms.date: 06/17/2019
ms.author: v-junlch
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3f3984099cc986812652a8414288a6ce57941e44
ms.sourcegitcommit: 9d5fd3184b6a47bf3b60ffdeeee22a08354ca6b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2019
ms.locfileid: "67305872"
---
# <a name="pass-custom-state-in-authentication-requests-using-msaljs"></a>使用 MSAL.js 在身份验证请求中传递自定义状态
由 OAuth 2.0 定义的 state  参数包含在身份验证请求中，并在令牌响应中返回，以防止跨站点请求伪造攻击。 默认情况下，适用于 JavaScript 的 Microsoft 身份验证库 (MSAL.js) 在身份验证请求中传递随机生成的唯一 state  参数值。

state 参数也可用于在重定向之前对应用的状态信息进行编码。 可以在应用中传递用户的状态，例如他们所在的页面或视图，作为此参数的输入。 MSAL.js 库允许你将自定义状态作为 state 参数传入 `Request` 对象：

```javascript
// Request type
export type AuthenticationParameters = {
    scopes?: Array<string>;
    extraScopesToConsent?: Array<string>;
    prompt?: string;
    extraQueryParameters?: QPDict;
    claimsRequest?: string;
    authority?: string;
    state?: string;
    correlationId?: string;
    account?: Account;
    sid?: string;
    loginHint?: string;
};
```

例如：

```javascript
let loginRequest = {
    scopes: ["https://microsoftgraph.chinacloudapi.cn/user.read", 
             "https://microsoftgraph.chinacloudapi.cn/user.write"],
    state: “page_url”
}

myMSALObj.loginPopup(loginRequest);
```

发送请求时，传入状态将追加到 MSAL.js 设置的唯一 GUID 中。 返回响应时，MSAL.js 将检查状态是否匹配，然后在 `Response` 对象中返回自定义传入状态作为 `accountState`。

```javascript
export type AuthResponse = {
    uniqueId: string;
    tenantId: string;
    tokenType: string;
    idToken: IdToken;
    accessToken: string;
    scopes: Array<string>;
    expiresOn: Date;
    account: Account;
    accountState: string;
};
```

若要了解详细信息，请阅读[使用 MSAL.js 生成单页应用程序 (SPA)](scenario-spa-overview.md)。

