---
title: "Azure Active Directory v2.0 终结点 | Microsoft Docs"
description: "使用 Microsoft 帐户和 Azure Active Directory 登录的构建应用简介。"
services: active-directory
documentationcenter: 
author: alexchen2016
manager: digimobile
editor: 
ms.assetid: 2dee579f-fdf6-474b-bc2c-016c931eaa27
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 05/01/2017
ms.date: 06/26/2017
ms.author: v-junlch
ms.custom: aaddev
ms.openlocfilehash: f4f38ba8a4f1638f62af6469ad7e4176131e8e84
ms.sourcegitcommit: a93ff901be297d731c91d77cd7d5c67da432f5d4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/29/2017
---
# <a name="sign-in-microsoft-account--azure-ad-users-in-a-single-app"></a>在单个应用中登录 Microsoft 帐户和 Azure AD 用户
在过去，想要支持个人 Microsoft 帐户和 Azure Active Directory 中的工作帐户的应用开发人员需要集成两个单独的系统。  **Azure AD v2.0 终结点**推出了新的身份验证 API 版本，可让你通过一个简单的集成登录这两种类型的帐户。  使用 v2.0 终结点的应用还可以通过其中一种帐户从 [Microsoft Graph](https://graph.microsoft.io) 使用 REST API。

## <a name="getting-started"></a>入门
从下述列表中选择偏爱的平台，以便使用开源库与框架生成应用。  或者，可以使用我们的 OAuth 2.0 和 OpenID Connect 协议文档来直接发送和接收协议消息，而不必使用身份验证库。

<br />

[!INCLUDE [active-directory-v2-quickstart-table](../../../includes/active-directory-v2-quickstart-table.md)]

## <a name="whats-new"></a>新增功能
此处提供的信息可帮助你了解 v2.0 终结点的定义及其功能限制。

- 了解[使用 v2.0 终结点可以构建哪种类型的应用](active-directory-v2-flows.md)。
- 了解 v2.0 终结点的[限制、局限性和约束](active-directory-v2-limitations.md)。

## <a name="reference"></a>引用
这些链接有助于深入地利用平台：

- [v2.0 协议参考](active-directory-v2-protocols.md)
- [v2.0 令牌参考](active-directory-v2-tokens.md)
- [v2.0 库参考](active-directory-v2-libraries.md)
- [v2.0 终结点中的范围和许可](active-directory-v2-scopes.md)
- [Microsoft Graph](https://graph.microsoft.io)

## <a name="help--support"></a>帮助和支持
可以通过这些地方获取在 Azure Active Directory 上进行开发的帮助。

- [Stack Overflow 的 `azure-active-directory` 和 `adal` 标记](http://stackoverflow.com/questions/tagged/azure-active-directory+or+adal)
- [有关 Azure Active Directory 的反馈](https://feedback.azure.com/forums/169401-azure-active-directory/category/164757-developer-experiences)


> [!NOTE]
> 如果只需从 Azure Active Directory 登录工作和学校帐户，则应从我们的 [Azure AD 开发人员指南](active-directory-developers-guide.md)开始。  V2.0 终结点供显式需要登录 Microsoft 个人帐户的开发人员使用。


