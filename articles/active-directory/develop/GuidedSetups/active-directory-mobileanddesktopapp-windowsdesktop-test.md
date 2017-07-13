---
title: "Azure AD v2 Windows 桌面入门 - 测试 | Microsoft Docs"
description: "Windows 桌面 .NET (XAML) 应用程序如何通过 Azure Active Directory v2 终结点调用需要访问令牌的 API"
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
ms.openlocfilehash: 39011f23baf13601f4b6a473fdb4bbc4588a2170
ms.sourcegitcommit: cc3f528827a8acd109ba793eee023b8c6b2b75e4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
## 测试代码
<a id="test-your-code" class="xliff"></a>

为了测试应用程序，请按 `F5`，在 Visual Studio 中运行项目。 主窗口应出现：

![示例屏幕截图](./media/active-directory-mobileanddesktopapp-windowsdesktop-test/samplescreenshot.png)

准备好进行测试时，单击“调用 Microsoft Graph API”，并使用 Azure Active Directory（组织帐户）或 Microsoft Account（live.com、outlook.com）帐户登录。 如果是首次登录，会看到要求用户登录的窗口：

![登录](./media/active-directory-mobileanddesktopapp-windowsdesktop-test/signinscreenshot.png)

### 同意
<a id="consent" class="xliff"></a>
首次登录应用程序时，将看到如下所示的许可屏幕，需要在此处进行显式接受：

![许可屏幕](./media/active-directory-mobileanddesktopapp-windowsdesktop-test/consentscreen.png)

### 预期结果
<a id="expected-results" class="xliff"></a>
API 调用结果屏幕上应显示 Microsoft Graph API 调用返回的用户配置文件信息。

“令牌信息”框中还应显示通过 `AcquireTokenAsync` 或 `AcquireTokenSilentAsync` 获得的令牌的相关基本信息：

|属性  |格式  |说明 |
|---------|---------|---------|
|名称 | {用户全名} |用户的名字和姓氏|
|用户名 |<span>user@domain.com</span> |用于标识用户的用户名|
|令牌过期 |{DateTime}         |令牌过期的时间。 MSAL 将在必要时，通过续订令牌延长到期日期|
|访问令牌 |{String}         |发送的令牌字符串，将发送到需要身份验证标头的 HTTP 请求|

<!--start-collapse-->
### 有关作用域和委派权限的详细信息
<a id="more-information-about-scopes-and-delegated-permissions" class="xliff"></a>
Graph API 需要 `user.read` 作用域来读取用户配置文件。 默认情况下，在我们的注册门户上注册的每个应用程序中，都会自动添加此作用域。 某些其他 Graph API 及后端服务器的自定义 API 需要其他作用域。 例如，对于 Graph，需要 `Calendars.Read` 才能列出用户的日历。 若要在应用程序环境中访问用户的日历，则需要添加 `Calendars.Read` 委派应用程序注册的信息，然后将 `Calendars.Read` 添加到 `AcquireTokenAsync` 调用。 增加作用域数量时，用户可能收到接受其他许可的提示。

如果后端 API 不需要作用域（不推荐），则可以将 `ClientId` 用作 `AcquireTokenAsync` 调用中的作用域。
<!--end-collapse-->




