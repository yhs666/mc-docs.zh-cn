---
title: "Azure AD v2 Windows 桌面入门 - 配置 | Microsoft Docs"
description: "Windows 桌面 .NET (XAML) 应用程序如何获取访问令牌并调用 Azure Active Directory v2 终结点保护的 API。"
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
ms.openlocfilehash: a7fc4ac2519236d74507476cea2135db03c5e587
ms.sourcegitcommit: cc3f528827a8acd109ba793eee023b8c6b2b75e4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
## 向应用添加应用程序的注册信息
<a id="add-the-applications-registration-information-to-your-app" class="xliff"></a>
在此步骤中，需要将应用程序 ID 添加到项目。

1.  打开 `App.xaml.cs`，并将包含 `ClientId` 的行替换为：

```csharp
private static string ClientId = "[Enter the application Id here]";
```

### 后续操作
<a id="what-is-next" class="xliff"></a>

[测试和验证](active-directory-mobileanddesktopapp-windowsdesktop-test.md)

