---
title: 配置 Microsoft 帐户身份验证 - Azure 应用服务
description: 了解如何为应用服务应用配置 Microsoft 帐户身份验证。
author: mattchenderson
services: app-service
documentationcenter: ''
manager: syntaxc4
editor: ''
ms.assetid: ffbc6064-edf6-474d-971c-695598fd08bf
ms.service: app-service
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
origin.date: 08/08/2019
ms.date: 11/25/2019
ms.author: v-tawe
ms.custom: seodec18
ms.openlocfilehash: 1eb7ed63ce5e59e3670dd7dfdb2929a37e0ae838
ms.sourcegitcommit: e7dd37e60d0a4a9f458961b6525f99fa0e372c66
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/27/2019
ms.locfileid: "74555986"
---
# <a name="configure-your-app-service-app-to-use-microsoft-account-login"></a>将应用服务应用配置为使用 Microsoft 帐户登录

[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

本主题说明如何将 Azure 应用服务配置为使用 Microsoft 帐户作为身份验证提供程序。 

## <a name="register-microsoft-account"> </a>将应用注册到 Microsoft 帐户

1. 在 Azure 门户中转到[**应用注册**](https://portal.azure.cn/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade)。 根据需要使用 Microsoft 帐户登录。
1. 选择“新建注册”  ，然后输入应用程序名称。
1. 在“重定向 URI”  中，选择 **Web**，然后输入 `https://<app-domain-name>/.auth/login/microsoftaccount/callback supply the endpoint for your application`。 将“\<app-domain-name>”  替换为应用的域名。  例如，`https://contoso.chinacloudsites.cn/.auth/login/microsoftaccount/callback`。 确保在 URL 中使用 HTTPS 方案。

1. 选择“注册”  。
1. 复制**应用程序(客户端) ID**。 稍后需要用到此信息。
1. 在左窗格中，选择“证书和机密”   >   “新建客户端机密”。 输入说明，选择有效期，然后选择“添加”  。
1. 复制“证书和机密”  页上显示的值。 离开页面后，就不再显示该值。

    > [!IMPORTANT]
    > 密码是一个非常重要的安全凭据。 请不要与任何人共享密码或者在客户端应用程序中分发它。

## <a name="secrets"> </a>将 Microsoft 帐户信息添加到应用服务应用程序

1. 在 [Azure 门户]中转到你的应用程序。
1. 选择“设置”   > “身份验证/授权”  ，并确保“应用服务身份验证”  为“启用”  。
1. 在“身份验证提供程序”下，选择“Microsoft 帐户”   。 粘贴前面获取的应用程序（客户端）ID 和客户端机密。 启用应用程序所需的任何范围。
1. 选择“确定”  。

   应用服务提供身份验证但不限制对站点内容和 API 的已授权访问。 必须在应用代码中为用户授权。

1. （可选）若要限制只有 Microsoft 帐户用户可以访问，请将“请求未经身份验证时需执行的操作”  设置为“使用 Microsoft 帐户登录”  。 设置此功能时，应用会要求对所有请求进行身份验证。 它还将所有未经身份验证的请求重定向到 Microsoft 帐户进行身份验证。

   > [!CAUTION]
   > 以这种方式限制访问适用于对应用的所有调用，对于主页公开可用的应用程序来说，这可能是不可取的，就像在许多单页应用程序中一样。 对于此类应用程序，“允许匿名请求(无操作)”  可能是首选，因此应用会以手动方式自行启动身份验证。 有关详细信息，请参阅[身份验证流](overview-authentication-authorization.md#authentication-flow)。

1. 选择“保存”  。

现在，可以使用 Microsoft 帐户在应用中进行身份验证。

## <a name="related-content"></a>后续步骤

[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- URLs. -->

[My Applications]: https://portal.azure.cn/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade
[Azure 门户]: https://portal.azure.cn/