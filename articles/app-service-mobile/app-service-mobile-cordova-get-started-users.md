---
title: 使用移动应用在 Apache Cordova 中添加身份验证 | Azure
description: 了解如何使用 Azure 应用服务中的移动应用，通过各种标识提供者对 Apache Cordova 应用的用户进行身份验证。 包括 Microsoft 和 Azure Active Directory。
services: app-service\mobile
documentationcenter: javascript
author: elamalani
manager: crdun
editor: ''
ms.assetid: 10dd6dc9-ddf5-423d-8205-00ad74929f0d
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-html
ms.devlang: javascript
ms.topic: article
origin.date: 06/25/2019
ms.date: 09/09/2019
ms.author: v-tawe
ms.openlocfilehash: 66115dd695062f442e35be138e643dc27b07fb76
ms.sourcegitcommit: c21b37e8a5e7f833b374d8260b11e2fb2f451782
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/18/2019
ms.locfileid: "72583585"
---
# <a name="add-authentication-to-your-apache-cordova-app"></a>将身份验证添加到 Apache Cordova 应用
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

> [!NOTE]
> Visual Studio App Center 正在投资于对移动应用开发至关重要的新集成服务。 开发人员可以使用**生成**、**测试**和**分发**服务来设置持续集成和交付管道。 部署应用后，开发人员可以使用**分析**和**诊断**服务监视其应用的状态和使用情况，并使用**推送**服务与用户互动。 开发人员还可以利用 **Auth** 对用户进行身份验证，利用**数据**服务在云中持久保存和同步应用数据。 立即查看 [App Center](https://appcenter.ms/?utm_source=zumo&utm_campaign=app-service-mobile-cordova-get-started-users)。
>

## <a name="summary"></a>摘要
本教程介绍如何使用支持的标识提供者将身份验证添加到 Apache Cordova 上的待办事项列表快速入门项目。 本教程基于 [Get started with Mobile Apps] （移动应用入门）教程，必须先完成该教程。

## <a name="register"></a>注册应用以进行身份验证并配置应用服务
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]



## <a name="permissions"></a>将权限限制给已经过身份验证的用户

[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

现在，可以验证是否已禁用对后端的匿名访问。 在 Visual Studio 中：

* 打开你在完成 [Get started with Mobile Apps]教程后创建的项目。
* 在 **Android 模拟器**中运行应用程序。
* 验证应用启动后显示“意外的连接失败”。

接下来，请更新应用，以便在从移动应用后端请求资源之前对用户进行身份验证。

## <a name="add-authentication"></a>向应用程序添加身份验证
1. 在 **Visual Studio** 中打开项目，然后打开 `www/index.html` 文件进行编辑。
2. 找到 head 节中的 `Content-Security-Policy` 元标记。  将 OAuth 主机添加到允许的源列表。

   | 提供程序 | SDK 提供程序名称 | OAuth 主机 |
   |:--- |:--- |:--- |
   | Azure Active Directory | aad | https://login.chinacloudapi.cn |
   | Microsoft | microsoftaccount | https://login.live.com |

    下面显示了 Content-Security-Policy（针对 Azure Active Directory 实现）的示例：

    ```
    <meta http-equiv="Content-Security-Policy" content="default-src 'self'
        data: gap: https://login.chinacloudapi.cn https://yourapp.chinacloudsites.cn; style-src 'self'">
    ```

    将 `https://login.chinacloudapi.cn` 替换为上表中的 OAuth 主机。  有关 content-security-policy 元标记的详细信息，请参阅 [Content-Security-Policy 文档]。

3. 打开 `www/js/index.js` 文件进行编辑，找到 `onDeviceReady()` 方法，然后在客户端创建代码下添加以下代码：

    ```
    // Login to the service
    client.login('SDK_Provider_Name')
        .then(function () {

            // BEGINNING OF ORIGINAL CODE

            // Create a table reference
            todoItemTable = client.getTable('todoitem');

            // Refresh the todoItems
            refreshDisplay();

            // Wire up the UI Event Handler for the Add Item
            $('#add-item').submit(addItemHandler);
            $('#refresh').on('click', refreshDisplay);

            // END OF ORIGINAL CODE

        }, handleError);
    ```

    此代码替换用于创建表引用和刷新 UI 的现有代码。

    login() 方法开始对提供程序进行身份验证。 login() 方法是返回 JavaScript Promise 的异步函数。  初始化的剩余部分放置在 promise 响应中，因此在 login() 方法完成之前不会执行。

4. 在刚刚添加的代码中，将 `SDK_Provider_Name` 替换为登录提供程序的名称。 例如，对于 Azure Active Directory，请使用 `client.login('aad')`。
5. 运行项目。  项目完成初始化后，应用程序针对所选的身份验证提供程序显示 OAuth 登录页。

## <a name="next-steps"></a>后续步骤
* 了解 [有关 Azure 应用服务身份验证] 的详细信息。

了解如何使用 SDK。

* [Apache Cordova SDK]
* [ASP.NET Server SDK]
* [Node.js Server SDK]

<!-- URLs. -->
[Get started with Mobile Apps]: app-service-mobile-cordova-get-started.md
[Content-Security-Policy 文档]: https://cordova.apache.org/docs/en/latest/guide/appdev/whitelist/index.html
[有关 Azure 应用服务身份验证]: app-service-mobile-auth.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
