---
title: "Azure AD v2 Android 入门 - 配置 | Microsoft Docs"
description: "Android 应用如何从 Azure Active Directory v2 终结点获取访问令牌并调用 Microsoft Graph API 或需要访问令牌的 API"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
origin.date: 05/09/2017
ms.date: 06/21/2017
ms.author: v-junlch
ms.custom: aaddev
ms.openlocfilehash: 3d12ede72c13c5d9e1a03c41add9327006c14297
ms.sourcegitcommit: cc3f528827a8acd109ba793eee023b8c6b2b75e4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
## 创建应用程序（快速）
<a id="create-an-application-express" class="xliff"></a>
现在需要在 Microsoft 应用程序注册门户中注册应用程序：
1. 通过 [Microsoft 应用程序注册门户](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=android&step=configure)注册应用程序
2.  输入应用程序的名称和你的电子邮件
3.  确保选中“引导式设置”选项
4.  按照说明获取应用程序 ID，并将其粘贴到代码中

### 将应用程序注册信息添加到解决方案（高级）
<a id="add-your-application-registration-information-to-your-solution-advanced" class="xliff"></a>
现在需要在 Microsoft 应用程序注册门户中注册应用程序：
1. 转到 [Microsoft 应用程序注册门户](https://apps.dev.microsoft.com/portal/register-app)注册应用程序
2. 输入应用程序的名称和你的电子邮件 
3. 确保取消选中“引导式设置”选项
4. 单击 `Add Platforms`，然后选择 `Native Application` 并点击“保存”
5.  打开 `MainActivity`（在 `app` > `java` > `{host}.{namespace}` 下）
6.  用刚注册的应用程序 ID 替换行（以 `final static String CLIENT_ID` 开头）中的 [在此处输入应用程序 ID]：

```java
final static String CLIENT_ID = "[Enter the application Id here]";
```
<!-- Workaround for Docs conversion bug -->
<ol start="7">
<li>
打开 `AndroidManifest.xml`（在 `app` > `manifests` 下），将以下活动添加到 `manifest\application` 节点。 此操作会注册 `BrowserTabActivity`，可允许 OS 在完成身份验证后继续运行应用程序：
</li>
</ol>

```xml
<!--Intent filter to capture System Browser calling back to our app after Sign In-->
<activity
    android:name="com.microsoft.identity.client.BrowserTabActivity">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        
        <!--Add in your scheme/host from registered redirect URI-->
        <!--By default, the scheme should be similar to 'msal[appId]' -->
        <data android:scheme="msal[Enter the application Id here]"
            android:host="auth" />
    </intent-filter>
</activity>
```
<!-- Workaround for Docs conversion bug -->
<ol start="8">
<li>
在 `BrowserTabActivity` 中，将 `[Enter the application Id here]` 替换为应用程序 ID。
</li>
</ol>

