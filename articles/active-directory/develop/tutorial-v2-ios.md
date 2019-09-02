---
title: iOS - Microsoft 标识平台入门 | Azure
description: iOS (Swift) 应用程序如何使用 Microsoft 标识平台调用需要访问令牌的 API。
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
origin.date: 04/26/2019
ms.date: 07/01/2019
ms.author: v-junlch
ms.reviwer: brandwe
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 426fb833d4747b2b7b87a0228399ca7eee4e924a
ms.sourcegitcommit: 5f85d6fe825db38579684ee1b621d19b22eeff57
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/05/2019
ms.locfileid: "67568706"
---
# <a name="sign-in-users-and-call-the-microsoft-graph-from-an-ios-app"></a>从 iOS 应用将用户登录并调用 Microsoft Graph

本教程介绍如何生成一个 iOS 应用程序并将其集成到 Microsoft 标识平台。 具体而言，此应用会将用户登录，获取用于调用 Microsoft Graph API 的访问令牌，并针对 Microsoft Graph API 发出基本请求。  

完成本指南后，该应用程序将接受任何公司或组织中使用 Azure Active Directory 的工作或学校帐户进行登录。

## <a name="how-this-guide-works"></a>本指南的工作原理

![显示本教程生成的示例应用的工作原理](../../../includes/media/active-directory-develop-guidedsetup-ios-introduction/iosintro.svg)

此示例中的应用会将用户登录并代表他们获取数据。  将通过一个受保护的 API（在本例中为 Microsoft Graph API）访问该数据，该 API 要求授权并且还受 Microsoft 标识平台保护。

更具体地说：

* 应用将通过浏览器或 Microsoft Authenticator 来让用户登录。
* 最终用户将接受应用程序请求的权限。 
* 将为你的应用颁发 Microsoft Graph API 的一个访问令牌。
* 该访问令牌将包括在对 Web API 的 HTTP 请求中。
* 处理 Microsoft Graph 响应。

本示例使用 Microsoft 身份验证库 (MSAL) 来实现身份验证。MSAL 将自动续订令牌，在设备上的其他应用之间提供 SSO，并管理帐户。

## <a name="prerequisites"></a>先决条件

- XCode 版本 10.x 是在本指南中创建的示例所必需的。 可从 [iTunes 网站](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12 "XCode 下载 URL") 下载 XCode。
- Microsoft 身份验证库 ([MSAL.framework](https://github.com/AzureAD/microsoft-authentication-library-for-objc))。 可以使用依赖项管理器，也可以手动添加。 后续部分提供了[详细信息](#add-msal)。 

## <a name="set-up-your-project"></a>设置项目

本教程将创建一个新项目。 若要下载已完成的教程，请[下载代码](https://github.com/Azure-Samples/active-directory-ios-swift-native-v2/archive/master.zip)。

### <a name="create-a-new-project"></a>创建新项目

1. 打开 Xcode，并选择“新建 Xcode 项目”  。
2. 选择“iOS”>“单一视图应用程序”，并选择“下一步”。  
3. 提供产品名称并选择“下一步”  。
4. 选择一个文件夹用于创建应用，然后单击“创建”。 

## <a name="register-your-application"></a>注册应用程序 

如接下来的两部分中所述，可以采用两种方式之一注册应用程序。

### <a name="register-your-app"></a>注册应用

1. 转到 [Azure 门户](https://portal.azure.cn/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview) > 选择 `New registration`。 
2. 输入应用的**名称** > `Register`。 **暂时不要设置重定向 URI**。 
3. 在 `Manage` 部分，转到 `Authentication` > `Add a platform` > `iOS`
    - 输入项目的捆绑 ID。 如果下载了代码，则此 ID 是 `com.microsoft.identitysample.MSALiOS`。
4. 点击 `Configure` 并存储 `MSAL Configuration` 供稍后使用。 

## <a name="add-msal"></a>添加 MSAL

### <a name="get-msal"></a>获取 MSAL

#### <a name="cocoapods"></a>CocoaPods

可以使用 [CocoaPods](https://cocoapods.org/) 来安装 `MSAL`，只需将其添加到目标下的 `Podfile` 即可：

```
use_frameworks!

target '<your-target-here>' do
   pod 'MSAL', '~> 0.4.0'
end
```

#### <a name="carthage"></a>Carthage

可以使用 [Carthage](https://github.com/Carthage/Carthage) 来安装 `MSAL`，只需将其添加到 `Cartfile` 即可： 

```
github "AzureAD/microsoft-authentication-library-for-objc" "master"
```

#### <a name="manually"></a>手动

还可以使用 Git 子模块或签出最新版本，并在应用程序中将其用作框架。

### <a name="add-your-app-registration"></a>添加应用注册

接下来，请将应用注册添加到代码中。 将***客户端/应用程序 ID*** 添加到 `ViewController.swift`：

```swift
let kClientID = "Your_Application_Id_Here"

// Additional variables for Auth and Graph API
let kGraphURI = "https://microsoftgraph.chinacloudapi.cn/v1.0/me/"
let kScopes: [String] = ["https://microsoftgraph.chinacloudapi.cn/user.read"]
let kAuthority = "https://login.partner.microsoftonline.cn/common"
var accessToken = String()
var applicationContext : MSALPublicClientApplication?
```

### <a name="configure-url-schemes"></a>配置 URL 方案

注册 `CFBundleURLSchemes` 来允许回调，以便在用户登录后将其重定向回到应用。

`LSApplicationQueriesSchemes` 允许在应用中使用 Microsoft Authenticator（如果可用）。 

为此，请打开 `Info.plist` 源代码并添加以下内容（请将 ***BUNDLE_ID*** 替换为在 Azure 门户中配置的内容）。

```xml
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>msauth.[BUNDLE_ID]</string>
        </array>
    </dict>
</array>
<key>LSApplicationQueriesSchemes</key>
<array>
    <string>msauth</string>
    <string>msauthv2</string>
</array>
```

### <a name="import-msal"></a>导入 MSAL

在 `ViewController.swift` 和 `AppDelegate.swift` 文件中导入 MSAL 框架：

```swift
import MSAL
```

## <a name="create-your-apps-ui"></a>创建应用的 UI

在本教程中，需要创建：

- “调用图形 API”按钮
- 注销按钮
- 日志记录文本视图

在 `ViewController.swift` 中定义属性和 `initUI()`，如下所示：

```swift

var loggingText: UITextView!
var signOutButton: UIButton!
var callGraphButton: UIButton!

func initUI() {
        // Add call Graph button
        callGraphButton  = UIButton()
        callGraphButton.translatesAutoresizingMaskIntoConstraints = false
        callGraphButton.setTitle("Call Microsoft Graph API", for: .normal)
        callGraphButton.setTitleColor(.blue, for: .normal)
        callGraphButton.addTarget(self, action: #selector(callGraphAPI(_:)), for: .touchUpInside)
        self.view.addSubview(callGraphButton)
        
        callGraphButton.centerXAnchor.constraint(equalTo: view.centerXAnchor).isActive = true
        callGraphButton.topAnchor.constraint(equalTo: view.topAnchor, constant: 50.0).isActive = true
        callGraphButton.widthAnchor.constraint(equalToConstant: 300.0).isActive = true
        callGraphButton.heightAnchor.constraint(equalToConstant: 50.0).isActive = true
        
        // Add sign out button
        signOutButton = UIButton()
        signOutButton.translatesAutoresizingMaskIntoConstraints = false
        signOutButton.setTitle("Sign Out", for: .normal)
        signOutButton.setTitleColor(.blue, for: .normal)
        signOutButton.setTitleColor(.gray, for: .disabled)
        signOutButton.addTarget(self, action: #selector(signOut(_:)), for: .touchUpInside)
        self.view.addSubview(signOutButton)
        
        signOutButton.centerXAnchor.constraint(equalTo: view.centerXAnchor).isActive = true
        signOutButton.topAnchor.constraint(equalTo: callGraphButton.bottomAnchor, constant: 10.0).isActive = true
        signOutButton.widthAnchor.constraint(equalToConstant: 150.0).isActive = true
        signOutButton.heightAnchor.constraint(equalToConstant: 50.0).isActive = true
        signOutButton.isEnabled = false
        
        // Add logging textfield
        loggingText = UITextView()
        loggingText.isUserInteractionEnabled = false
        loggingText.translatesAutoresizingMaskIntoConstraints = false
        
        self.view.addSubview(loggingText)
        
        loggingText.topAnchor.constraint(equalTo: signOutButton.bottomAnchor, constant: 10.0).isActive = true
        loggingText.leftAnchor.constraint(equalTo: self.view.leftAnchor, constant: 10.0).isActive = true
        loggingText.rightAnchor.constraint(equalTo: self.view.rightAnchor, constant: 10.0).isActive = true
        loggingText.bottomAnchor.constraint(equalTo: self.view.bottomAnchor, constant: 10.0).isActive = true
    }
```

接下来，将 `viewDidLoad()` 添加到 ViewController.swift：

```swift
    override func viewDidLoad() {
        super.viewDidLoad()
        initUI()
        do {
            try self.initMSAL()
        } catch let error {
            self.loggingText.text = "Unable to create Application Context \(error)"
        }
    }
```

## <a name="use-msal"></a>使用 MSAL

### <a name="initialize-msal"></a>初始化 MSAL

首先，需要使用应用程序的 `MSALPublicClientConfiguration` 实例创建 `MSALPublicClientApplication`：

```swift
    func initMSAL() throws {
        
        guard let authorityURL = URL(string: kAuthority) else {
            self.loggingText.text = "Unable to create authority URL"
            return
        }
        
        let authority = try MSALAADAuthority(url: authorityURL)
        
        let msalConfiguration = MSALPublicClientApplicationConfig(clientId: kClientID, redirectUri: nil, authority: authority)
        self.applicationContext = try MSALPublicClientApplication(configuration: msalConfiguration)
    }
```

### <a name="handle-the-callback"></a>处理回调

若要在登录后处理回调，请在 `appDelegate` 中添加 `MSALPublicClientApplication.handleMSALResponse`：

```swift
    func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
        
        guard let sourceApplication = options[UIApplication.OpenURLOptionsKey.sourceApplication] as? String else {
            return false
        }
        
        return MSALPublicClientApplication.handleMSALResponse(url, sourceApplication: sourceApplication)
    }
```

#### <a name="acquire-tokens"></a>获取令牌

现在，我们可以通过 MSAL 以交互方式实现应用程序的 UI 处理逻辑和获取令牌。 

MSAL 公开两个主要方法用于获取令牌：`acquireTokenSilently` 和 `acquireTokenInteractively`。  

如果帐户存在，`acquireTokenSilently()` 会尝试将用户登录并获取令牌，而无需任何用户交互。

尝试将用户登录和获取令牌时，`acquireTokenInteractively` 始终会显示 UI；但是，它可能会使用浏览器中的会话 Cookie 或 Microsoft Authenticator 中的帐户来提供交互式 SSO 体验。 

```swift
    @objc func callGraphAPI(_ sender: UIButton) {
        
        guard let currentAccount = self.currentAccount() else {
            // We check to see if we have a current logged in account.
            // If we don't, then we need to sign someone in.
            acquireTokenInteractively()
            return
        }
        
        acquireTokenSilently(currentAccount)
    }

    func currentAccount() -> MSALAccount? {
        
        guard let applicationContext = self.applicationContext else { return nil }
        
        // We retrieve our current account by getting the first account from cache
        // In multi-account applications, account should be retrieved by home account identifier or username instead
        
        do {
            let cachedAccounts = try applicationContext.allAccounts()
            if !cachedAccounts.isEmpty {
                return cachedAccounts.first
            }
        } catch let error as NSError {
            self.updateLogging(text: "Didn't find any accounts in cache: \(error)")
        }
        
        return nil
    }
```

#### <a name="get-a-token-interactively"></a>以交互方式获取令牌

首次获取令牌时，需要创建 `MSALInteractiveTokenParameters` 并调用 `acquireToken`。

1. 创建带有范围的 `MSALInteractiveTokenParameters`。
2. 结合创建的参数调用 `acquireToken`。
3. 相应地处理错误。 有关更多详细信息，请参阅 [iOS 错误处理指南](https://github.com/AzureAD/microsoft-authentication-library-for-objc/wiki/Error-Handling)。
4. 处理成功的案例。 

```swift
    func acquireTokenInteractively() {
   
        guard let applicationContext = self.applicationContext else { return }
     // #1    
        let parameters = MSALInteractiveTokenParameters(scopes: kScopes)
     // #2        
        applicationContext.acquireToken(with: parameters) { (result, error) in
     // #3            
            if let error = error {
                self.updateLogging(text: "Could not acquire token: \(error)")
                return
            }
            guard let result = result else {   
                self.updateLogging(text: "Could not acquire token: No result returned")
                return
            }
     // #4            
            self.accessToken = result.accessToken
            self.updateLogging(text: "Access token is \(self.accessToken)")
            self.updateSignOutButton(enabled: true)
            self.getContentWithToken()
        }
    }
```



#### <a name="get-a-token-silently"></a>以无提示方式获取令牌

若要以无提示方式获取更新的令牌，需要创建 `MSALSilentTokenParameters` 并调用 `acquireTokenSilent`：

```swift
    
    func acquireTokenSilently(_ account : MSALAccount!) {
        guard let applicationContext = self.applicationContext else { return }
        let parameters = MSALSilentTokenParameters(scopes: kScopes, account: account)
        
        applicationContext.acquireTokenSilent(with: parameters) { (result, error) in    
            if let error = error {
                let nsError = error as NSError
                if (nsError.domain == MSALErrorDomain) {
                    if (nsError.code == MSALError.interactionRequired.rawValue) {
                        DispatchQueue.main.async {
                            self.acquireTokenInteractively()
                        }
                        return
                    }
                }
                self.updateLogging(text: "Could not acquire token silently: \(error)")
                return
            }
            
            guard let result = result else {
                self.updateLogging(text: "Could not acquire token: No result returned")
                return
            }
            
            self.accessToken = result.accessToken
            self.updateLogging(text: "Refreshed Access token is \(self.accessToken)")
            self.updateSignOutButton(enabled: true)
            self.getContentWithToken()
        }
    }
```

### <a name="call-the-microsoft-graph-api"></a>调用 Microsoft Graph API 

通过 `self.accessToken` 获取令牌后，应用可以在 HTTP 标头中使用此值向 Microsoft Graph 发出授权请求：

| 标头密钥    | value                 |
| ------------- | --------------------- |
| 授权 | 持有者 <访问令牌> |

将下列内容添加到 `ViewController.swift`：

```swift
    func getContentWithToken() {        
        // Specify the Graph API endpoint
        let url = URL(string: kGraphURI)
        var request = URLRequest(url: url!)
        
        // Set the Authorization header for the request. We use Bearer tokens, so we specify Bearer + the token we got from the result
        request.setValue("Bearer \(self.accessToken)", forHTTPHeaderField: "Authorization")
               
        URLSession.shared.dataTask(with: request) { data, response, error in
               
        if let error = error {
            self.updateLogging(text: "Couldn't get graph result: \(error)")
            return
        }
               
        guard let result = try? JSONSerialization.jsonObject(with: data!, options: []) else {
               
        self.updateLogging(text: "Couldn't deserialize result JSON")
            return
        }
               
        self.updateLogging(text: "Result from Graph: \(result))")
        
        }.resume()
    }
```

详细了解 [Microsoft 图形 API](https://microsoftgraph.chinacloudapi.cn)

### <a name="use-msal-for-sign-out"></a>使用 MSAL 注销

接下来，我们添加注销应用的支持。 

必须注意，使用 MSAL 注销会从此应用程序删除有关用户的所有已知信息，但用户仍在其设备上保持活动会话。 如果用户再次尝试登录，他们可能会看到交互内容，但不一定需要重新输入其凭据，因为设备会话当前是活动的。 

若要添加注销功能，请将以下方法复制到 `ViewController.swift` 中：

```swift 
    @objc func signOut(_ sender: UIButton) {
        
        guard let applicationContext = self.applicationContext else { return }
        
        guard let account = self.currentAccount() else { return }
        
        do {
            
            /**
             Removes all tokens from the cache for this application for the provided account
             
             - account:    The account to remove from the cache
             */
            
            try applicationContext.remove(account)
            self.loggingText.text = ""
            self.signOutButton.isEnabled = false
            
        } catch let error as NSError {
            
            self.updateLogging(text: "Received error signing account out: \(error)")
        }
    }
```

### <a name="enable-token-caching"></a>启用令牌缓存

默认情况下，MSAL 在 iOS 密钥链中缓存应用的令牌。 

若要启用令牌缓存，请转到“Xcode 项目设置”> `Capabilities tab` > `Enable Keychain Sharing` > 单击 `Plus` > 输入 **com.microsoft.adalcache**。

### <a name="add-helper-methods"></a>添加帮助器方法

添加以下帮助器方法以完成本示例：

``` swift
    
    func updateLogging(text : String) {
        
        if Thread.isMainThread {
            self.loggingText.text = text
        } else {
            DispatchQueue.main.async {
                self.loggingText.text = text
            }
        }
    }
    
    func updateSignOutButton(enabled : Bool) {
        if Thread.isMainThread {
            self.signOutButton.isEnabled = enabled
        } else {
            DispatchQueue.main.async {
                self.signOutButton.isEnabled = enabled
            }
        }
    }
```

### <a name="multi-account-applications"></a>多帐户应用程序

此应用是针对单帐户方案生成的。 MSAL 也支持多帐户方案，但是，这需要在应用中执行一些额外的操作。 需要创建 UI，以帮助用户选择他们要对需要令牌的每个操作使用的帐户。 或者，应用可以通过 `getAllAccounts(...)` 方法实现一种启发式算法来选择要使用的帐户。

## <a name="test-your-app"></a>测试应用程序

### <a name="run-locally"></a>在本地运行

如果遵循了上述代码，请尝试生成应用并将其部署到测试设备或仿真器。 现在应该可以登录并获取 Azure AD 的令牌！ 用户登录后，此应用将显示 Microsoft Graph `/me` 终结点返回的数据。 

如果遇到任何问题，请在此文档或 MSAL 库中提出问题并告诉我们。 

### <a name="consent-to-your-app"></a>许可应用

当任何用户首次登录你的应用时，Microsoft 标识会提示他们许可请求的权限。  尽管大多数用户都可以提供许可，但某些 Azure AD 租户已禁用用户许可 - 需要管理员代表所有用户提供许可。  若要支持此方案，请务必在 Azure 门户中注册应用的范围。

## <a name="help-and-support"></a>帮助和支持

在学习本教程或者在使用 Microsoft 标识平台过程中遇到了任何问题？ 请参阅[帮助与支持](/active-directory/develop/developer-support-help-options)


<!-- Update_Description: link update -->