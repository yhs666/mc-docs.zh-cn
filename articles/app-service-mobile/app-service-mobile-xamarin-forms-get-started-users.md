---
title: "Xamarin.Forms 应用中的移动应用身份验证入门"
description: "了解如何使用移动应用通过各种标识提供者（包括 AAD、Google、Facebook、Twitter 和 Microsoft）对 Xamarin Forms 应用的用户进行身份验证。"
services: app-service\mobile
documentationcenter: xamarin
author: panarasi
manager: crdun
editor: 
ms.assetid: 9c55e192-c761-4ff2-8d88-72260e9f6179
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: article
origin.date: 08/07/2017
ms.author: v-yiso
ms.date: 01/29/2018
ms.openlocfilehash: d6c1aec3aba291335d7dabc7595ffd502c6aa7b1
ms.sourcegitcommit: a20b3fbe305d3bb4b6ddfdae98b3e0ab8a79bbfa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2018
---
# <a name="add-authentication-to-your-xamarin-forms-app"></a>向 Xamarin Forms 应用添加身份验证
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

##<a name="overview"></a>概述
本主题演示如何从客户端应用程序对应用服务移动应用的用户进行身份验证。 在本教程中，使用应用服务支持的标识提供者向 Xamarin Forms 快速入门项目添加身份验证。 移动应用成功进行身份验证和授权后，将显示用户 ID 值，该用户能够访问受限制的表数据。

## <a name="prerequisites"></a>先决条件
为了使本教程达到最佳效果，建议先完成[创建 Xamarin Forms 应用][1]教程。 完成此教程后，用户可以获得一个 Xamarin Forms 项目，它是一个多平台 TodoList 应用。

如果不使用下载的快速入门服务器项目，必须将身份验证扩展包添加到项目。 有关服务器扩展包的详细信息，请参阅[使用适用于 Azure 移动应用的 .NET 后端服务器 SDK][2]。

##<a name="register-your-app-for-authentication-and-configure-app-services"></a>注册应用以进行身份验证并配置应用服务

[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="redirecturl"></a>将应用添加到允许的外部重定向 URL

安全身份验证要求为应用定义新的 URL 方案。 此方案允许在完成身份验证过程后，身份验证系统重定向到应用。 在本教程中，我们自始至终使用 URL 方案 _appname_ 。 但是，可以使用任何你所选的 URL 方案。 对于移动应用程序而言，它应是唯一的。 在服务器端启用重定向：

1. 在 [Azure 门户]中，选择应用服务。

2. 单击“身份验证/授权”菜单选项。

3. 在“允许的外部重定向 URL”中，输入 `url_scheme_of_your_app://easyauth.callback`。  此字符串中的 **url_scheme_of_your_app** 是移动应用程序的 URL 方案。  它应该遵循协议的正常 URL 规范（仅使用字母和数字，并以字母开头）。  应记下此字符串，因为在一些地方需要使用此 URL 方案调整移动应用代码。

4. 单击 **“确定”**。

5. 单击“保存” 。

## <a name="restrict-permissions-to-authenticated-users"></a>将权限限制给已经过身份验证的用户
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

## <a name="add-authentication-to-the-portable-class-library"></a>向可移植类库添加身份验证
移动应用使用 [MobileServiceClient][4] 上的 [LoginAsync][3] 扩展方法通过应用服务身份验证登录用户。 此示例使用服务器托管的身份验证流，在应用中显示提供程序的登录界面。 有关详细信息，请参阅[服务器托管的身份验证][5]。 若要在生产应用中提供更好的用户体验，则应考虑改用[客户端托管的身份验证][6]。

若要使用 Xamarin Forms 项目进行身份验证，请在可移植类库中为应用定义 **IAuthenticate** 接口。 然后，将“登录”按钮添加到可移植类库中定义的用户界面，用户单击此按钮即可开始进行身份验证。 身份验证成功后，将从移动应用后端加载数据。

为应用支持的每个平台实现 **IAuthenticate** 接口。

1. 在 Visual Studio 或 Xamarin Studio 中，从名称中包含 Portable 的项目（该项目是可移植类库项目）中打开 App.cs，然后添加以下 `using` 语句：

    ```
    using System.Threading.Tasks;
    ```
2. 在 App.cs 中，在 `App` 类定义前添加以下 `IAuthenticate` 接口定义。

    ```
    public interface IAuthenticate
    {
        Task<bool> Authenticate();
    }
    ```

3. 若要使用平台特定的实现初始化接口，可向 **App** 类添加以下静态成员。

    ```
    public static IAuthenticate Authenticator { get; private set; }

    public static void Init(IAuthenticate authenticator)
    {
        Authenticator = authenticator;
    }
    ```

4. 从可移植类库项目中打开 TodoList.xaml，在 **buttonsPanel** 布局元素中现有按钮之后添加以下 *Button* 元素： 

    ```
      <Button x:Name="loginButton" Text="Sign-in" MinimumHeightRequest="30" 
        Clicked="loginButton_Clicked"/>
    ```

    此按钮通过移动应用后端触发服务器托管的身份验证。

5. 从可移植类库项目中打开 TodoList.xaml.cs，并将以下字段添加到 `TodoList` 类：

    ```
    // Track whether the user has authenticated. 
    bool authenticated = false;
    ```

6. 将 **OnAppearing** 方法替换为以下代码：

    ```
    protected override async void OnAppearing()
    {
        base.OnAppearing();

        // Refresh items only when authenticated.
        if (authenticated == true)
        {
            // Set syncItems to true in order to synchronize the data 
            // on startup when running in offline mode.
            await RefreshItems(true, syncItems: false);

            // Hide the Sign-in button.
            this.loginButton.IsVisible = false;
        }
    }
    ```

    该代码可确保仅在用户经过身份验证后，才从服务刷新数据。

7. 在 TodoList 类中为 Clicked 事件添加以下处理程序：

    ```
    async void loginButton_Clicked(object sender, EventArgs e)
    {
        if (App.Authenticator != null)
            authenticated = await App.Authenticator.Authenticate();

        // Set syncItems to true to synchronize the data on startup when offline is enabled.
        if (authenticated == true)
            await RefreshItems(true, syncItems: false);
    }
    ```

8. 保存更改，重新生成可移植类库项目，并验证没有错误。

##<a name="add-authentication-to-the-android-app"></a>向 Android 应用添加身份验证

本部分演示如何在 Android 应用项目中实现 **IAuthenticate** 接口。 如果不要支持 Android 设备，请跳过本部分。

1. 在 Visual Studio 或 Xamarin Studio 中，右键单击 droid 项目，然后单击“设为启动项目”。
2. 按 F5 在调试器中启动项目，然后验证启动该应用后，是否会引发状态代码为 401（“未授权”）的未处理异常。 因为后端上的访问仅限于授权用户，因此会生成 401 代码。
3. 在 Android 项目中打开 MainActivity.cs，并添加以下 `using` 语句：

    ```
    using Microsoft.WindowsAzure.MobileServices;
    using System.Threading.Tasks;
    ```

4. 更新 MainActivity 类，实现 IAuthenticate 接口，如下所示：

    ```
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity, IAuthenticate
    ```

5. 通过添加 MobileServiceUser 字段和 IAuthenticate 接口所需的 Authenticate 方法，更新 MainActivity 类，如下所示：

    ```
    // Define a authenticated user.
    private MobileServiceUser user;

    public async Task<bool> Authenticate()
    {
        var success = false;
        var message = string.Empty;
        try
        {
                // Sign in with MicrosoftAccount login using a server-managed flow.
            user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(this,
                    MobileServiceAuthenticationProvider.MicrosoftAccount, "{url_scheme_of_your_app}");
            if (user != null)
            {
                message = string.Format("you are now signed-in as {0}.",
                    user.UserId);
                success = true;
            }
        }
        catch (Exception ex)
        {
            message = ex.Message;
        }

        // Display the success or failure message.
        AlertDialog.Builder builder = new AlertDialog.Builder(this);
        builder.SetMessage(message);
        builder.SetTitle("Sign-in result");
        builder.Create().Show();

        return success;
    }
    ```

    如果使用的是 MicrosoftAccount 以外的其他标识提供者，请为 [MobileServiceAuthenticationProvider] 选择不同的值。

6. 在 `<application>` 元素内添加以下 XML，更新 **AndroidManifest.xml** 文件：

```xml
    <activity android:name="com.microsoft.windowsazure.mobileservices.authentication.RedirectUrlActivity" android:launchMode="singleTop" android:noHistory="true">
      <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="{url_scheme_of_your_app}" android:host="easyauth.callback" />
      </intent-filter>
    </activity>
    ```
    Replace `{url_scheme_of_your_app}` with your URL scheme.
7. Add the following code to the **OnCreate** method of the **MainActivity** class before the call to `LoadApplication()`:

        // Initialize the authenticator before loading the app.
        App.Init((IAuthenticate)this);

    This code ensures the authenticator is initialized before the app loads.
2. Rebuild the app, run it, then sign in with the authentication provider you chose and verify you are able to access data as an authenticated user.

##Add authentication to the iOS app

This section shows how to implement the **IAuthenticate** interface in the iOS app project. Skip this section if you are not supporting iOS devices.

1. In Visual Studio or Xamarin Studio, right-click the **iOS** project, then **Set as StartUp Project**.
2. Press F5 to start the project in the debugger, then verify that an unhandled exception with a status code of 401 (Unauthorized) is raised after
   the app starts. The 401 response is produced because access on the backend is restricted to authorized users only.
3. Open AppDelegate.cs in the iOS project and add the following `using` statements:

    ```
    using Microsoft.WindowsAzure.MobileServices;
    using System.Threading.Tasks;
    ```
4. Update the **AppDelegate** class to implement the **IAuthenticate** interface, as follows:

    ```
    public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate, IAuthenticate
    ```
5. Update the **AppDelegate** class by adding a **MobileServiceUser** field and an **Authenticate** method, which is required by the **IAuthenticate**
   interface, as follows:

    ```
    // Define a authenticated user.
    private MobileServiceUser user;

    public async Task<bool> Authenticate()
    {
        var success = false;
        var message = string.Empty;
        try
        {
            // Sign in with MicrosoftAccount login using a server-managed flow.
            if (user == null)
            {
                user = await TodoItemManager.DefaultManager.CurrentClient
                    .LoginAsync(UIApplication.SharedApplication.KeyWindow.RootViewController,
                        MobileServiceAuthenticationProvider.MicrosoftAccount, "{url_scheme_of_your_app}");
                if (user != null)
                {
                    message = string.Format("You are now signed-in as {0}.", user.UserId);
                    success = true;                        
                }
            }        
        }
        catch (Exception ex)
        {
           message = ex.Message;
        }

        // Display the success or failure message.
        UIAlertView avAlert = new UIAlertView("Sign-in result", message, null, "OK", null);
        avAlert.Show();         

        return success;
    }
    ```

    If you are using an identity provider other than MicrosoftAccount, choose a different value for [MobileServiceAuthenticationProvider].

6. Update the **AppDelegate** class by adding the **OpenUrl** method overload, as follows:

        public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
        {
            return TodoItemManager.DefaultManager.CurrentClient.ResumeWithURL(url);
        }

6. Add the following line of code to the **FinishedLaunching** method before the call to `LoadApplication()`:

    ```
    App.Init(this);
    ```

    This code ensures the authenticator is initialized before the app is loaded.

8. Open Info.plist and add a **URL Type**. Set the **Identifier** to a name of your choosing, the **URL Schemes** to the URL scheme for your app, and the **Role** to None.

7. Rebuild the app, run it, then sign in with the authentication provider you chose and verify you are able to access data as an authenticated user.

## Add authentication to Windows 10 (including Phone) app projects
This section shows how to implement the **IAuthenticate** interface in the Windows 10 app projects. The same steps apply for
Universal Windows Platform (UWP) projects, but using the **UWP** project (with noted changes). Skip this section if you are not supporting Windows devices.

1. In Visual Studio, right-click the **UWP** project, then **Set as StartUp Project**.
2. Press F5 to start the project in the debugger, then verify that an unhandled exception with a status code of 401 (Unauthorized) is raised after
   the app starts. The 401 response happens because access on the backend is restricted to authorized users only.
3. Open MainPage.xaml.cs for the Windows app project and add the following `using` statements:

    ```
    using Microsoft.WindowsAzure.MobileServices;
    using System.Threading.Tasks;
    using Windows.UI.Popups;
    using <your_Portable_Class_Library_namespace>;
    ```

    Replace `<your_Portable_Class_Library_namespace>` with the namespace for your portable class library.

4. Update the **MainPage** class to implement the **IAuthenticate** interface, as follows:

    ```
    public sealed partial class MainPage : IAuthenticate
    ```
5. Update the **MainPage** class by adding a **MobileServiceUser** field and an **Authenticate** method, which is required by the **IAuthenticate**
   interface, as follows:

    ```
    // Define a authenticated user.
    private MobileServiceUser user;

    public async Task<bool> Authenticate()
    {
        string message = string.Empty;
        var success = false;

        try
        {
            // Sign in with MicrosoftAccount login using a server-managed flow.
            if (user == null)
            {
                user = await TodoItemManager.DefaultManager.CurrentClient
                        .LoginAsync(MobileServiceAuthenticationProvider.MicrosoftAccount, "{url_scheme_of_your_app}");
                if (user != null)
                {
                    success = true;
                    message = string.Format("You are now signed-in as {0}.", user.UserId);
                }
            }

        }
        catch (Exception ex)
        {
            message = string.Format("Authentication Failed: {0}", ex.Message);
        }

        // Display the success or failure message.
        await new MessageDialog(message, "Sign-in result").ShowAsync();

        return success;
    }
    ```

    If you are using an identity provider other than MicrosoftAccount, choose a different value for [MobileServiceAuthenticationProvider].

6. Add the following line of code in the constructor for the **MainPage** class before the call to `LoadApplication()`:

    ```
    // Initialize the authenticator before loading the app.
    <your_Portable_Class_Library_namespace>.App.Init(this);
    ```

    Replace `<your_Portable_Class_Library_namespace>` with the namespace for your portable class library.

3. If you are using **UWP**, add the following **OnActivated** method override to the **App** class:

       protected override void OnActivated(IActivatedEventArgs args)
       {
           base.OnActivated(args);

            if (args.Kind == ActivationKind.Protocol)
            {
                ProtocolActivatedEventArgs protocolArgs = args as ProtocolActivatedEventArgs;
                TodoItemManager.DefaultManager.CurrentClient.ResumeWithURL(protocolArgs.Uri);
            }

       }

3. Open Package.appxmanifest and add a **Protocol** declaration. Set the **Display name** to a name of your choosing, and the **Name** to the URL scheme for you app.

4. Rebuild the app, run it, then sign in with the authentication provider you chose and verify you are able to access data as an authenticated user.

##Next steps

Now that you completed this basic authentication tutorial, consider continuing on to one of the following tutorials:


* [Enable offline sync for your app](app-service-mobile-xamarin-forms-get-started-offline-data.md)

  Learn how to add offline support your app using a Mobile App backend. Offline sync allows end users to interact with a mobile app - viewing, adding,
  or modifying data - even when there is no network connection.

<!-- Images. -->

<!-- URLs. -->
[1]: ./app-service-mobile-xamarin-forms-get-started.md
[2]: ./app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[3]: https://msdn.microsoft.com/zh-cn/library/azure/dn268341(v=azure.10).aspx
[4]: https://msdn.microsoft.com/zh-cn/library/azure/JJ553674(v=azure.10).aspx
[5]: ./app-service-mobile-dotnet-how-to-use-client-library.md#serverflow
[6]: ./app-service-mobile-dotnet-how-to-use-client-library.md#clientflow
[7]: https://msdn.microsoft.com/zh-cn/library/azure/jj730936(v=azure.10).aspx
[8]: https://portal.azure.cn

<!--Update_Description:add the section of "Add app to the Allowed External Redirect URLs" and update some code-->