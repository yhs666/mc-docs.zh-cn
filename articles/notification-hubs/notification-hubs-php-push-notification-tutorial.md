---
title: 如何结合使用通知中心与 PHP
description: 了解如何从 PHP 后端使用 Azure 通知中心。
services: notification-hubs
documentationcenter: ''
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: 0156f994-96d0-4878-b07b-49b7be4fd856
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: php
ms.devlang: php
ms.topic: article
origin.date: 01/04/2019
ms.date: 10/09/2019
ms.author: v-tawe
ms.openlocfilehash: adfc04b2c276af8eaa94245a887bbac2c09faccb
ms.sourcegitcommit: c9398f89b1bb6ff0051870159faf8d335afedab3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/11/2019
ms.locfileid: "72272614"
---
# <a name="how-to-use-notification-hubs-from-php"></a>如何通过 PHP 使用通知中心

[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

如 MSDN 主题[通知中心 REST API](https://msdn.microsoft.com/library/dn223264.aspx) 中所述，可以使用通知中心 REST 接口从 Java/PHP/Ruby 后端访问所有通知中心功能。

本主题中，我们将向你介绍如何：

* 以 PHP 构建 REST 客户端以获取通知中心功能；
* 请按照选定移动平台的[入门教程](notification-hubs-ios-apple-push-notification-apns-get-started.md)操作，用 PHP 实现后端部分。

## <a name="client-interface"></a>客户端接口

主要的客户端接口可提供 [.NET 通知中心 SDK](https://msdn.microsoft.com/library/jj933431.aspx) 中提供的相同方法，这允许直接翻译当前此站点上提供的所有教程和示例，这些内容均来自 Internet 上的社区。

可以在 [PHP REST 包装器示例] 中找到所有可用代码。

例如，创建客户端：

    ```php
    $hub = new NotificationHub("connection string", "hubname");
    ```

发送 iOS 本机通知：

    ```php
    $notification = new Notification("apple", '{"aps":{"alert": "Hello!"}}');
    $hub->sendNotification($notification, null);
    ```

## <a name="implementation"></a>实现

如果尚未实现，请按照[入门教程]学至最后一节，必须在此过程中实现后端。
此外，如果你希望可以使用 [PHP REST 包装器示例]中的代码，可直接转到[完成本教程](#complete-tutorial)部分。

有关实现完整 REST 包装器的所有详细信息，请访问 [MSDN](https://msdn.microsoft.com/library/dn530746.aspx)。 本部分介绍了访问通知中心 REST 终结点所需的主要步骤的 PHP 实现：

1. 解析连接字符串
2. 生成授权令牌
3. 执行 HTTP 调用

### <a name="parse-the-connection-string"></a>解析连接字符串

下面是实现客户端的主类，其构造函数将解析连接字符串：

    ```php
    class NotificationHub {
        const API_VERSION = "?api-version=2013-10";

        private $endpoint;
        private $hubPath;
        private $sasKeyName;
        private $sasKeyValue;

        function __construct($connectionString, $hubPath) {
            $this->hubPath = $hubPath;

            $this->parseConnectionString($connectionString);
        }

        private function parseConnectionString($connectionString) {
            $parts = explode(";", $connectionString);
            if (sizeof($parts) != 3) {
                throw new Exception("Error parsing connection string: " . $connectionString);
            }

            foreach ($parts as $part) {
                if (strpos($part, "Endpoint") === 0) {
                    $this->endpoint = "https" . substr($part, 11);
                } else if (strpos($part, "SharedAccessKeyName") === 0) {
                    $this->sasKeyName = substr($part, 20);
                } else if (strpos($part, "SharedAccessKey") === 0) {
                    $this->sasKeyValue = substr($part, 16);
                }
            }
        }
    }
    ```

### <a name="create-a-security-token"></a>创建安全令牌

<!-- Refer to the Azure documentation for information about how to [Create a SAS Security Token](https://docs.microsoft.com/previous-versions/azure/reference/dn495627(v=azure.100)#create-sas-security-token). --�

Add the `generateSasToken` method to the `NotificationHub` class to create the token based on the URI of the current request and the credentials extracted from the connection string.

    ```php
    private function generateSasToken($uri) {
        $targetUri = strtolower(rawurlencode(strtolower($uri)));

        $expires = time();
        $expiresInMins = 60;
        $expires = $expires + $expiresInMins * 60;
        $toSign = $targetUri . "\n" . $expires;

        $signature = rawurlencode(base64_encode(hash_hmac('sha256', $toSign, $this->sasKeyValue, TRUE)));

        $token = "SharedAccessSignature sr=" . $targetUri . "&sig="
                    . $signature . "&se=" . $expires . "&skn=" . $this->sasKeyName;

        return $token;
    }
    ```

### Send a notification

First, let us define a class representing a notification.

    ```php
    class Notification {
        public $format;
        public $payload;

        # array with keynames for headers
        # Note: Some headers are mandatory: Windows: X-WNS-Type, WindowsPhone: X-NotificationType
        # Note: For Apple you can set Expiry with header: ServiceBusNotification-ApnsExpiry in W3C DTF, YYYY-MM-DDThh:mmTZD (for example, 1997-07-16T19:20+01:00).
        public $headers;

        function __construct($format, $payload) {
            if (!in_array($format, ["template", "apple", "windows",  "windowsphone"])) {
                throw new Exception('Invalid format: ' . $format);
            }

            $this->format = $format;
            $this->payload = $payload;
        }
    }
    ```

This class is a container for a native notification body, or a set of properties on case of a template notification, and a set of headers, which contains format (native platform or template) and platform-specific properties (like Apple expiration property and WNS headers).

Refer to the [Notification Hubs REST APIs documentation](https://msdn.microsoft.com/library/dn495827.aspx) and the specific notification platforms' formats for all the options available.

Armed with this class, we can now write the send notification methods inside of the `NotificationHub` class:

    ```php
    public function sendNotification($notification, $tagsOrTagExpression="") {
        if (is_array($tagsOrTagExpression)) {
            $tagExpression = implode(" || ", $tagsOrTagExpression);
        } else {
            $tagExpression = $tagsOrTagExpression;
        }

        # build uri
        $uri = $this->endpoint . $this->hubPath . "/messages" . NotificationHub::API_VERSION;
        $ch = curl_init($uri);

        if (in_array($notification->format, ["template", "apple"])) {
            $contentType = "application/json";
        } else {
            $contentType = "application/xml";
        }

        $token = $this->generateSasToken($uri);

        $headers = [
            'Authorization: '.$token,
            'Content-Type: '.$contentType,
            'ServiceBusNotification-Format: '.$notification->format
        ];

        if ("" !== $tagExpression) {
            $headers[] = 'ServiceBusNotification-Tags: '.$tagExpression;
        }

        # add headers for other platforms
        if (is_array($notification->headers)) {
            $headers = array_merge($headers, $notification->headers);
        }

        curl_setopt_array($ch, array(
            CURLOPT_POST => TRUE,
            CURLOPT_RETURNTRANSFER => TRUE,
            CURLOPT_SSL_VERIFYPEER => FALSE,
            CURLOPT_HTTPHEADER => $headers,
            CURLOPT_POSTFIELDS => $notification->payload
        ));

        // Send the request
        $response = curl_exec($ch);

        // Check for errors
        if($response === FALSE){
            throw new Exception(curl_error($ch));
        }

        $info = curl_getinfo($ch);

        if ($info['http_code'] <> 201) {
            throw new Exception('Error sending notification: '. $info['http_code'] . ' msg: ' . $response);
        }
    } 
    ```

The above methods send an HTTP POST request to the `/messages` endpoint of your notification hub, with the correct body and headers to send the notification.

## <a name="complete-tutorial"></a>Complete the tutorial

Now you can complete the Get Started tutorial by sending the notification from a PHP backend.

Initialize your Notification Hubs client (substitute the connection string and hub name as instructed in the [Get started tutorial]):

    ```php
    $hub = new NotificationHub("connection string", "hubname");
    ```

Then add the send code depending on your target mobile platform.

### Windows Store and Windows Phone 8.1 (non-Silverlight)

    ```php
    $toast = '<toast><visual><binding template="ToastText01"><text id="1">Hello from PHP!</text></binding></visual></toast>';
    $notification = new Notification("windows", $toast);
    $notification->headers[] = 'X-WNS-Type: wns/toast';
    $hub->sendNotification($notification, null);
    ```

### iOS

    ```php
    $alert = '{"aps":{"alert":"Hello from PHP!"}}';
    $notification = new Notification("apple", $alert);
    $hub->sendNotification($notification, null);
    ```


### Windows Phone 8.0 and 8.1 Silverlight

    ```php
    $toast = '<?xml version="1.0" encoding="utf-8"?>' .
                '<wp:Notification xmlns:wp="WPNotification">' .
                   '<wp:Toast>' .
                        '<wp:Text1>Hello from PHP!</wp:Text1>' .
                   '</wp:Toast> ' .
                '</wp:Notification>';
    $notification = new Notification("windowsphone", $toast);
    $notification->headers[] = 'X-WindowsPhone-Target : toast';
    $notification->headers[] = 'X-NotificationClass : 2';
    $hub->sendNotification($notification, null);
    ```

### Kindle Fire

    ```php
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("adm", $message);
    $hub->sendNotification($notification, null);
    ```

Running your PHP code should produce now a notification appearing on your target device.

## Next Steps

In this topic, we showed how to create a simple Java REST client for Notification Hubs. From here you can:

* Download the full [PHP REST wrapper sample], which contains all the code above.
* Continue learning about Notification Hubs tagging feature in the [Breaking News tutorial]
* Learn about pushing notifications to individual users in [Notify Users tutorial]

For more information, see also the [PHP Developer Center](/develop/php/).

[PHP REST wrapper sample]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-php
[Get started tutorial]: ./notification-hubs-ios-apple-push-notification-apns-get-started.md
