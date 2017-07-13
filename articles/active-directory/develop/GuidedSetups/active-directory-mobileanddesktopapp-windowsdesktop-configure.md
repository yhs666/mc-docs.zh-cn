---
title: "Azure AD v2 Windows 桌面入门 - 配置 | Microsoft Docs"
description: "Windows 桌面 .NET (XAML) 应用程序如何获取访问令牌并调用 Azure Active Directory v2 终结点保护的 API。 | Azure | Azure"
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
ms.openlocfilehash: 706c2c5fbe897f24b173c96fb1595bd58a857439
ms.sourcegitcommit: cc3f528827a8acd109ba793eee023b8c6b2b75e4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
## 创建应用程序（快速）
<a id="create-an-application-express" class="xliff"></a>
现在需要在 Microsoft 应用程序注册门户中注册应用程序：
1. 通过 [Microsoft 应用程序注册门户](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=windowsDesktop&step=configure)注册应用程序
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
5. 复制应用程序 ID 中的 GUID，返回 Visual Studio，打开 `App.xaml.cs`，然后用刚才注册的应用程序 ID 替换 `your_client_id_here`：

```csharp
private static string ClientId = "your_application_id_here";
```

