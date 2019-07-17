---
title: Azure 门户中的应用注册培训指南 - Azure
description: 使用设备代码授予生成嵌入式无浏览器身份验证流。
services: active-directory
documentationcenter: ''
author: archieag
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
origin.date: 04/26/2019
ms.date: 07/02/2019
ms.author: v-junlch
ms.reviewer: lenalepa, keyam
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: ae0f6ee3fa837e31e86b1f0e704f2d6b415378a0
ms.sourcegitcommit: 5f85d6fe825db38579684ee1b621d19b22eeff57
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/05/2019
ms.locfileid: "67568755"
---
# <a name="training-guide-app-registrations-in-the-azure-portal"></a>培训指南：Azure 门户中的应用注册  

在 Azure 门户上的新[应用注册](https://portal.azure.cn/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview)体验中，可以发现大量的改进。 如果对旧体验比较熟悉，可以参考本培训指南开始使用新体验。

## <a name="key-changes"></a>关键更改

- 应用注册不局限于 **Web 应用/API** 或**本机**应用。 注册相应的重定向 URI，即可对所有这些应用使用同一个应用注册。
- 旧体验仅支持登录组织 (Azure AD) 帐户的应用。 应用注册为单租户（仅支持应用注册到的目录中的组织帐户），可修改为多租户（支持所有组织帐户）。
- 仅当已使用组织帐户登录到 Azure 门户后，才可以使用旧体验。 

## <a name="list-of-applications"></a>应用程序列表

- 新应用列表显示通过 Azure 门户中旧应用注册体验注册的应用程序（登录 Azure AD 帐户的应用）。
- 新应用列表不包含“应用程序类型”列（因为单个应用注册可能是多种类型），但包含两个附加的列：“创建时间”列与“证书和机密”列，后者显示已在应用中注册的凭据的状态（当前凭据、即将过期或已过期）。   

## <a name="new-app-registration"></a>新应用注册

在旧体验中，若要注册应用，需要提供：“名称”、“应用程序类型”和“单一登录 URL/重定向 URI”。    创建的应用是仅限 Azure AD 的单租户应用程序，这意味着，它们仅支持应用注册到的目录中的组织帐户。

在新体验中，必须提供应用的**名称**，并选择**支持的帐户类型**。 可以选择性地提供**重定向 URI**。 如果提供重定向 URI，需要指定它是否为 Web/公共 URI（移动和桌面）。 有关如何使用新应用注册体验注册应用的详细信息，请参阅[此快速入门](quickstart-register-app.md)。

## <a name="the-legacy-properties-page"></a>旧“属性”页

旧体验包含“属性”页，而新体验则不包含。  “属性”边栏选项卡包含以下字段：  “名称”、“对象 ID”、“应用程序 ID”、“应用 ID URI”、“徽标”、“主页 URL”、“注销 URL”、“服务条款 URL”、“隐私声明 URL”、“应用程序类型”和“多租户”。           

在新体验中的以下位置可以找到等效的功能：

- “名称”、“徽标”、“主页 URL”、“服务条款 URL”和“隐私声明 URL”现在位于应用的“品牌”页上。      
- “对象 ID”和“应用程序(客户端) ID”位于“概述”页上。   
- 旧体验中由“多租户”切换开关控制的功能已由“身份验证”页上的“支持的帐户类型”取代。    有关多租户如何映射到支持的帐户类型选项的详细信息，请参阅[此快速入门](quickstart-modify-supported-accounts.md)。
- “注销 URL”现在位于“身份验证”页上。  
- “应用程序类型”不再是有效字段。  现在由重定向 URI（可在“身份验证”页上找到）确定支持哪些应用类型。 
- “应用 ID URI”现在名为“应用程序 ID URI”，可在“公开 API”边栏选项卡上找到。    在旧体验中，此属性是使用以下格式自动注册的：`https://{tenantdomain}/{appID}`（例如 `https://microsoft.partner.onmschina.cn/aeb4be67-a634-4f20-9a46-e0d4d4f1f96d`）。 在新格式，它自动生成为 `api://{appID}`，但需要显式保存。

## <a name="reply-urlsredirect-urls"></a>回复 URL/重定向 URI

在旧体验中，应用包含“回复 URL”页。  在新体验中，可以在应用的“身份验证”部分找到“回复 URL”。  此外，它们现在称为“重定向 URI”。  此外，重定向 URI 的格式已更改。 需要将它们与应用类型（Web 或公共）相关联。 此外，出于安全原因，不支持通配符和 http:// 方案（ http://localhost) 除外）。

## <a name="keyscertificates--secrets"></a>密钥/证书和机密

在旧体验中，应用包含“密钥”页。  在新体验中，该设置已重命名为“证书和机密”。  此外，“公钥”现在称为“证书”，“密码”称为“客户端机密”。    

## <a name="required-permissionsapi-permissions"></a>所需的权限/API 权限

- 在旧体验中，应用包含“所需的权限”页。  在新体验中，该设置已重命名为“API 权限”。 
- 在旧体验中选择某个 API 时，可以从 Microsoft API 的精简列表中进行选择，或者搜索租户中的服务主体。 在新体验中，可以选择从多个选项卡进行选择：“Microsoft API”、“我的组织使用的 API”或“我的 API”。    在“我的组织使用的 API”选项卡上的搜索栏可以搜索租户中的服务主体。  

   > [!NOTE]
   > 如果应用程序未与租户相关联，则不会显示此选项卡。 有关如何使用新体验请求权限的详细信息，请参阅[此快速入门](quickstart-configure-app-access-web-apis.md)。

- 旧体验在“请求的权限”页面顶部提供了“授予权限”按钮。   新体验提供“授予许可”部分，并在应用的“API 权限”部分提供“授予管理员许可”按钮。    此外，按钮的运行方式有一些差异：
   - 在旧体验中，逻辑根据已登录用户和请求的权限而异。 逻辑是：
      - 如果请求只能由用户许可的权限，并且登录的用户不是管理员，则该用户可以针对所请求的权限授予用户许可。
      - 如果至少请求了一个需要管理员许可的权限，并且登录的用户不是管理员，则该用户在尝试授予许可时会收到错误。
      - 如果登录的用户是管理员，则针对所有请求的权限授予管理员许可。
   - 在新体验中，只有管理员能够授予许可。 当管理员选择“授予管理员许可”按钮时，将对请求的所有权限授予管理员许可。 

## <a name="deleting-an-app-registration"></a>删除应用注册

在旧体验中，只能删除单租户应用。 对于多租户应用，已禁用删除按钮。 在新体验中，可以删除处于任何状态的应用，但必须确认该操作。 有关删除应用注册的详细信息，请参阅[此快速入门](quickstart-remove-app.md)。

## <a name="application-manifest"></a>应用程序清单

旧体验和新体验对清单编辑器中的 JSON 格式使用不同的版本。 有关详细信息，请参阅[应用程序清单](reference-app-manifest.md)。

## <a name="new-ui"></a>新 UI

对于以前只能使用清单编辑器或 API 设置的或者不存在的属性，现在提供了新的 UI。

- 在“身份验证”页上可以找到“隐式授权流”(oauth2AllowImplicitFlow)。   与在旧体验中不同，现在可以启用“访问令牌”和/或“ID 令牌”。  
- 可以通过“公开 API”页配置“此 API 定义的范围”(oauth2Permissions) 和“已授权的客户端应用程序”(preAuthorizedApplications)。    有关如何将应用配置为 Web API 和公开权限/范围的详细信息，请参阅[此快速入门](quickstart-configure-app-expose-web-apis.md)。
- 在“品牌”边栏选项卡页上可以找到“发布者域”（在[应用程序的许可提示](application-consent-experience.md)中向用户显示）。   有关如何配置发布者域的详细信息，请参阅[此操作指南](howto-configure-publisher-domain.md)。

## <a name="limitations"></a>限制

新体验具有以下限制：

- 新体验目前在 Azure AD B2C 租户中不可用。
- 客户端机密（应用密码）的格式不同于旧体验中的格式，会中断 CLI。
- 不支持在 UI 中更改受支持帐户的值。 除非在 Azure AD 单租户与多租户之间切换，否则需要使用应用清单。

