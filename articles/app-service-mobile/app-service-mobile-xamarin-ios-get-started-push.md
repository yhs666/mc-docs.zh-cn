---
title: 使用 Azure 应用服务向 Xamarin.iOS 应用添加推送通知
description: 了解如何使用 Azure 应用服务将推送通知发送到 Xamarin iOS 应用
services: app-service\mobile
documentationcenter: xamarin
author: elamalani
manager: crdun
editor: ''
ms.assetid: 2921214a-49f8-45e1-a306-a85ce21defca
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: article
origin.date: 06/25/2019
ms.date: 12/16/2019
ms.author: v-tawe
ms.openlocfilehash: 47105c7d3fd93792d53066504cad932def268681
ms.sourcegitcommit: 4a09701b1cbc1d9ccee46d282e592aec26998bff
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75336015"
---
# <a name="add-push-notifications-to-your-xamarinios-app"></a>向 Xamarin.iOS 应用添加推送通知

[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

> [!NOTE]
> Visual Studio App Center 支持以移动应用开发为中心的端到端集成服务。 开发人员可以使用“生成”  、“测试”  和“分发”  服务来设置“持续集成和交付”管道。 部署应用后，开发人员可以使用**分析**和**诊断**服务监视其应用的状态和使用情况，并使用**推送**服务与用户互动。 开发人员还可以利用“身份验证”  对其用户进行身份验证，并使用“数据”  服务在云中保留和同步应用数据。
>
> 如果希望将云服务集成到移动应用程序中，请立即注册到 [App Center](https://appcenter.ms/?utm_source=zumo&utm_medium=Azure&utm_campaign=zumo%20doc) 中。

## <a name="overview"></a>概述

本教程介绍如何向 [Xamarin.iOS 快速入门](app-service-mobile-xamarin-ios-get-started.md)项目添加推送通知，以便每次插入一条记录时，向设备发送一条推送通知。

如果不使用下载的快速入门服务器项目，则需要推送通知扩展包。 有关详细信息，请参阅 [使用适用于 Azure 移动应用的 .NET 后端服务器 SDK](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) 。

## <a name="prerequisites"></a>先决条件

* 完成 [Xamarin.iOS 快速入门](app-service-mobile-xamarin-ios-get-started.md)教程。
* 物理 iOS 设备。 iOS 模拟器不支持推送通知。

## <a name="register-the-app-for-push-notifications-on-apples-developer-portal"></a>在 Apple 的开发人员门户上为推送通知注册应用

[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

## <a name="configure-your-mobile-app-to-send-push-notifications"></a>配置移动应用以发送推送通知

[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

## <a name="update-the-server-project-to-send-push-notifications"></a>更新服务器项目以发送推送通知

[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a name="configure-your-xamarinios-project"></a>配置 Xamarin.iOS 项目

[!INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]

## <a name="add-push-notifications-to-your-app"></a>向应用程序添加推送通知

1. 在 **QSTodoService** 中，添加以下属性使 **AppDelegate** 可以获取移动客户端：

    ```csharp
    public MobileServiceClient GetClient {
        get
        {
            return client;
        }
        private set
        {
            client = value;
        }
    }
    ```

2. 在 **AppDelegate.cs** 文件顶部添加以下 `using` 语句。

    ```csharp
    using Microsoft.WindowsAzure.MobileServices;
    using Newtonsoft.Json.Linq;
    ```

3. 在 **AppDelegate** 中，重写 **FinishedLaunching** 事件：

   ```csharp
    public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
    {
        // registers for push for iOS8
        var settings = UIUserNotificationSettings.GetSettingsForTypes(
            UIUserNotificationType.Alert
            | UIUserNotificationType.Badge
            | UIUserNotificationType.Sound,
            new NSSet());

        UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
        UIApplication.SharedApplication.RegisterForRemoteNotifications();

        return true;
    }
    ```

4. 在同一文件中，重写 `RegisteredForRemoteNotifications` 事件。 在此代码中，将注册一个简单的模板通知，服务器会将此通知发送到所有支持的平台。

    有关使用通知中心的模板的详细信息，请参阅[模板](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md)。

    ```csharp
    public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
    {
        MobileServiceClient client = QSTodoService.DefaultService.GetClient;

        const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

        JObject templates = new JObject();
        templates["genericMessage"] = new JObject
        {
            {"body", templateBodyAPNS}
        };

        // Register for push with your mobile app
        var push = client.GetPush();
        push.RegisterAsync(deviceToken, templates);
    }
    ```

5. 然后，重写 **DidReceivedRemoteNotification** 事件：

   ```csharp
    public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
    {
        NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

        string alert = string.Empty;
        if (aps.ContainsKey(new NSString("alert")))
            alert = (aps [new NSString("alert")] as NSString).ToString();

        //show alert
        if (!string.IsNullOrEmpty(alert))
        {
            UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
            avAlert.Show();
        }
    }
    ```

应用现已更新，可支持推送通知。

## <a name="test"></a>在应用程序中测试推送通知

1. 在支持 iOS 的设备中按“运行”  按钮，以生成项目并启动应用，然后单击“确定”  接受推送通知。

   > [!NOTE]
   > 必须显式接受来自应用程序的推送通知。 此请求只会在首次运行应用程序时出现。

2. 在应用中，键入一项任务，然后单击加号 ( **+** ) 图标。
3. 检查是否已收到通知，并单击“确定”  取消通知。
4. 重复步骤 2 并立即关闭应用，并检查是否已显示通知。

已成功完成本教程。
