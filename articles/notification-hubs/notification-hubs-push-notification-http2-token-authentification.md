---
title: "Azure 通知中心内针对 APNS 的基于令牌的 (HTTP/2) 身份验证 | Microsoft Docs"
description: "本主题介绍了如何利用新的针对 APNS 的令牌身份验证"
services: notification-hubs
documentationcenter: .net
author: kpiteira
manager: erikre
editor: 
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
origin.date: 05/17/2017
ms.date: 01/22/2018
ms.author: v-junlch
ms.openlocfilehash: e9388346851b6446bdd50fb3756954bc2f4ad710
ms.sourcegitcommit: c0a9889921cd1ee52d58255a5a2db5e7930c4a4b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/23/2018
---
# <a name="token-based-http2-authentication-for-apns"></a>针对 APNS 的基于令牌的 (HTTP/2) 身份验证
## <a name="overview"></a>概述
本文详细介绍了如何将新的 APNS HTTP/2 协议与基于令牌的身份验证一起使用。

使用新协议的主要好处包括：
-   生成令牌相对来说没有麻烦（与证书相比）
-   不再有过期日期 - 由你控制身份验证令牌及其吊销
-   有效负载现在最大可以达到 4 KB
- 同步反馈
-   采用的是 Apple 的最新协议 - 证书仍然使用二进制协议（已标记为弃用）

可以在几分钟内通过两个步骤完成此新机制的使用：
1.  从 Apple 开发人员帐户门户获取所需的信息
2.  使用新信息配置你的通知中心

通知中心现在全部设置为对 APNS 使用新的身份验证系统。 

请注意，如果是从为 APNS 使用证书凭据迁移而来：
- 则令牌属性会覆盖你在我们的系统中的证书，
- 但你的应用程序将继续无缝地接收通知。

## <a name="obtaining-authentication-information-from-apple"></a>从 Apple 获取身份验证信息
若要启用基于令牌的身份验证，需要你的 Apple 开发人员帐户中的以下属性：
### <a name="key-identifier"></a>密钥标识符
可以从你的 Apple 开发人员帐户中的“密钥”页面中获取密钥标识符

![](./media/notification-hubs-push-notification-http2-token-authentification/obtaining-auth-information-from-apple.png)

### <a name="application-identifier--application-name"></a>应用程序标识符和应用程序名称
可通过开发人员帐户的应用 ID 页获取应用程序名称。 
![](./media/notification-hubs-push-notification-http2-token-authentification/app-name.png)

可通过开发人员帐户的成员身份详细信息页获取应用程序标识符。
![](./media/notification-hubs-push-notification-http2-token-authentification/app-id.png)


### <a name="authentication-token"></a>身份验证令牌
在为应用程序生成令牌后，可以下载身份验证令牌。 有关如何生成此令牌的详细信息，请参阅 [Apple 的开发人员文档](http://help.apple.com/xcode/mac/current/#/dev11b059073?sub=dev1eb5dfe65)。

## <a name="configuring-your-notification-hub-to-use-token-based-authentication"></a>将通知中心配置为使用基于令牌的身份验证
### <a name="configure-via-the-azure-portal"></a>通过 Azure 门户进行配置
若要在门户中启用基于令牌的身份验证，请登录到 Azure 门户并转到你的通知中心 > 通知服务 > APNS 面板。 

这里有一个新属性 -“身份验证模式”。 选择“令牌”将允许你使用所有相关令牌属性更新你的中心。

![](./media/notification-hubs-push-notification-http2-token-authentification/azure-portal-apns-settings.png)

- 输入从 Apple 开发人员帐户检索的属性， 
- 选择应用程序模式（“生产”或“沙盒”） 
- 单击“保存”以更新 APNS 凭据。 

### <a name="configure-via-management-api-rest"></a>通过管理 API (REST) 进行配置

可以使用[管理 API](https://msdn.microsoft.com/library/azure/dn495827.aspx) 将通知中心更新为使用基于令牌的身份验证。
根据你配置的应用程序是“沙盒”应用还是“生产”应用（在 Apple 开发人员帐户中指定的），使用对应的终结点之一：

- 沙盒终结点：[https://api.development.push.apple.com:443/3/device](https://api.development.push.apple.com:443/3/device)
- 生产终结点：[https://api.push.apple.com:443/3/device](https://api.push.apple.com:443/3/device)

> [!IMPORTANT]
> 基于令牌的身份验证需要的 API 版本：**2017-04 或更高版本**。
> 
> 

此处有一个 PUT 请求示例，它使用基于令牌的身份验证更新某个中心：


        PUT https://{namespace}.servicebus.chinacloudapi.cn/{Notification Hub}?api-version=2017-04
          "Properties": {
            "ApnsCredential": {
              "Properties": {
                "KeyId": "<Your Key Id>",
                "Token": "<Your Authentication Token>",
                "AppName": "<Your Application Name>",
                "AppId": "<Your Application Id>",
                "Endpoint":"<Sandbox/Production Endpoint>"
              }
            }
          }
        

### <a name="configure-via-the-net-sdk"></a>通过 .NET SDK 进行配置
可以使用[最新的客户端 SDK](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/1.0.8) 将中心配置为使用基于令牌的身份验证。 

下面是演示了正确用法的代码示例：


        NamespaceManager nm = NamespaceManager.CreateFromConnectionString(_endpoint);
        string token = "YOUR TOKEN HERE";
        string keyId = "YOUR KEY ID HERE";
        string appName = "YOUR APP NAME HERE";
        string appId = "YOUR APP ID HERE";
        NotificationHubDescription desc = new NotificationHubDescription("PATH TO YOUR HUB");
        desc.ApnsCredential = new ApnsCredential(token, keyId, appId, appName);
        desc.ApnsCredential.Endpoint = @"https://api.development.push.apple.com:443/3/device";
        nm.UpdateNotificationHubAsync(desc);

## <a name="reverting-to-using-certificate-based-authentication"></a>恢复为使用基于证书的身份验证
任何时候都可以通过使用前面的任何方法并传递证书而非令牌属性来恢复为使用基于证书的身份验证。 该操作将覆盖以前存储的凭据。

<!-- Update_Description: wording update -->