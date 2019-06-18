---
title: 使用 Azure 通知中心向 Swift iOS 应用推送通知 | Azure Docs
description: 了解如何使用 Azure 通知中心向 Swift iOS 应用推送通知
services: notification-hubs
documentationcenter: ios
author: mikeparker104
manager: patniko
editor: spelluru
ms.assetid: 4e3772cf-20db-4b9f-bb74-886adfaaa65d
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 06/24/2019
origin.date: 05/21/2019
ms.author: v-biyu
ms.openlocfilehash: 5de5be2e0525e8219388f6de5a5f0c47d93e6ac2
ms.sourcegitcommit: b3434f6e7ee50a7f84e6b0868f418480aadb1368
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/11/2019
ms.locfileid: "66829513"
---
# <a name="tutorial-push-notifications-to-swift-ios-apps-using-the-notification-hubs-rest-api"></a>教程：使用通知中心 REST API 向 Swift iOS 应用推送通知

> [!div class="op_single_selector"]
> * [Objective-C](notification-hubs-ios-apple-push-notification-apns-get-started.md)
> * [Swift](notification-hubs-ios-push-notifications-swift-apps-get-started.md)

在本教程中，你将在 Azure 通知中心使用 [REST API](https://docs.microsoft.com//rest/api/notificationhubs/) 向基于 Swift 的 iOS 应用程序推送通知。 你将创建一个空白 iOS 应用，它使用 [Apple Push Notification 服务 (APNs)](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html#//apple_ref/doc/uid/TP40008194-CH8-SW1) 接收推送通知。

在本教程中，你将执行以下步骤：

> [!div class="checklist"]
> * 生成证书签名请求文件
> * 请求你的应用推送通知
> * 为应用程序创建配置文件
> * 创建通知中心
> * 使用 APNS 信息配置通知中心
> * 将 iOS 应用连接到通知中心
> * 测试解决方案

## <a name="prerequisites"></a>先决条件
若要按照文中内容操作，需要：

- 如果你不熟悉该服务，请参阅 [Azure 通知中心概述](notification-hubs-push-notification-overview.md)。 
- 了解注册和安装：[注册管理](notification-hubs-push-notification-registration-management.md)
- 一个有效的 [Apple 开发人员帐户](https://developer.apple.com) 
- 一台包含 Xcode 的 Mac 计算机，以及安装在密钥链中的有效开发人员证书
- 可以运行且可用于调试的 iPhone 实物设备（无法使用模拟器来测试推送通知）
- 该 iPhone 实物设备已在 [Apple 门户](https://developer.apple.com)中注册且与你的证书相关联
- 一个可在其中创建和管理资源的 [Azure 订阅](https://portal.azure.cn)

过去没有经验应该也可以遵循本文创建此基本原则示例。 但是，熟悉以下概念会有所帮助：

- 使用 Xcode 和 Swift 生成 iOS 应用
- 配置 iOS 的 [Azure 通知中心](notification-hubs-ios-apple-push-notification-apns-get-started.md)
- 熟悉 [Apple 开发人员门户](https://developer.apple.com)和 [Azure 门户](https://portal.azure.cn)

> [!NOTE]
> 通知中心将配置为仅使用“沙盒”身份验证模式。  不应将此身份验证模式用于生产工作负荷。

[!INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

## <a name="connect-your-ios-app-to-notification-hubs"></a>将 iOS 应用连接到通知中心
在本部分，你将生成要连接到通知中心的 iOS 应用。  

### <a name="create-an-ios-project"></a>创建 iOS 项目
1. 在“Xcode”中，创建新的 iOS 项目并选择“单视图应用程序”模板。  
2. 设置新项目的选项时，请执行以下步骤：
    1. 使用在 **Apple 开发人员门户**中设置“捆绑标识符”时所用的相同“产品名称”（即 **PushDemo**）和“组织标识符”（即 `com.<organization>`）。    
    2. 选择为其设置了“应用 ID”的“团队”。  
    3. 将“语言”设置为“Swift”。  
    4. 单击“下一步” 
3. 创建名为 **SupportingFiles** 的新文件夹。
4. 在 **SupportingFiles** 文件夹下创建名为 **devsettings.plist** 的新 **plist** 文件。 确保将此文件夹添加到 **gitignore** 文件，以便在使用 **Git 存储库**时不会提交该文件。 在生产应用中，你可能会按条件将这些机密设置为自动化生成过程的一部分。 但是，本演练不使用这些机密。
5. 更新 **devsettings.plist** 以包含以下配置条目（使用预配的**通知中心**内你自己的值）：

   | 键                            | 类型                     | Value                     |               
   |--------------------------------| -------------------------| --------------------------|
   | notificationHubKey             | String                   | \<hubKey\>                |
   | notificationHubKeyName         | String                   | \<hubKeyName\>            |
   | notificationHubName            | String                   | \<hubName\>               |
   | notificationHubNamespace       | String                   | \<hubNamespace\>          |
    
   可以在 **Azure 门户**中导航到“通知中心”资源来找到必要的值。  可以在“概述”页中“概要”摘要的右上角找到 **notificationHubName** 和 **notificationHubNamespace** 值。  

   ![通知中心“概要”摘要](./media/notification-hubs-ios-push-notifications-swift-apps-get-started/hub-essentials.png)

   可以通过导航到“访问策略”并单击相应的**访问策略**找到 **notificationHubKeyName** 和 **notificationHubKey** 值。  例如，`DefaultFullSharedAccessSignature`。 然后在“主连接字符串”中，复制 `notificationHubKeyName` 的带有 `SharedAccessKeyName=` 前缀的值，以及 `notificationHubKey` 的带有 `SharedAccessKey=` 前缀的值。  连接字符串应采用以下格式：
    
   ```xml
   Endpoint=sb://<namespace>.servicebus.chinacloudapi.cn/;SharedAccessKeyName=<notificationHubKeyName>;SharedAccessKey=<notificationHubKey>
   ```

   为简单起见，本文将使用 *DefaultFullSharedAccessSignature*，以便也可以使用令牌来发送通知。 但在实践中，如果你只想要接收通知，则 `DefaultListenSharedAccessSignature` 是更好的选择。       
6. 在“项目导航器”下单击“项目名称”，然后单击“常规”选项卡。   
7. 找到“标识”并设置“捆绑标识符”值，使其与在前面步骤中用于“应用 ID”的值（即 `'com.<organization>.PushDemo'`）相匹配。   
8. 找到“签名”，并确保为 **Apple 开发人员帐户**选择适当的**团队**（前面在其下创建了证书和配置文件的团队）。   **Xcode** 应会根据“捆绑标识符”自动下拉相应的**预配配置文件**。  如果未显示新的**预配配置文件**，请尝试刷新“签名标识”的配置文件（“Xcode”>“首选项”>“帐户”>“查看详细信息”）。   依次单击“签名标识”、右下角的“刷新”按钮应会下载配置文件。  
9. 选择“功能”选项卡，并确保已启用“推送通知”。  
10. 打开 **AppDelegate.swift** 文件以实现 *UNUserNotificationCenterDelegate* 协议，并将以下代码添加到类的顶部：
    
    ```swift
    @UIApplicationMain
    class AppDelegate: UIResponder, UIApplicationDelegate, UNUserNotificationCenterDelegate {
        
        ...

        var configValues: NSDictionary?
        var notificationHubNamespace : String?
        var notificationHubName : String?
        var notificationHubKeyName : String?
        var notificationHubKey : String?
        let tags = ["12345"]
        let genericTemplate = PushTemplate(withBody: "{\"aps\":{\"alert\":\"$(message)\"}}")
        
        ...
    }
    ```

    稍后要使用这些成员。 *tags* 和 *genericTemplate* 将在注册过程中使用。 有关标记的详细信息，请参阅[注册的标记](notification-hubs-tags-segment-push-message.md)和[模板注册](notification-hubs-templates-cross-platform-push-messages.md)。
 
11. 在同一文件中的 *didFinishLaunchingWithOptions* 函数中添加以下代码：

    ```swift
    if let path = Bundle.main.path(forResource: "devsettings", ofType: "plist") {
        if let configValues = NSDictionary(contentsOfFile: path) {
            self.notificationHubNamespace = configValues["notificationHubNamespace"] as? String
            self.notificationHubName = configValues["notificationHubName"] as? String
            self.notificationHubKeyName = configValues["notificationHubKeyName"] as? String
            self.notificationHubKey = configValues["notificationHubKey"] as? String
        }
    }
        
    if #available(iOS 10.0, *){
        UNUserNotificationCenter.current().delegate = self
        UNUserNotificationCenter.current().requestAuthorization(options: [.alert, .sound, .badge]) {
            (granted, error) in
                
            if (granted)
            {
                DispatchQueue.main.async {
                    application.registerForRemoteNotifications()
                }
            }
        }
    }

    return true
    ```

    此代码从 **devsettings.plist** 中检索设置值，将 **AppDelegate** 类设置为 *UNUserNotificationCenter* 委托，请求为推送通知授权，然后调用 *registerForRemoteNotifications*。
    
    为简单起见，该代码仅支持 **iOS 10 和更高版本**。 可以按条件使用相应的 API 以及平时采用的方法，添加对较旧 OS 版本的支持。

12. 在同一文件中添加以下函数：

    ```swift
    func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
        let installationId = (UIDevice.current.identifierForVendor?.description)!
        let pushChannel = deviceToken.reduce("", {$0 + String(format: "%02X", $1)})
    }

    func showAlert(withText text : String) {
        let alertController = UIAlertController(title: "PushDemo", message: text, preferredStyle: UIAlertControllerStyle.alert)
        alertController.addAction(UIAlertAction(title: "OK", style: UIAlertActionStyle.default,handler: nil))
        self.window?.rootViewController?.present(alertController, animated: true, completion: nil)
    }
    ```

    该代码使用 *installationId* 和 *pushChannel* 值向**通知中心**注册。 在本例中，你将使用 *UIDevice.current.identifierForVendor* 向我们提供唯一的值用于标识设备，然后设置 *deviceToken* 的格式，以便向我们提供所需的 *pushChannel* 值。 *showAlert* 函数只会显示一些消息文本供演示。

13. 仍在 **AppDelegate.swift** 中，添加 *willPresent* 和 *didReceive* **UNUserNotificationCenterDelegate** 函数，以便在应用分别在前台和后台运行期间，当通知抵达时显示警报
    
    ```swift
    @available(iOS 10.0, *)
    func userNotificationCenter(_ center: UNUserNotificationCenter, 
        willPresent notification: UNNotification, 
        withCompletionHandler completionHandler: @escaping (UNNotificationPresentationOptions) -> Void) {
        showAlert(withText: notification.request.content.body)
    }
    
    @available(iOS 10.0, *)
    func userNotificationCenter(_ center: UNUserNotificationCenter, 
        didReceive response: UNNotificationResponse, 
        withCompletionHandler completionHandler: @escaping () -> Void) {
        showAlert(withText: response.notification.request.content.body)
    }
    ```    

14. 将 print 语句添加到 *didRegisterForRemoteNotificationsWithDeviceToken* 函数的底部，以验证是否正在为 *installationId* 和 *pushChannel* 赋值
15. 为稍后要添加到项目的基础组件创建文件夹（**Models**、**Services** 和 **Utilities**）
16. 在物理设备上检查项目的生成和运行（无法使用模拟器测试推送通知）

### <a name="create-models"></a>创建模型
此步骤将创建一组模型来表示[通知中心 REST API](https://docs.microsoft.com/rest/api/notificationhubs/) 有效负载并存储所需的 **SAS 令牌**数据。


1.  将一个名为 **PushTemplate.swift** 的新 swift 文件添加到 **Models** 中。 此模型提供一个结构用于表示 **DeviceInstallation** 有效负载包含的单个模板的**正文**：
    
    ```swift
    import Foundation

    struct PushTemplate : Codable {
        let body : String
    
        init(withBody body : String) {
            self.body = body
        }
    }
    ```

2. 将一个名为 **DeviceInstallation.swift** 的新 swift 文件添加到 **Models** 文件夹中。 此文件定义一个结构，该结构表示用于创建或更新**设备安装**的有效负载。 将以下代码添加到该文件：
    
    ```swift
    import Foundation

    struct DeviceInstallation : Codable {
        let installationId : String
        let pushChannel : String
        let platform : String = "apns"
        var tags : [String]
        var templates : Dictionary<String, PushTemplate>
    
        init(withInstallationId installationId : String, andPushChannel pushChannel : String) {
            self.installationId = installationId
            self.pushChannel = pushChannel
            self.tags = [String]()
            self.templates = Dictionary<String, PushTemplate>()
        }
    }
    ```

3.  在 **Models** 下添加名为 **TokenData.swift** 的新 swift 文件。 此模型用于存储 **SAS 令牌**及其过期时间

    ```swift
    import Foundation

    struct TokenData {
    
        let token : String
        let expiration : Int

        init(withToken token : String, andTokenExpiration expiration : Int) {
            self.token = token
            self.expiration = expiration
        }
    }
    ```

### <a name="generate-a-sas-token"></a>生成 SAS 令牌
**通知中心**使用的安全基础结构与 **Azure 服务总线**相同。 若要调用 REST API，需要[以编程方式生成](https://docs.microsoft.com/rest/api/eventhub/generate-sas-token)一个可在请求的 **Authorization** 标头中使用的 SAS 令牌。  

生成的令牌采用以下格式： 

```xml
SharedAccessSignature sig=<UrlEncodedSignature>&se=<ExpiryEpoch>&skn=<KeyName>&sr=<UrlEncodedResourceUri>
```

该过程本身涉及到相同的六个主要步骤：  

1.  计算 [UNIX 纪元时间](https://en.wikipedia.org/wiki/Unix_time)格式（自 1970 年 1 月 1 日 00:00:00 UTC 至今的秒数）的过期时间
2.  设置 **ResourceUrl**（表示尝试访问的资源，即 `'https://\<namespace\>.servicebus.windows.net/\<hubName\>'`） 的格式，使其经过百分比编码且采用小写
3.  准备由 `'\<**UrlEncodedResourceUrl**\>\n\<**ExpiryEpoch**\>'` 构成的 **StringToSign**
4.  结合**连接字符串**的 **Key** 部分，使用 **StringToSign** 值的 **HMAC-SHA256**（遵循相应的**授权规则**）计算**签名**（并对其进行 base 64 编码）
5.  设置已经过 base 64 编码的**签名**的格式，使其经过百分比编码
6.  使用 **UrlEncodedSignature**、**ExpiryEpoch**、**KeyName** 和 **UrlEncodedResourceUrl** 值以预期的格式构造**令牌**

有关**共享访问签名**的更全面概述以及 **Azure 服务总线**和**通知中心**如何使用共享访问签名的更全面概述，请参阅 [Azure 服务总线文档](../service-bus-messaging/service-bus-sas.md)。

对于此 Swift 示例，你将借助 Apple 的开源 **CommonCrypto** 库来对签名进行哈希处理。 由于它是一个 C 库，因此无法在 Swift 中直接访问它。 但是，可以使用一个桥接标头来实现此目的。 若要添加并配置桥接标头：

1. 在“Xcode”中，依次转到“文件”、“新建”、“文件”，然后选择“标头文件”并将文件命名为 *BridgingHeader.h*   ****     
2. 编辑该文件以导入 **CommonHMAC**

    ```swift
    #import <CommonCrypto/CommonHMAC.h>

    #ifndef BridgingHeader_h
    #define BridgingHeader_h


    #endif /* BridgingHeader_h */
    ```

3. 更新目标的“生成设置”以引用桥接标头。  打开“生成设置”选项卡并向下滚动到“Swift 编译器”部分。 ****   ****   确保“安装 Objective-C 兼容性标头”选项设置为“是”，并在“Objective-C 桥接标头”选项中输入桥接标头的文件路径，即 `'\<ProjectName\>/BridgingHeader.h'`。 ****   ****    如果找不到这些选项，请确保已选择“所有”视图（而不是“基本”或“自定义”）。   
    
   有许多第三方开源包装器库可以略微简化 **CommonCrypto** 的使用难度。 这超出了本文的范畴。

4. 在 **Utilities** 文件夹下添加名为 **TokenUtility.swift** 的新 Swift 文件，并添加以下代码：

   ```swift
   import Foundation

   struct TokenUtility {    
        typealias Context = UnsafeMutablePointer<CCHmacContext>
    
        static func getSasToken(forResourceUrl resourceUrl : String, withKeyName keyName : String, andKey key : String, andExpiryInSeconds expiryInSeconds : Int = 3600) -> TokenData {
            let expiry = (Int(NSDate().timeIntervalSince1970) + expiryInSeconds).description
            let encodedUrl = urlEncodedString(withString: resourceUrl)
            let stringToSign = "\(encodedUrl)\n\(expiry)"
            let hashValue = sha256HMac(withData: stringToSign.data(using: .utf8)!, andKey: key.data(using: .utf8)!)
            let signature = hashValue.base64EncodedString(options: .init(rawValue: 0))
            let encodedSignature = urlEncodedString(withString: signature)
            let sasToken = "SharedAccessSignature sr=\(encodedUrl)&sig=\(encodedSignature)&se=\(expiry)&skn=\(keyName)"
            let tokenData = TokenData(withToken: sasToken, andTokenExpiration: expiryInSeconds)
        
            return tokenData
        }
    
        private static func sha256HMac(withData data : Data, andKey key : Data) -> Data {
            let context = Context.allocate(capacity: 1)
            CCHmacInit(context, CCHmacAlgorithm(kCCHmacAlgSHA256), (key as NSData).bytes, size_t((key as NSData).length))
            CCHmacUpdate(context, (data as NSData).bytes, (data as NSData).length)
            var hmac = Array<UInt8>(repeating: 0, count: Int(CC_SHA256_DIGEST_LENGTH))
            CCHmacFinal(context, &hmac)
        
            let result = NSData(bytes: hmac, length: hmac.count)
            context.deallocate()
        
            return result as Data
        }
    
        private static func urlEncodedString(withString stringToConvert : String) -> String {
            var encodedString = ""
            let sourceUtf8 = (stringToConvert as NSString).utf8String
            let length = strlen(sourceUtf8)
        
            let charArray: [Character] = [ ".", "-", "_", "~", "a", "z", "A", "Z", "0", "9"]
            let asUInt8Array = String(charArray).utf8.map{ Int8($0) }
        
            for i in 0..<length {
                let currentChar = sourceUtf8![i]
            
                if (currentChar == asUInt8Array[0] || currentChar == asUInt8Array[1] || currentChar == asUInt8Array[2] || currentChar == asUInt8Array[3] ||
                    (currentChar >= asUInt8Array[4] && currentChar <= asUInt8Array[5]) ||
                    (currentChar >= asUInt8Array[6] && currentChar <= asUInt8Array[7]) ||
                    (currentChar >= asUInt8Array[8] && currentChar <= asUInt8Array[9])) {
                    encodedString += String(format:"%c", currentChar)
                }
                else {
                    encodedString += String(format:"%%%02x", currentChar)
                }
            }
        
            return encodedString
        }
    }
   ```
    
    此实用工具封装负责生成 **SAS 令牌**的逻辑。 *getSasToken* 函数协调准备令牌（如前所述）所要执行的高级步骤；在本教程稍后的步骤中，安装服务将调用该函数。 getSasToken 函数调用另外两个函数：用于计算签名的 *sha256HMac*，以及用于对相应 URL 字符串进行编码的 *urlEncodedString*。  之所以需要 *urlEncodedString* 函数，是因为使用内置的 *addingPercentEncoding* 函数无法实现所需的输出。 [Azure 存储 iOS SDK](https://github.com/Azure/azure-storage-ios/blob/master/Lib/Azure%20Storage%20Client%20Library/Azure%20Storage%20Client%20Library/AZSUtil.m) 很好地示范了如何实现这些操作，即使是在 Objective-C 中。 在 [Azure 服务总线文档](../service-bus-messaging/service-bus-sas.md)中可以找到有关 **Azure 服务总线 SAS 令牌**的更多信息。 

### <a name="verify-the-sas-token"></a>验证 SAS 令牌
在客户端中实现安装服务之前，可以使用所选的 HTTP 实用工具检查我们的应用是否在正确生成 **SAS 令牌**。 本文所选的工具是 **Postman**。

利用放在适当位置的 print 语句或断点，记下应用生成的 *installationId* 和 *token* 值。 

遵循以下步骤调用安装 API： 

1. 在“Postman”中打开一个新的选项卡 
2. 将请求设置为 **GET**，并设置以下地址：

    ```xml
    https://<namespace>.servicebus.chinacloudapi.cn/<hubName>/installations/<installationId>?api-version=2015-01
    ```

3. 按如下所示配置请求标头：
    
   | 键           | Value            |
   | ------------- | ---------------- |
   | Content-Type  | application/json |
   | 授权 | \<sasToken\>     |
   | x-ms-version  | 2015-01          |

4. 单击“代码”按钮（“保存”按钮的右上角）。   请求应类似于以下示例：

    ```html
    GET /<hubName>/installations/<installationId>?api-version=2015-01 HTTP/1.1
    Host: <namespace>.servicebus.chinacloudapi.cn
    Content-Type: application/json
    Authorization: <sasToken>
    x-ms-version: 2015-01
    Cache-Control: no-cache
    Postman-Token: <postmanToken>
    ```

5. 单击“发送”按钮 

指定的 *installationId* 暂时不存在注册，但它应会导致生成“404 未找到”响应，而不是“401 未授权”。   此结果确认已接受 **SAS 令牌**。

### <a name="implement-the-installation-service-class"></a>实现安装服务类
接下来，围绕[安装 REST API](https://docs.microsoft.com/rest/api/notificationhubs/create-overwrite-installation) 实现基本包装器。  

在 **Services** 文件夹下添加名为 **NotificationRegistrationService.swift** 的新 Swift 文件，然后将以下代码添加到此文件：

```swift
import Foundation

class NotificationRegistrationService {
    private let tokenizedBaseAddress: String = "https://%@.servicebus.chinacloudapi.cn/%@"
    private let tokenizedCreateOrUpdateInstallationRequest = "/installations/%@?api-version=%@"
    private let session = URLSession(configuration: URLSessionConfiguration.default)
    private let apiVersion = "2015-01"
    private let jsonEncoder = JSONEncoder()
    private let defaultHeaders: [String : String]
    private let installationId : String
    private let pushChannel : String
    private let hubNamespace : String
    private let hubName : String
    private let keyName : String
    private let key : String
    private var tokenData : TokenData? = nil
    
    init(withInstallationId installationId : String,
            andPushChannel pushChannel : String,
            andHubNamespace hubNamespace : String,
            andHubName hubName : String,
            andKeyName keyName : String,
            andKey key: String) {
        self.installationId = installationId
        self.pushChannel = pushChannel
        self.hubNamespace = hubNamespace
        self.hubName = hubName
        self.keyName = keyName
        self.key = key
        self.defaultHeaders = ["Content-Type": "application/json", "x-ms-version": apiVersion]
    }
    
    func register(
        withTags tags : [String]? = nil,
        andTemplates templates : Dictionary<String, PushTemplate>? = nil,
        completeWith completion: ((_ result: Bool) -> ())? = nil) {
        
        var deviceInstallation = DeviceInstallation(withInstallationId: installationId, andPushChannel: pushChannel)
        
        if let tags = tags {
            deviceInstallation.tags = tags
        }
        
        if let templates = templates {
            deviceInstallation.templates = templates
        }
        
        if let deviceInstallationJson = encodeToJson(deviceInstallation) {
            let sasToken = getSasToken()
            let requestUrl = String.init(format: tokenizedCreateOrUpdateInstallationRequest, installationId, apiVersion)
            let apiEndpoint = "\(getBaseAddress())\(requestUrl)"
            
            var request = URLRequest(url: URL(string: apiEndpoint)!)
            request.httpMethod = "PUT"
            
            for (key,value) in self.defaultHeaders {
                request.addValue(value, forHTTPHeaderField: key)
            }
            
            request.addValue(sasToken, forHTTPHeaderField: "Authorization")
            request.httpBody = Data(deviceInstallationJson.utf8)
            
            (self.session.dataTask(with: request) { dat, res, err in
                if let completion = completion {
                        completion(err == nil && (res as! HTTPURLResponse).statusCode == 200)
                }
            }).resume()
        }
    }
    
    private func getBaseAddress() -> String {
        return String.init(format: tokenizedBaseAddress, hubNamespace, hubName)
    }
    
    private func getSasToken() -> String {
        if (tokenData == nil ||
            Date(timeIntervalSince1970: Double((tokenData?.expiration)!)) < Date(timeIntervalSinceNow: -(5 * 60))) {
            self.tokenData = TokenUtility.getSasToken(forResourceUrl: getBaseAddress(), withKeyName: self.keyName, andKey: self.key)
        }

        return (tokenData?.token)!
    }
    
    private func encodeToJson<T : Encodable>(_ object: T) -> String? {
        do {
            let jsonData = try jsonEncoder.encode(object)
            if let jsonString = String(data: jsonData, encoding: .utf8) {
                return jsonString
            } else {
                return nil
            }
        }
        catch {
            return nil
        }
    }
}
```
 
在初始化过程中将提供必要的详细信息。 可以选择性地将标记和模板传入 *register* 函数，以构成**设备安装** JSON 有效负载。  

*register* 函数将调用其他专用函数来准备证书请求。 收到响应后，将调用 completion 来指示是否注册是否成功。  

*getBaseAddress* 函数使用初始化期间提供的**通知中心**参数 **namespace** 和 **name** 来构造请求终结点。  

*getSasToken* 函数将检查当前存储的令牌是否有效，否则它会调用 **TokenUtility** 来生成并存储新的令牌，然后返回一个值。 

最后，*encodeToJson* 会将相应的模型对象转换为 JSON，以用作请求正文的一部分。

### <a name="invoke-the-notification-hubs-rest-api"></a>调用通知中心 REST API
最后一步是将 **AppDelegate** 更新为使用 **NotifiationRegistrationService** 向**通知中心**注册。 

1. 打开 **AppDelegate.swift**，添加一个类级变量用于存储对 **NoficiationRegistrationService** 的引用：

    ```swift
    var registrationService : NotificationRegistrationService?
    ```

2. 在同一文件中，将 *didRegisterForRemoteNotificationsWithDeviceToken* 函数更新为使用必要参数初始化 **NotificationRegistrationService**，然后调用 *register* 函数。

    ```swift
    func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
        let installationId = (UIDevice.current.identifierForVendor?.description)!
        let pushChannel = deviceToken.reduce("", {$0 + String(format: "%02X", $1)})

        // Initialize the Notification Registration Service
        self.registrationService = NotificationRegistrationService(
            withInstallationId: installationId,
            andPushChannel: pushChannel,
            andHubNamespace: notificationHubNamespace!,
            andHubName: notificationHubName!,
            andKeyName: notificationHubKeyName!,
            andKey: notificationHubKey!)
    
        // Call register passing in the tags and template parameters
        self.registrationService!.register(withTags: tags, andTemplates: ["genericTemplate" : self.genericTemplate]) { (result) in
            if !result {
                print("Registration issue")
            } else {
                print("Registered")
            }
        }
    }
    ```

    为简单起见，本文将使用几个 print 语句，以更新输出窗口并在其中提供 *register* 操作的结果。 

3. 现在请生成并运行应用（在物理设备上）。 应会在输出窗口中看到 **“Registered”** 。

## <a name="test-the-solution"></a>测试解决方案
在此阶段，我们的应用已注册到**通知中心**，并可以接收推送通知。 在“Xcode”中，停止调试器并关闭应用（如果它当前正在运行）。  接下来，检查**设备安装**详细信息是否符合预期，以及应用现在是否确实可以接收推送通知。  

### <a name="verify-the-device-installation"></a>验证设备安装
现在，可以发出前面在使用 **Postman** [验证 SAS 令牌](#verify-the-sas-token)时所用的相同请求。 假设 **SAS 令牌**尚未过期，则响应现在应该包含提供的安装详细信息，例如模板和标记。  

```json
{
    "installationId": "<installationId>",
    "pushChannel": "<pushChannel>",
    "pushChannelExpired": false,
    "platform": "apns",
    "expirationTime": "9999-12-31T23:59:59.9999999Z",
    "tags": [
        "12345"
    ],
    "templates": {
        "genericTemplate": {
            "body": "{\"aps\":{\"alert\":\"$(message)\"}}",
            "tags": [
                "genericTemplate"
            ]
        }
    }
}
```

### <a name="send-a-test-notification-azure-portal"></a>发送测试通知（Azure 门户）
测试现在是否可以接收通知的最快方法是在“Azure 门户”中导航到“通知中心”。  

1. 在“Azure 门户”中，导航到“通知中心”上的“概述”选项卡   
2. 单击“概要”摘要左上方的“测试发送”  

    ![通知中心 -“概要”摘要 -“测试发送”按钮](./media/notification-hubs-ios-push-notifications-swift-apps-get-started/hub-essentials-test-send.png)
3. 从“平台”列表中选择“自定义模板”  
4. 在“发送到标记表达式”中输入 **12345**（在安装中已指定此标记） 
5. （可选）编辑 JSON 有效负载中的**消息**
    
    ![通知中心 - 测试发送](./media/notification-hubs-ios-push-notifications-swift-apps-get-started/hub-test-send.png)
6. 单击“发送”，门户应指示是否已成功将通知发送到设备 

    ![通知中心 - 测试发送结果](./media/notification-hubs-ios-push-notifications-swift-apps-get-started/hub-test-send-result.png)

    设备上的“通知中心”内也应会显示一条通知（假设应用不是在前台运行）。  点击该通知会打开应用并显示警报。

    ![已收到通知的示例](./media/notification-hubs-ios-push-notifications-swift-apps-get-started/test-send-notification-received.png)

### <a name="send-a-test-notification-postman"></a>发送测试通知 (Postman)
也可以在 **Postman** 中通过相应的 [REST API](https://docs.microsoft.com/rest/api/notificationhubs/) 发送通知，这可能是更方便的测试方法。 

1. 在“Postman”中打开一个新的选项卡 
2. 将请求设置为 **POST** 并输入以下地址：

    ```xml
    https://<namespace>.servicebus.chinacloudapi.cn/<hubName>/messages/?api-version=2015-01
    ```

3. 按如下所示配置请求标头：
    
   | 键                            | Value                          |
   | ------------------------------ | ------------------------------ |
   | Content-Type                   | application/json;charset=utf-8 |
   | 授权                  | \<sasToken\>                   |
   | ServiceBusNotification-Format  | template                       |
   | Tags                           | "12345"                        |

4. 将请求**正文**配置为使用包含以下 JSON 有效负载的“RAW - JSON (application.json)”： 

    ```json
    {
       "message" : "Hello from Postman!"
    }
    ```

5. 单击“代码”按钮（“保存”按钮的右上角）。   请求应类似于以下示例：

    ```html
    POST /<hubName>/messages/?api-version=2015-01 HTTP/1.1
    Host: <namespace>.servicebus.chinacloudapi.cn
    Content-Type: application/json;charset=utf-8.
    ServiceBusNotification-Format: template
    Tags: "12345"
    Authorization: <sasToken>
    Cache-Control: no-cache
    Postman-Token: <postmanToken>

    {
        "message" : "Hello from Postman!"
    }
    ```

5. 单击“发送”按钮 

随后你应会收到一个成功状态代码，而客户端设备上会收到通知。

## <a name="next-steps"></a>后续步骤
现已通过 [REST API](https://docs.microsoft.com/rest/api/notificationhubs/) 将一个基本的 iOS Swift 应用连接到**通知中心**，并且可以发送和接收通知。 有关详细信息，请参阅以下文章： 

- [Azure 通知中心概述](notification-hubs-push-notification-overview.md)
- [通知中心 REST API](https://docs.microsoft.com/rest/api/notificationhubs/)
- [用于后端操作的通知中心 SDK](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)
- [GitHub 上的通知中心 SDK](https://github.com/Azure/azure-notificationhubs)
- [使用应用程序后端注册](notification-hubs-ios-aspnet-register-user-from-backend-to-push-notification.md)
- [注册管理](notification-hubs-push-notification-registration-management.md)
- [使用标记](notification-hubs-tags-segment-push-message.md) 
- [使用自定义模板](notification-hubs-templates-cross-platform-push-messages.md)
- [使用共享访问签名进行服务总线访问控制](../service-bus-messaging/service-bus-sas.md)
- [以编程方式生成 SAS 令牌](https://docs.microsoft.com/rest/api/eventhub/generate-sas-token)
- [Apple 安全性：通用加密](https://developer.apple.com/security/)
- [UNIX 纪元时间](https://en.wikipedia.org/wiki/Unix_time)
- [HMAC](https://en.wikipedia.org/wiki/HMAC)
