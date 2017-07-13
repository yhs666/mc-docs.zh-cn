---
title: "Azure AD v2 ASP.NET Web 服务器入门 - 简介 | Microsoft Docs"
description: "通过基于传统 Web 浏览器的使用 OpenID Connect 标准的应用程序，对 ASP.NET 解决方案实现 Microsoft 登录"
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
ms.openlocfilehash: ffb5a3748ef71a13399636b5a07dc9ce38d0d80e
ms.sourcegitcommit: cc3f528827a8acd109ba793eee023b8c6b2b75e4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
# 向 ASP.NET Web 应用添加 Microsoft 登录
<a id="add-sign-in-with-microsoft-to-an-aspnet-web-app" class="xliff"></a>

本指南演示如何通过基于传统 Web 浏览器的使用 OpenID Connect 的应用程序，使用 ASP.NET MVC 解决方案实现 Microsoft 登录。 

在本指南结束时，应用程序将能接受使用个人帐户（包括 outlook.com、live.com 和其他帐户）以及与 Azure Active Directory 集成的任何公司或组织的工作和学校帐户进行登录。 

> 本指南需要 Visual Studio 2015 Update 3 或 Visual Studio 2017。  尚未安装？  [免费下载 Visual Studio 2017](https://www.visualstudio.com/downloads/)

## 本指南的工作原理
<a id="how-this-guide-works" class="xliff"></a>

![本指南的工作原理](./media/active-directory-serversidewebapp-aspnetwebappowin-intro/aspnetbrowsergeneral.png)

本指南基于这样的情景：浏览器访问 ASP.NET 网站，要求用户通过登录按钮进行身份验证。 在此方案中，呈现网页的大部分工作在服务器端完成。

## 库
<a id="libraries" class="xliff"></a>

本指南使用以下库：

|库|说明|
|---|---|
|[Microsoft.Owin.Security.OpenIdConnect](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/)|让应用程序可使用 OpenIdConnect 进行身份验证的中间件|
|[Microsoft.Owin.Security.Cookies](https://www.nuget.org/packages/Microsoft.Owin.Security.Cookies)|让应用程序可使用 Cookie 维持用户会话的中间件|
|[Microsoft.Owin.Host.SystemWeb](https://www.nuget.org/packages/Microsoft.Owin.Host.SystemWeb)|让基于 OWIN 的应用程序可使用 ASP.NET 请求管道在 IIS 上运行|


