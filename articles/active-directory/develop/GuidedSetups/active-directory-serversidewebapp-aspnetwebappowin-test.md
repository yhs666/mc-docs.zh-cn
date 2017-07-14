---
title: "Azure AD v2 ASP.NET Web 服务器入门 - 测试 | Microsoft Docs"
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
ms.openlocfilehash: daad7dd6d16c9ee32194447a497249913ba98a0f
ms.sourcegitcommit: cc3f528827a8acd109ba793eee023b8c6b2b75e4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
## 测试代码
<a id="test-your-code" class="xliff"></a>

按 `F5` 在 Visual Studio 中运行项目。 浏览器将打开“http://localhost:{端口}”并将你重定向到该网址，其中会显示“使用 Microsoft 登录”按钮。 继续操作并单击它以登录。

准备好测试时，使用工作或学校帐户 (Azure Active Directory) 或个人帐户（live.com、 outlook.com）登录。 

![使用 Microsoft 浏览器窗口登录](./media/active-directory-serversidewebapp-aspnetwebappowin-test/aspnetbrowsersignin.png)

![使用 Microsoft 浏览器窗口登录](./media/active-directory-serversidewebapp-aspnetwebappowin-test/aspnetbrowsersignin2.png)

#### 预期结果
<a id="expected-results" class="xliff"></a>
登录后，用户将重定向到网站主页，该网站是 Microsoft 应用程序注册门户上应用程序注册信息中指定的 HTTPS URL。 此页现在显示“Hello {用户}”、注销链接，以及查看用户声明的链接（即指向之前创建的 Authorize 控制器的链接）。

### 查看用户的声明
<a id="see-users-claims" class="xliff"></a>
选择超链接，查看用户的声明。 你将转到仅供通过身份验证的用户访问的控制器和视图。

#### 预期结果
<a id="expected-results" class="xliff"></a>
 此时应会显示一个表，其中包含已登录用户的基本属性：

| 属性 | 值 | 说明|
|---|---|---|
| 名称 | {用户全名} | 用户的名字和姓氏
|用户名 | <span>user@domain.com</span>| 用于标识已登录用户的用户名
| 使用者| {使用者}|一个唯一标识 Web 上用户登录名的字符串|
| 租户 ID| {Guid}| *guid* 唯一表示用户的 Azure Active Directory 组织。|

此外还会显示一个表格，其中包含身份验证请求中的所有用户声明。 有关 ID 令牌和说明中所有声明的列表，请参阅[此文](/active-directory/develop/active-directory-token-and-claims/)。


### 对访问具有 *[Authorize]* 属性的方法进行测试（可选）
<a id="test-accessing-a-method-that-has-an-authorize-attribute-optional" class="xliff"></a>
此步将测试作为匿名用户对 Authenticated 控制器的访问：<br/>
选择注销用户的链接并完成注销过程。<br/>
现在在浏览器中键入 http://localhost:{端口}/authenticated，访问受 `[Authorize]` 属性保护的控制器

#### 预期结果
<a id="expected-results" class="xliff"></a>
应会出现提示，要求进行身份验证以查看视图。

## 其他信息
<a id="additional-information" class="xliff"></a>

<!--start-collapse-->
### 保护整个网站
<a id="protect-your-entire-web-site" class="xliff"></a>
若要保护整个网站，请在 `Global.asax` `Application_Start` 方法中将 `AuthorizeAttribute` 添加到 `GlobalFilters`：

```csharp
GlobalFilters.Filters.Add(new AuthorizeAttribute());
```
<!--end-collapse-->

<div></div>
<br/>

> [!NOTE]
> 如何限制单个组织中的用户登录你的应用程序

> 默认情况下，个人帐户（包括 outlook.com、live.com 和其他），以及与 Azure Active Directory 集成的任何公司或组织的工作和学校帐户可以登录你的应用程序。 

> 如果要应用程序接受来自唯一的 Azure Active Directory 组织的登录，请将 `Common` 的 *web.config* 中的 `Tenant` 参数替换为组织的租户名，例如 *contoso.partner.onmschina.cn*。 之后将“OWIN 启动类”中的 `ValidateIssuer` 参数更改为 `true`。

> 若要允许来自特定组织列表的用户，将 `ValidateIssuer` 设置为 true，并使用 `ValidIssuers` 参数来指定组织的列表。

> 还可通过 IssuerValidator 参数实现自定义方法来验证颁发者。 有关 `TokenValidationParameters` 的详细信息，请参阅[此](https://msdn.microsoft.com/library/system.identitymodel.tokens.tokenvalidationparameters.aspx) MSDN 文章。


