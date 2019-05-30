---
title: include 文件
description: include 文件
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
origin.date: 04/11/2019
ms.date: 05/10/2019
ms.author: v-junlch
ms.custom: include file
ms.openlocfilehash: bfe3fe9fe733b6f698276c3e052a896aeecb3280
ms.sourcegitcommit: 1ebc1e0b99272e62090448d1cd2af385b74ef4b3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/10/2019
ms.locfileid: "65960900"
---
# <a name="sign-in-users-and-call-the-microsoft-graph-api-from-a-javascript-single-page-application-spa"></a>让用户登录并从 JavaScript 单页应用程序 (SPA) 调用 Microsoft 图形 API

本指南演示了 JavaScript 单页面应用程序 (SPA) 如何从 Microsoft 标识平台终结点登录工作和学校帐户，获取访问令牌以及调用 Microsoft 图形 API 或其他需要访问令牌的 API。

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>本指南生成的示例应用的工作原理

![演示本教程生成的示例应用的工作原理](media/active-directory-develop-guidedsetup-javascriptspa-introduction/javascriptspa-intro.svg)

<!--start-collapse-->
### <a name="more-information"></a>更多信息

本指南创建的示例应用程序允许 JavaScript SPA 查询从 Microsoft 标识平台终结点接受令牌的 Microsoft 图形 API 或 Web API。 此方案中，用户登录后请求了访问令牌，并通过授权标头将其添加到 HTTP 请求。 由 Microsoft 身份验证库 (MSAL) 获取和更新令牌。

<!--end-collapse-->

<!--start-collapse-->
### <a name="libraries"></a>库

本指南使用以下库：

|库|说明|
|---|---|
|[msal.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)|适用于 JavaScript 的 Microsoft 身份验证库预览|

> [!NOTE]
> msal.js 面向 Microsoft 标识平台终结点，学校和工作帐户可通过该终结点登录并获取令牌   。 Microsoft 标识平台终结点有[一些限制](../articles/active-directory/develop/azure-ad-endpoint-comparison.md)  。
> 若要了解 v1.0 和 v2.0 终结点之间的差异，请参阅[终结点比较指南](../articles/active-directory/develop/azure-ad-endpoint-comparison.md)。

<!--end-collapse-->

