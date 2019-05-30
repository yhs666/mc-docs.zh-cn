---
title: include 文件
description: include 文件
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
origin.date: 09/17/2018
ms.date: 05/10/2019
ms.author: v-junlch
ms.custom: include file
ms.openlocfilehash: ea2a08dcf8527d785b4371fa5f34aa6f79d82d91
ms.sourcegitcommit: dc0db00da570f0c57f4a1398797fc158a2c423c5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/21/2019
ms.locfileid: "65960905"
---
## <a name="use-the-microsoft-authentication-library-msal-to-sign-in-the-user"></a>使用 Microsoft 身份验证库 (MSAL) 登录用户

1. 将以下代码添加到 `index.html` 文件的 `<script></script>` 标记中：

    ```javascript
    var msalConfig = {
        auth: {
            clientId: "Enter_the_Application_Id_here"
            authority: "https://login.partner.microsoftonline.cn/Enter_the_Tenant_Info_Here"
        },
        cache: {
            cacheLocation: "localStorage",
            storeAuthStateInCookie: true
        }
    };
    
    var graphConfig = {
        graphMeEndpoint: "https://microsoftgraph.chinacloudapi.cn/v1.0/me"
    };

    // this can be used for login or token request, however in more complex situations
    // this can have diverging options
    var request = {
        scopes: ["user.read"]
    };

    var myMSALObj = new Msal.UserAgentApplication(msalConfig);
    // Register Callbacks for redirect flow
    myMSALObj.handleRedirectCallbacks(acquireTokenRedirectCallBack, acquireTokenErrorRedirectCallBack);

    function signIn() {

        myMSALObj.loginPopup(request).then(function (loginResponse) {
            //Login Success
            showWelcomeMessage();
            acquireTokenPopupAndCallMSGraph();
        }).catch(function (error) {
            console.log(error);
        });
    }


    function acquireTokenPopupAndCallMSGraph() {
        //Call acquireTokenSilent (iframe) to obtain a token for Microsoft Graph
        myMSALObj.acquireTokenSilent(request).then(function (tokenResponse) {
            callMSGraph(graphConfig.graphMeEndpoint, tokenResponse.accessToken, graphAPICallback);
        }).catch(function (error) {
            console.log(error.errorCode);
            // Call acquireTokenPopup (popup window) in case of acquireTokenSilent failure due to consent or interaction required ONLY
            if (requiresInteraction(error.errorCode)) {
                myMSALObj.acquireTokenPopup(request).then(function (tokenResponse) {
                    callMSGraph(graphConfig.graphMeEndpoint, tokenResponse.accessToken, graphAPICallback);
                }).catch(function (error) {
                    console.log(error);
                });
            }
        });
    }


    function graphAPICallback(data) {
        document.getElementById("json").innerHTML = JSON.stringify(data, null, 2);
    }


    function showWelcomeMessage() {
        var divWelcome = document.getElementById('WelcomeMessage');
        divWelcome.innerHTML = 'Welcome ' + myMSALObj.getAccount().userName + "to Microsoft Graph API";
        var loginbutton = document.getElementById('SignIn');
        loginbutton.innerHTML = 'Sign Out';
        loginbutton.setAttribute('onclick', 'signOut();');
    }


   function acquireTokenRedirectAndCallMSGraph() {
        //Call acquireTokenSilent (iframe) to obtain a token for Microsoft Graph
        myMSALObj.acquireTokenSilent(request).then(function (tokenResponse) {
            callMSGraph(graphConfig.graphMeEndpoint, tokenResponse.accessToken, graphAPICallback);
        }).catch(function (error) {
            console.log(error);
            //Call acquireTokenRedirect in case of acquireToken Failure
            if (requiresInteraction(error.errorCode)) {
                myMSALObj.acquireTokenRedirect(request);
            }
        });
    }


    function acquireTokenRedirectCallBack(response) {
        if (response.tokenType === "access_token") {
            callMSGraph(graphConfig.graphEndpoint, response.accessToken, graphAPICallback);
        } else {
            console.log("token type is:" + response.tokenType);
        }
    }


    function  acquireTokenErrorRedirectCallBack(error) {
        console.log(error);
    }

    function requiresInteraction(errorCode) {
        if (!errorCode || !errorCode.length) {
            return false;
        }
        return errorCode === "consent_required" ||
            errorCode === "interaction_required" ||
            errorCode === "login_required";
    }


    if (loginType === 'POPUP') {
        if (myMSALObj.getAccount()) {// avoid duplicate code execution on page load in case of iframe and popup window.
            showWelcomeMessage();
            acquireTokenPopupAndCallMSGraph();
        }
    }
    else if (loginType === 'REDIRECT') {
        document.getElementById("SignIn").onclick = function () {
            myMSALObj.loginRedirect(request);
        };
        if (myMSALObj.getAccount() && !myMSALObj.isCallback(window.location.hash)) {// avoid duplicate code execution on page load in case of iframe and popup window.
            showWelcomeMessage();
            acquireTokenRedirectAndCallMSGraph();
        }
    } else {
        console.error('Please set a valid login type');
    }
    ```

<!--start-collapse-->
### <a name="more-information"></a>更多信息

用户首次单击“登录”按钮后，`signIn` 方法将调用 `loginPopup` 以登录用户  。 此方法导致使用 Microsoft 标识平台终结点打开一个弹出窗口，以提示并验证用户的凭据  。 登录成功后，用户被重定向回原始的 index.html 页面，并接收到一个由 `msal.js` 处理的令牌，令牌中包含的信息被缓存  。 该令牌称为 ID令牌，并包含有关用户的基本信息，如用户显示名  。 如果计划将此令牌提供的数据用于任何目的，则需确保此令牌已由后端服务器验证，以保证将令牌颁发给应用程序的有效用户。

本指南生成的 SPA 调用 `acquireTokenSilent` 和/或 `acquireTokenPopup` 来获取用于查询 Microsoft Graph API 以获取用户配置文件信息的访问令牌  。 如果需要验证 ID 令牌的示例，请查看 GitHub 中的[此](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2 "GitHub active-directory-javascript-singlepageapp-dotnet-webapi-v2 示例")示例应用程序 - 该示例使用 ASP.NET Web API 进行令牌验证。

#### <a name="getting-a-user-token-interactively"></a>以交互方式获取用户令牌

初次登录后，你不希望在用户每次需要请求令牌访问资源时都要求其重新认证，因此大部分时间都应使用 *acquireTokenSilent* 来获取令牌。 但有些情况下，需要强制用户与 Microsoft 标识平台终结点交互，部分示例包括：

- 由于密码已过期，用户可能需要重新输入凭据
- 应用程序正在请求访问用户需要同意的资源
- 需要双重身份验证

调用 acquireTokenPopup  会弹出窗口（或调用 acquireTokenRedirect  会将用户重定向到 Microsoft 标识平台终结点），在其中用户需要通过确认凭据、同意必需资源或完成双因素身份验证来与之交互。

#### <a name="getting-a-user-token-silently"></a>以静默方式获取用户令牌

`acquireTokenSilent` 方法处理令牌获取和续订，无需进行任何用户交互。 首次执行 `loginPopup`（或 `loginRedirect`）后，通常使用 `acquireTokenSilent` 方法获取用于访问受保护资源的令牌，以便进行后续调用，因为调用请求或续订令牌都以静默方式进行。
`acquireTokenSilent` 在某些情况下可能会失败，例如用户的密码已过期。 应用程序可以通过两种方式处理此异常：

1. 立即调用 `acquireTokenPopup`，随后出现用户登录提示。 此模式通常用于联机应用程序，此时应用程序中没有可供用户使用的未经身份验证的内容。 本指导式设置生成的示例使用此模式。

2. 应用程序还可以直观地提示用户以交互方式登录，用户可以选择在合适的时间登录，或者应用程序可以稍后重试 `acquireTokenSilent`。 如果用户可以在不中断应用程序的情况下（例如，应用程序中有可用的未经身份验证的内容）使用应用程序的其他功能，则通常会使用此方法。 在这种情况下，用户可以决定何时登录并访问受保护的资源，或何时刷新过期信息。

> [!NOTE]
> 如果使用的浏览器是 Internet Explorer，本快速入门使用 `loginRedirect` 和 `acquireTokenRedirect` 方法，因为存在与 Internet Explorer 浏览器处理弹出窗口有关的[已知问题](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#issues)。
<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-using-the-token-you-just-obtained"></a>使用刚获得的令牌调用 Microsoft Graph API

将以下代码添加到 `index.html` 文件的 `<script></script>` 标记中：

```javascript
function callMSGraph(theUrl, accessToken, callback) {
    var xmlHttp = new XMLHttpRequest();
    xmlHttp.onreadystatechange = function () {
        if (this.readyState == 4 && this.status == 200)
            callback(JSON.parse(this.responseText));
    }
    xmlHttp.open("GET", theUrl, true); // true for asynchronous
    xmlHttp.setRequestHeader('Authorization', 'Bearer ' + accessToken);
    xmlHttp.send();
}
```
<!--start-collapse-->

### <a name="more-information-on-making-a-rest-call-against-a-protected-api"></a>对受保护 API 进行 REST 调用的详细信息

在本指南创建的示例应用程序中，使用 `callMSGraph()` 方法对需要令牌的受保护资源发出 HTTP `GET` 请求，然后将内容返回给调用方。 此方法可在 HTTP 授权标头中添加获取的令牌  。 本指南创建的示例应用程序中的资源是 Microsoft Graph API *me* 终结点，它显示用户个人资料信息。

<!--end-collapse-->

## <a name="add-a-method-to-sign-out-the-user"></a>添加方法以注销用户

将以下代码添加到 `index.html` 文件的 `<script></script>` 标记中：

```javascript
/**
 * Sign out the user
 */
 function signOut() {
     myMSALObj.logout();
 }
```

