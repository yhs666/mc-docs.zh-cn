---
title: 配置 Microsoft 帐户身份验证 - Azure 应用服务
description: 了解如何为应用服务应用程序配置 Microsoft 帐户身份验证。
author: mattchenderson
services: app-service
documentationcenter: ''
manager: syntaxc4
editor: ''
ms.assetid: ffbc6064-edf6-474d-971c-695598fd08bf
ms.service: app-service
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
origin.date: 08/08/2019
ms.date: 09/03/2019
ms.author: v-tawe
ms.custom: seodec18
ms.openlocfilehash: e9f2f5d757a4138f980c6a606b8c5a40d1caa7b9
ms.sourcegitcommit: bc34f62e6eef906fb59734dcc780e662a4d2b0a2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/09/2019
ms.locfileid: "70806881"
---
# <a name="how-to-configure-your-app-service-application-to-use-microsoft-account-login"></a>如何将应用服务应用程序配置为使用 Microsoft 帐户登录
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

本主题说明如何将 Azure 应用服务配置为使用 Microsoft 帐户作为身份验证提供程序。 

## <a name="register-microsoft-account"> </a>将应用注册到 Microsoft 帐户
1. 登录到 [Azure 门户]，并导航到应用程序。 

<!-- Copy your **URL**, which you will use later to configure your app with Microsoft Account. -->
1. 导航到[**应用注册**](https://portal.azure.cn/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade)，并使用 Microsoft 帐户登录（如果要求）。

1. 单击“新建注册”  ，然后键入应用程序名称。

1. 在**重定向 URI** 中，选择 **Web**，然后键入 `https://<app-domain-name>/.auth/login/microsoftaccount/callback supply the endpoint for your application`。 将“\<app-domain-name>”  替换为应用的域名。  例如，`https://contoso.chinacloudsites.cn/.auth/login/microsoftaccount/callback`。 

   > [!NOTE]
   > 在 URL 中使用 HTTPS 方案。

1. 选择“注册”  。 

1. 复制**应用程序(客户端) ID**。 稍后需要用到此信息。 
   
7. 从新应用注册的左侧导航栏中，选择“证书和机密”   > “新建客户端密码”  。 提供说明，选择有效期，然后选择“添加”  。

1. 复制“证书和机密”  页中显示的值。 关闭页面后，就不再显示该值。

    > [!IMPORTANT]
    > 密码是一个非常重要的安全凭据。 请不要与任何人共享密码或者在客户端应用程序中分发它。

## <a name="secrets"> </a>将 Microsoft 帐户信息添加到应用服务应用程序
1. 在 [Azure 门户]中，导航到应用程序。 在左侧导航栏中，单击“身份验证/授权”  。

2. 如果“身份验证/授权”功能未启用，请选择“打开”  。

3. 在“身份验证提供程序”下，选择“Microsoft 帐户”   。 粘贴先前获得的“应用程序(客户端) ID”和“客户端密码”，并可选择启用应用程序所需的任何范围。  。

    默认情况下，应用服务提供身份验证但不限制对站点内容和 API 的已授权访问。 必须在应用代码中为用户授权。

4. （可选）若要限制只有 Microsoft 帐户用户可以访问，请将“请求未经身份验证时需执行的操作”  设置为“使用 Microsoft 帐户登录”  。 这会要求对所有请求进行身份验证，所有未经身份验证的请求将重定向到 Microsoft 帐户进行身份验证。

    > [!CAUTION]
    > 以这种方式限制访问适用于对应用的所有调用，对于想要主页公开可用的应用程序来说，这可能是不可取的，就像在许多单页应用程序中一样。 对于此类应用程序，“允许匿名请求(无操作)”  可能是首选，应用本身手动启动登录，如[此处](overview-authentication-authorization.md#authentication-flow)所述。

5. 单击“保存”  。

现在，可以使用 Microsoft 帐户在应用中进行身份验证。

## <a name="related-content"> </a>相关内容
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- URLs. -->

[My Applications]: https://portal.azure.cn/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade
[Azure 门户]: https://portal.azure.cn/