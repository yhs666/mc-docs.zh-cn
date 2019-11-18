---
title: 用于登录用户的 Web 应用（应用注册）- Microsoft 标识平台
description: 了解如何构建用于登录用户的 Web 应用（应用注册）
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
origin.date: 10/30/2019
ms.date: 11/07/2019
ms.author: v-junlch
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: dc9c24bb5e2308198f7c3be626f7e9040534e88c
ms.sourcegitcommit: a88cc623ed0f37731cb7cd378febf3de57cf5b45
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/08/2019
ms.locfileid: "73830909"
---
# <a name="web-app-that-signs-in-users---app-registration"></a>用于登录用户的 Web 应用 - 应用注册

此页介绍用于登录用户的 Web 应用的应用注册详细信息。

若要注册应用程序，可以使用：

- [Web 应用快速入门](#register-an-app-using-the-quickstarts) - 除了提供创建应用程序的第一手体验，Azure 门户中的快速入门还包含名为“为我进行此更改”的按钮。  可以使用此按钮设置所需属性，对现有应用也可以这样做。 需根据自己的情况调整这些属性的值。 具体说来，应用的 Web API URL 可能会不同于建议的默认值，后者还会影响注销 URI。
- 用于[手动注册应用程序](#register-an-app-using-azure-portal)的 Azure 门户
- PowerShell 和命令行工具

## <a name="register-an-app-using-the-quickstarts"></a>按照快速入门注册应用

如果导航到此链接，则可通过创建 Web 应用程序来创建启动过程：

- [ASP.NET Core](https://portal.azure.cn/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview)
- [ASP.NET](https://portal.azure.cn/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/AspNetWebAppQuickstartPage/sourceType/docs)

## <a name="register-an-app-using-azure-portal"></a>使用 Azure 门户注册应用

> [!NOTE]
> 要使用的门户取决于应用程序是在国家/地区云还是在主权云中运行。 有关详细信息，请参阅[国家/地区云](./authentication-national-cloud.md#app-registration-endpoints)


1. 使用工作或学校帐户登录到 [Azure 门户](https://portal.azure.cn)。 或者，登录到所选的国家/地区云 Azure 门户。
1. 如果你的帐户有权访问多个租户，请在右上角选择该帐户，并将门户会话设置为所需的 Azure AD 租户。
1. 在左侧导航窗格中，选择“Azure Active Directory”服务  ，然后选择“应用注册” > “新建注册”。  

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

4. “注册应用程序”页出现后，请输入应用程序的注册信息： 
   1. 为应用程序选择支持的帐户类型（请参阅[支持的帐户类型](./v2-supported-account-types.md)）
   1. 在“名称”  部分输入一个会显示给应用用户的有意义的应用程序名称，例如 `AspNetCore-WebApp`。
   1. 在**重定向 URI** 中，添加将在成功进行身份验证后接受返回的令牌响应的应用程序类型和 URI 目标。 例如，`https://localhost:44321/`。  选择“注册”  。
1. 选择“身份验证”菜单，然后添加以下信息  ：
   1. 在“回复 URL”  中，添加类型为“Web”的 `https://localhost:44321/signin-oidc`。
   1. 在“高级设置”  部分，将“注销 URL”设置为 `https://localhost:44321/signout-oidc`。 
   1. 在“隐式授权”下，勾选“ID 令牌”。  
   1. 选择“保存”  。

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

4. 出现“注册应用程序”页后，请输入应用程序的注册信息： 
   1. 为应用程序选择支持的帐户类型（请参阅[支持的帐户类型](./v2-supported-account-types.md)）
   - 在“名称”  部分输入一个会显示给应用用户的有意义的应用程序名称，例如 `MailApp-openidconnect-v2`。
   - 在“重定向 URI (可选)”部分中，选择组合框中的“Web”  并输入以下重定向 URI：`https://localhost:44326/`。
1. 选择“注册”  以创建应用程序。
1. 选择“身份验证”菜单，然后添加以下信息  ：
   - 在“高级设置”  |“隐式授权”  部分中，选中“ID 令牌”  ，因为此示例需要启用“[隐式授权流](v2-oauth2-implicit-grant-flow.md)”才能登录用户。
1. 选择“保存”  。

# <a name="javatabjava"></a>[Java](#tab/java)

4. 当 **“注册应用程序”页**出现时，输入应用程序的友好名称（例如“java-webapp”），选择“任何组织目录中的帐户”并选择“Web 应用/API”作为“应用程序类型”  。
1. 单击“注册”  以注册应用程序。
1. 在左侧菜单上，单击“身份验证”  ，并在“重定向 URI”  下，选择“Web”。 将需要输入两个不同的重定向 URI：一个用于登录页，另一个用于图形页。 对于这个 URI，应该使用相同的主机和端口号，然后后跟“/msal4jsample/secure/aad”作为登录页，后跟“msal4jsample/graph/me”作为用户信息页。
 默认情况下，该示例使用：

    - `http://localhost:8080/msal4jsample/secure/aad`。
    - `http://localhost:8080/msal4jsample/graph/me`

    在“高级设置”  部分，将“注销 URL”设置为 `http://localhost:8080/msal4jsample/sign_out`。 

     单击“保存”  。

1. 从菜单中选择“证书和机密”  ，并在“客户端密码”  部分中，单击“新建客户端密码”  ：

    - 键入密钥说明
    - 选择密钥持续时间“1 年”。 
    - 选择“添加”  时，将显示密钥值。
    - 复制密钥的值以备后用。 此密钥值将不会再次显示，也无法通过其他任何方式检索，因此，一旦在 Azure 门户中显示该密钥值，就将其记录下来。

# <a name="pythontabpython"></a>[Python](#tab/python)

4. 出现“注册应用程序”页后，请输入应用程序的注册信息： 
   - 在“名称”  部分输入一个会显示给应用用户的有意义的应用程序名称，例如 `python-webapp`。
   - 将“支持的帐户类型”更改为“任何组织目录中的帐户”。  
   - 在“重定向 URI (可选)”部分中，选择组合框中的“Web”  并输入以下重定向 URI：`http://localhost:5000/getAToken`。
1. 选择“注册”  以创建应用程序。
1. 在应用的“概述”页上，找到“应用程序(客户端) ID”值，并记下该值以供后续使用   。 稍后需要使用它为此项目配置 Visual Studio 配置文件。
1. 在应用的“概述”页中，选择“身份验证”部分。 
   - 在“高级设置”  部分中，将“注销 URL”设置为 `http://localhost:5000/logout` 

  选择“保存”  。
1. 在左侧菜单中选择“证书和机密”，然后在“客户端机密”部分单击“新建客户端机密”：   

      - 键入密钥说明
      - 选择密钥持续时间“1 年”。 
      - 单击“添加”时，将显示密钥值。 
      - 复制密钥的值。 稍后需要用到此值。
---

## <a name="register-an-app-using-powershell"></a>使用 PowerShell 注册应用

> [!NOTE]
> 目前，Azure AD PowerShell 仅创建使用下述受支持帐户类型的应用程序：
>
> - MyOrg（仅此组织目录中的帐户）
> - AnyOrg（任何组织目录中的帐户）。
>

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [应用的代码配置](scenario-web-app-sign-user-app-configuration.md)

<!-- Update_Description: wording update -->