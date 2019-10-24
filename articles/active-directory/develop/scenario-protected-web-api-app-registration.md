---
title: 受保护的 Web API - 应用注册 | Azure
description: 了解构建受保护 Web API 的方法以及注册应用所需的信息。
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
origin.date: 05/07/2019
ms.date: 08/26/2019
ms.author: v-junlch
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: c020305b8cc7ab406a5afe535697d7acbef9dbe4
ms.sourcegitcommit: 18a0d2561c8b60819671ca8e4ea8147fe9d41feb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2019
ms.locfileid: "70134252"
---
# <a name="protected-web-api-app-registration"></a>受保护的 Web API：应用注册

本文介绍适用于受保护 Web API 的应用注册详细信息。

请参阅[快速入门：将应用程序注册到 Microsoft 标识平台](quickstart-register-app.md)，了解注册应用的步骤。

## <a name="accepted-token-version"></a>接受的令牌版本

Microsoft 标识平台终结点可以发出两种类型的令牌：v1.0 令牌和 v2.0 令牌。 有关这些令牌的详细信息，请参阅[访问令牌](access-tokens.md)。 接受的令牌版本取决于创建应用程序时选择的“支持的帐户类型”： 

创建应用程序后，可以按以下步骤确定或更改接受的令牌版本：

1. 在 Azure 门户中选择应用，然后选择应用的“清单”  。
2. 在清单中搜索 **"accessTokenAcceptedVersion"** 。 请注意其值为 **2**。 此属性让 Azure Active Directory (Azure AD) 知道 Web API 接受 v2.0 令牌。 如果该值为 **null**，则接受的令牌版本为 v1.0。
3. 如果更改了令牌版本，请选择“保存”。 

> [!NOTE]
> Web API 会指定它所接受的令牌版本（v1.0 或 v2.0）。 当客户端从 Microsoft 标识平台 v2.0 终结点请求 Web API 的令牌时，所获得的令牌会指示哪个版本是 Web API 可以接受的。

## <a name="no-redirect-uri"></a>无重定向 URI

Web API 不需注册重定向 URI，因为没有用户以交互方式登录。

## <a name="expose-an-api"></a>公开 API

特定于 Web API 的另一设置是公开的 API 和公开的作用域

### <a name="resource-uri-and-scopes"></a>资源 URI 和作用域

范围通常采用 `resourceURI/scopeName` 格式。 对于 Microsoft Graph，范围具有类似于 `User.Read` 的快捷方式。 此字符串是 `https://microsoftgraph.chinacloudapi.cn/user.read` 的快捷方式。

在应用注册过程中，需定义以下参数：

- 资源 URI。 默认情况下，门户会建议使用 `api://{clientId}`。 此资源 URI 是唯一的，但不是用户可读的。 可以更改它，但请确保新值是唯一的。
- 一个或多个范围。  （对于客户端应用程序，它们会显示为 Web API 的委托权限。） 
- 一个或多个应用角色。  （对于客户端应用程序，它们会显示为 Web API 的应用程序权限。） 

范围还会显示在呈现给应用的最终用户的许可屏幕上。 因此，需提供用于描述范围的相应字符串：

- 如最终用户所见。
- 如租户管理员所见，谁能授予管理员许可。

### <a name="exposing-delegated-permissions-scopes"></a>公开委托的权限（范围）

1. 在应用程序注册中选择“公开 API”部分。 
1. 选择“添加范围”。 
1. 出现提示时，请选择“保存并继续”，接受建议的应用程序 ID URI (`api://{clientId}`)。 
1. 输入以下参数：
      - 对于“范围名称”，请使用 **access_as_user**。 
      - 对于“谁能许可”，请确保选择“管理员和用户”。  
      - 在“管理员许可显示名称”中，输入“以用户身份访问 TodoListService”。  
      - 在“管理员许可说明”中，输入“以用户身份访问 TodoListService Web API”。  
      - 在“用户许可显示名称”中，输入“以用户身份访问 TodoListService”。  
      - 在“用户许可说明”中，输入“以用户身份访问 TodoListService Web API”。  
      - 将“状态”始终设置为“已启用”   。
      - 选择“添加作用域”。 

### <a name="if-your-web-api-is-called-by-a-daemon-app"></a>如果 Web API 由守护程序应用调用

本部分介绍如何注册受保护的 Web API，使其可由守护程序应用安全调用。

- 需要公开应用程序权限。  只需声明应用程序权限，因为守护程序应用不会与用户交互，因此委托的权限没有作用。
- 租户管理员可以要求 Azure Active Directory (Azure AD) 仅将 Web API 的令牌颁发给已注册的、可访问某个 Web API 应用程序权限的应用程序。

#### <a name="exposing-application-permissions-app-roles"></a>公开应用程序权限（应用角色）

若要公开应用程序权限，需要编辑清单。

1. 在应用程序的应用程序注册中选择“清单”。 
1. 编辑清单：找到 `appRoles` 设置，然后添加一个或多个应用程序角色。 以下示例 JSON 块中提供了角色定义。 仅将 `allowedMemberTypes` 保留设置为 `"Application"`。 确保 `id` 是唯一的 GUID，且 `displayName` 和 `value` 不包含空格。
1. 保存清单。

以下示例显示了 `appRoles` 的内容。 （`id` 可以是任何唯一的 GUID。）

```JSon
"appRoles": [
    {
    "allowedMemberTypes": [ "Application" ],
    "description": "Accesses the TodoListService-Cert as an application.",
    "displayName": "access_as_application",
    "id": "ccf784a6-fd0c-45f2-9c08-2f9d162a0628",
    "isEnabled": true,
    "lang": null,
    "origin": "Application",
    "value": "access_as_application"
    }
],
```

#### <a name="ensuring-that-azure-ad-issues-tokens-for-your-web-api-to-only-allowed-clients"></a>确保 Azure AD 仅向允许的客户端颁发 Web API 的令牌

Web API 将检查应用角色。 （这是开发人员公开应用程序权限的方式。）但是，你也可以将 Azure AD 配置为仅向租户管理员批准的、可访问你的 API 的应用颁发 Web API 的令牌。 若要添加此增强的安全性：

1. 在应用注册的应用“概述”页上的“本地目录中的托管应用程序”下，选择包含应用名称的链接。   此字段的标题可能已截断。 例如，你可能会看到“...中的托管应用程序”。 

   > [!NOTE]
   >
   > 选择此链接后，你将转到与相应租户中应用程序的服务主体关联的“企业应用程序概述”页。  可以使用浏览器中的“后退”按钮导航回到应用注册页。

1. 在“企业应用程序”页的“管理”部分选择“属性”页。  
1. 如果希望 Azure AD 仅允许从特定的客户端访问你的 Web API，请将“需要用户分配?”设置为“是”。  

   > [!IMPORTANT]
   >
   > 如果将“需要用户分配?”设置为“是”，当客户端请求 Web API 的访问令牌时，Azure AD 会检查这些客户端的应用角色分配。   如果客户端未分配到任何应用角色，Azure AD 将返回错误 `invalid_client: AADSTS501051: Application <application name> is not assigned to a role for the <web API>`。
   >
   > 如果将“需要用户分配?”设置为“否”，当客户端请求 Web API 的访问令牌时，Azure AD 不会检查应用角色分配。    任何守护程序客户端（即，任何使用客户端凭据流的客户端）只需指定其受众，即可获取 API 的访问令牌。 任何应用程序都可以访问 API，而无需请求其权限。 但是，如上一部分中所述，Web API 始终可以验证应用程序是否具有适当的角色（由租户管理员授权）。 API 的验证方式是验证访问令牌是否包含角色声明，以及此声明的值是否正确。 （在本例中，该值为 `access_as_application`。）

1. 选择“保存”  。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [应用的代码配置](scenario-protected-web-api-app-configuration.md)

<!-- Update_Description: wording update -->