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
ms.date: 06/20/2019
ms.author: v-junlch
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: e4508d46d23a44811e4cb0fb2ea5be4986bdad91
ms.sourcegitcommit: 9d5fd3184b6a47bf3b60ffdeeee22a08354ca6b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2019
ms.locfileid: "67305926"
---
# <a name="protected-web-api---app-registration"></a>受保护的 Web API - 应用注册

本文介绍适用于受保护 Web API 的应用注册详细信息。

请参阅[快速入门：将应用程序注册到 Microsoft 标识平台](quickstart-register-app.md)，了解如何通过常用步骤来注册应用程序。

## <a name="accepted-token-version"></a>接受的令牌版本

Microsoft 标识平台终结点可以发出两种类型的令牌：v1.0 令牌和 v2.0 令牌。 可以在[访问令牌](access-tokens.md)中详细了解这些令牌。 

创建应用程序后，可以按以下步骤更改接受的令牌版本：

1. 在 Azure 门户中选择应用，然后选择应用的“清单”  。
2. 在清单中搜索 **"accessTokenAcceptedVersion"** ，可以看到其值为 **2**。 此属性让 Azure AD 知道 Web API 接受 v2.0 令牌。 如果此属性为 **null**，则接受的令牌版本将为 v1.0。
3. 选择“其他安全性验证”  。

> [!NOTE]
> 将由 Web API 确定所接受的令牌版本（v1.0 或 v2.0）。 当客户端使用 Microsoft 标识平台 v2.0 终结点请求 Web API 的令牌时，所获得的令牌会指示哪个版本是 Web API 可以接受的。

## <a name="no-redirect-uri"></a>无重定向 URI

Web API 不需注册重定向 URI，因为没有用户以交互方式登录。

## <a name="expose-an-api"></a>公开 API

特定于 Web API 的另一设置是公开的 API 和公开的作用域

### <a name="resource-uri-and-scopes"></a>资源 URI 和作用域

作用域通常采用 `resourceURI/scopeName` 形式。 对于 Microsoft Graph，作用域有 `User.Read` 之类的快捷方式，但该字符串就是 `https://microsoftgraph.chinacloudapi.cn/user.read` 的快捷方式。

在应用注册过程中，需定义以下参数：

- 一个资源 URI - 默认情况下，门户建议你使用 `api://{clientId}`。 此资源 URI 独一无二，但不是人工可读的。 可以更改它，但必须确保它是独一无二的。
- 一个或多个作用域

作用域也显示在呈现给使用应用程序的最终用户的许可屏幕上。 因此，需提供用于描述作用域的相应字符串：

- 如最终用户所见
- 如租户管理员所见，谁能授予管理员许可

### <a name="how-to-expose-the-api"></a>如何公开 API

1. 在应用程序注册中选择“公开 API”部分，然后执行以下操作： 
   1. 选择“添加范围”。 
   1. 选择“保存并继续”，接受建议的应用程序 ID URI (api://{clientId})。 
   1. 输入以下参数：
      - 对于“作用域名称”  ，请使用 `access_as_user`。
      - 对于“谁能许可”，请确保选择“管理员和用户”选项。  
      - 在“管理员许可显示名称”中，  键入 `Access TodoListService as a user`。
      - 在“管理员许可说明”中，  键入 `Accesses the TodoListService Web API as a user`。
      - 在“用户许可显示名称”中，  键入 `Access TodoListService as a user`。
      - 在“用户许可说明”中，  键入 `Accesses the TodoListService Web API as a user`。
      - 将“状态”始终设置为“已启用”   。
      - 选择“添加作用域”。 

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [应用的代码配置](scenario-protected-web-api-app-configuration.md)

