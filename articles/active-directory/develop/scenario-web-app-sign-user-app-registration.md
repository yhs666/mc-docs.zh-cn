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
origin.date: 05/07/2019
ms.date: 06/20/2019
ms.author: v-junlch
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: f7cb4f29aabdf72e5a01786dcd4b34027b773a35
ms.sourcegitcommit: 9d5fd3184b6a47bf3b60ffdeeee22a08354ca6b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2019
ms.locfileid: "67305876"
---
# <a name="web-app-that-signs-in-users---app-registration"></a>用于登录用户的 Web 应用 - 应用注册

此页介绍用于登录用户的 Web 应用的应用注册详细信息。

若要注册应用程序，可以使用：

- [Web 应用快速入门](#register-an-app-using-the-quickstarts) - 除了提供创建应用程序的第一手体验，Azure 门户中的快速入门还包含名为“为我进行此更改”的按钮。  可以使用此按钮设置所需属性，对现有应用也可以这样做。 需根据自己的情况调整这些属性的值。 具体说来，应用的 Web API URL 可能会不同于提议的默认值，后者还会影响注销 URI。
- 用于[手动注册应用程序](#register-an-app-using-azure-portal)的 Azure 门户
- PowerShell 和命令行工具

## <a name="register-an-app-using-the-quickstarts"></a>按照快速入门注册应用

如果导航到此链接，则可通过创建 Web 应用程序来创建启动过程：

- [ASP.NET Core](https://portal.azure.cn/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview)
- [ASP.NET](https://portal.azure.cn/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/AspNetWebAppQuickstartPage/sourceType/docs)

### <a name="register-an-app-using-azure-portal"></a>使用 Azure 门户注册应用

> [!NOTE]
> 要使用的门户取决于应用程序是在国家/地区云还是在主权云中运行。 有关详细信息，请参阅[国家/地区云](./authentication-national-cloud.md#app-registration-endpoints)

1. 使用工作或学校帐户登录到 [Azure 门户](https://portal.azure.cn)。 
1. 如果你的帐户有权访问多个租户，请在右上角选择该帐户，并将门户会话设置为所需的 Azure AD 租户。
1. 在左侧导航窗格中，选择“Azure Active Directory”服务  ，然后选择“应用注册” > “新建注册”。  
1. 出现“注册应用程序”页后，请输入应用程序的注册信息： 
   - 为应用程序选择支持的帐户类型（请参阅[支持的帐户类型](./v2-supported-account-types.md)）
   - 在“名称”  部分输入一个会显示给应用用户的有意义的应用程序名称，例如 `AspNetCore-WebApp`。
   - 在“回复 URL”中添加应用的回复 URL，例如 `https://localhost:44321/`，然后选择“注册”。  
1. 选择“身份验证”菜单，然后添加以下信息  ：
- 在“回复 URL”中添加 `https://localhost:44321/signin-oidc`，然后选择“注册”。  
- 在“高级设置”  部分，将“注销 URL”设置为 `https://localhost:44321/signout-oidc`。 
- 在“隐式授权”下，勾选“ID 令牌”。  
- 选择“其他安全性验证”  。

### <a name="register-an-app-using-powershell"></a>使用 PowerShell 注册应用

> [!NOTE]
> 目前，Azure AD PowerShell 仅创建使用下述受支持帐户类型的应用程序：
>
> - MyOrg（仅此组织目录中的帐户）
> - AnyOrg（任何组织目录中的帐户）。
>

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [应用的代码配置](scenario-web-app-sign-user-app-configuration.md)

