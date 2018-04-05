---
title: 结合 Azure Active Directory 和 API 管理使用 OAuth 2.0 保护 API
description: 了解如何使用 Azure Active Directory 和 API 管理保护 Web API 后端。
services: api-management
documentationcenter: ''
author: miaojiang
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 10/30/2017
ms.date: 04/09/2018
ms.author: v-yiso
ms.openlocfilehash: f15d37d376663c7ae26da2d325388ff2e5ef1141
ms.sourcegitcommit: 4e2ee8ad9e6f30e31d3f0c24c716cc78f780dbf5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2018
---
# <a name="how-to-protect-an-api-using-oauth-20-with-azure-active-directory-and-api-management"></a>如何结合 Azure Active Directory 和 API 管理使用 OAuth 2.0 保护 API

本指南介绍如何结合 Azure Active Directory (AAD) 使用 OAuth 2.0 协议配置 API 管理 (APIM) 实例，以保护 API。 

## <a name="prerequisite"></a>先决条件
若要执行本文中的步骤，必须提供：
* 一个 APIM 实例
* 使用 APIM 实例发布的 API
* 一个 AAD 租户

## <a name="overview"></a>概述

本指南介绍如何在 APIM 中使用 OAuth 2.0 保护 API。 本文中使用 AAD 作为授权服务器（OAuth 服务器）。 下面是简要的步骤概述：

1. 在 AAD 中注册一个应用程序 (backend-app)，用于表示 API
2. 在 AAD 中注册另一个应用程序 (client-app)，用于表示需要调用 API 的客户端应用程序
3. 在 AAD 中授予权限，使 client-app 能够调用 backend-app
4. 将开发人员控制台配置为使用 OAuth 2.0 用户授权
5. 添加 validate-jwt 策略以验证每个传入请求的 OAuth 令牌

## <a name="register-an-application-in-aad-to-represent-the-api"></a>在 AAD 中注册一个应用程序，用于表示 API

若要使用 AAD 保护 API，首先需要在 AAD 中注册一个表示该 API 的应用程序。 

导航到 AAD 租户，然后导航到“应用注册”。

选择“新建应用程序注册”。 

提供应用程序的名称。 本示例使用 `backend-app`。  

选择“Web 应用/API”作为“应用程序类型”。 

对于“登录 URL”，可以使用 `https://localhost` 作为占位符。

单击“创建”。

创建应用程序后，记下“应用程序 ID”，以便在后续步骤中使用。 

## <a name="register-another-application-in-aad-to-represent-a-client-application"></a>在 AAD 中注册另一个应用程序，用于表示客户端应用程序

需要调用该 API 的每个客户端应用程序也必须作为应用程序注册到 AAD 中。 在本指南中，我们将使用 APIM 开发人员门户中的开发人员控制台作为示例客户端应用程序。 

我们需要在 AAD 中注册另一个应用程序，用于表示开发人员控制台。

再次单击“新建应用程序注册”。 

提供应用程序的名称，并选择“Web 应用/API”作为“应用程序类型”。 本示例使用 `client-app`。  

对于“登录 URL”，可以使用 `https://localhost` 作为占位符，或使用 APIM 实例的登录 URL。 本示例使用 `https://contoso5.portal.azure-api.cn/signin`。

单击“创建”。

创建应用程序后，记下“应用程序 ID”，以便在后续步骤中使用。 

现在，需要创建此应用程序的客户端机密，以便在后续步骤中使用。

再次单击“设置”并转到“密钥”。

在“密码”下面提供**密钥说明**，选择密钥过期时间，并单击“保存”。

记下密钥值。 

## <a name="grant-permissions-in-aad"></a>在 AAD 中授予权限

注册用于表示 API（即 backend-app）和开发人员控制台（即 client-app）的两个应用程序之后，需要授予权限，使 client-app 能够调用 backend-app。  

再次导航到“应用程序注册”。 

单击 `client-app` 并转到“设置”。

单击“所需权限”，然后单击“添加”。

单击“选择 API”并搜索 `backend-app`。

在“委托的权限”下面选中`Access backend-app`。 

单击“选择”，然后单击“完成”。 

> [!NOTE]
> 如果“Windows Azure Active Directory”未在“对其他应用程序的权限”下列出，请单击“添加”并从列表中添加该项。
> 
> 

## <a name="enable-oauth-20-user-authorization-in-the-developer-console"></a>在开发人员控制台中启用 OAuth 2.0 用户授权

此时，我们已在 AAD 中创建应用程序，并已授予适当的权限来让 client-app 应用调用 backend-app。 

在本指南中，我们将使用开发人员控制台作为 client-app。 以下步骤说明如何在开发人员控制台中启用 OAuth 2.0 用户授权 

导航到 APIM 实例。

单击“OAuth 2.0”，然后单击“添加”。

提供“显示名称”和“说明”。

对于“客户端注册页 URL”，**请输入占位符值，如 `http://localhost`。  “客户端注册页 URL”指向供用户针对 OAuth 2.0 提供程序创建和配置自己帐户的页面，这些提供程序支持用户管理帐户。 在此示例中，用户不创建和配置自己的帐户，因此使用了占位符。

选中“授权代码”作为“授权类型”。

接下来，指定“授权终结点 URL”和“令牌终结点 URL”。

可以从 AAD 租户中的“终结点”页检索这些值。 若要访问终结点，请再次导航到“应用注册”页，并单击“终结点”。

复制“OAuth 2.0 授权终结点”，并将其粘贴到“授权终结点 URL”文本框。

复制“OAuth 2.0 令牌终结点”，并将其粘贴到“令牌终结点 URL”文本框。

除了在令牌终结点中粘贴上述值以外，请添加名为 **resource** 的附加正文参数，并使用 backend-app 的“应用程序 ID”作为该参数的值。

接下来，指定客户端凭据。 这些是 client-app 的凭据。

对于“客户端 ID”，请使用 client-app 的“应用程序 ID”。

对于“客户端机密”，请使用前面为 client-app 创建的密钥。 

紧接在客户端机密的后面，是授权代码授权类型的 **redirect_url**。

记下此 URL。

单击“创建”。

导航回到 client-app 的“设置”页。

单击“回复 URL”，并在第一行中粘贴 **redirect_url**。 在本示例中，我们已将 `https://localhost` 替换为第一行中的 URL。  

配置 OAuth 2.0 授权服务器后，开发人员控制台应该能够从 AAD 获取访问令牌。 

下一步是为 API 启用 OAuth 2.0 用户授权，使开发人员控制台知道在调用 API 之前，需要代表用户获取访问令牌。

导航到 APIM 实例，并转到“API”。

单击要保护的 API。 本示例使用 `Echo API`。

转到“设置”。

在“安全性”下，选择“OAuth 2.0”并选择前面配置的 OAuth 2.0 服务器。 

单击“保存” 。

## <a name="successfully-call-the-api-from-the-developer-portal"></a>从开发人员门户成功调用 API

在 `Echo API` 中启用 OAuth 2.0 用户授权后，开发人员控制台在调用 API 之前，将会代表用户获取访问令牌。

在开发人员门户中导航到 `Echo API` 下的任一操作，并单击“试用”，然后就会转到开发人员控制台。

可以看到，“授权”部分出现了一个与刚刚添加的授权服务器对应的新项。

从授权下拉列表中选择“授权代码”。系统会提示登录到 AAD 租户。 如果已使用帐户登录，则可能不会出现提示。

成功登录后，会将一个 `Authorization` 标头添加到请求，该标头包含从 AAD 获取的访问令牌。 

下面显示了一个采用 Base64 编码的示例令牌。

```
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6IlNTUWRoSTFjS3ZoUUVEU0p4RTJnR1lzNDBRMCIsImtpZCI6IlNTUWRoSTFjS3ZoUUVEU0p4RTJnR1lzNDBRMCJ9.eyJhdWQiOiIxYzg2ZWVmNC1jMjZkLTRiNGUtODEzNy0wYjBiZTEyM2NhMGMiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC80NDc4ODkyMC05Yjk3LTRmOGItODIwYS0yMTFiMTMzZDk1MzgvIiwiaWF0IjoxNTIxMTUyNjMzLCJuYmYiOjE1MjExNTI2MzMsImV4cCI6MTUyMTE1NjUzMywiYWNyIjoiMSIsImFpbyI6IkFWUUFxLzhHQUFBQUptVzkzTFd6dVArcGF4ZzJPeGE1cGp2V1NXV1ZSVnd1ZXZ5QU5yMlNkc0tkQmFWNnNjcHZsbUpmT1dDOThscUJJMDhXdlB6cDdlenpJdzJLai9MdWdXWWdydHhkM1lmaDlYSGpXeFVaWk9JPSIsImFtciI6WyJyc2EiXSwiYXBwaWQiOiJhYTY5ODM1OC0yMWEzLTRhYTQtYjI3OC1mMzI2NTMzMDUzZTkiLCJhcHBpZGFjciI6IjEiLCJlbWFpbCI6Im1pamlhbmdAbWljcm9zb2Z0LmNvbSIsImZhbWlseV9uYW1lIjoiSmlhbmciLCJnaXZlbl9uYW1lIjoiTWlhbyIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0Ny8iLCJpcGFkZHIiOiIxMzEuMTA3LjE3NC4xNDAiLCJuYW1lIjoiTWlhbyBKaWFuZyIsIm9pZCI6IjhiMTU4ZDEwLWVmZGItNDUxMS1iOTQzLTczOWZkYjMxNzAyZSIsInNjcCI6InVzZXJfaW1wZXJzb25hdGlvbiIsInN1YiI6IkFGaWtvWFk1TEV1LTNkbk1pa3Z3MUJzQUx4SGIybV9IaVJjaHVfSEM1aGciLCJ0aWQiOiI0NDc4ODkyMC05Yjk3LTRmOGItODIwYS0yMTFiMTMzZDk1MzgiLCJ1bmlxdWVfbmFtZSI6Im1pamlhbmdAbWljcm9zb2Z0LmNvbSIsInV0aSI6ImFQaTJxOVZ6ODBXdHNsYjRBMzBCQUEiLCJ2ZXIiOiIxLjAifQ.agGfaegYRnGj6DM_-N_eYulnQdXHhrsus45QDuApirETDR2P2aMRxRioOCR2YVwn8pmpQ1LoAhddcYMWisrw_qhaQr0AYsDPWRtJ6x0hDk5teUgbix3gazb7F-TVcC1gXpc9y7j77Ujxcq9z0r5lF65Y9bpNSefn9Te6GZYG7BgKEixqC4W6LqjtcjuOuW-ouy6LSSox71Fj4Ni3zkGfxX1T_jiOvQTd6BBltSrShDm0bTMefoyX8oqfMEA2ziKjwvBFrOjO0uK4rJLgLYH4qvkR0bdF9etdstqKMo5gecarWHNzWi_tghQu9aE3Z3EZdYNI_ZGM-Bbe3pkCfvEOyA
```

单击“发送”，然后即可成功调用 API。


## <a name="configure-a-jwt-validation-policy-to-pre-authorize-requests"></a>配置 JWT 验证策略，对请求进行预授权

此时，当用户尝试从开发人员控制台发出调用时，系统会提示其登录，而开发人员控制台会代表该用户获取访问令牌。 一切都符合预期。 但是，如果有人调用我们的 API 但未提供令牌或者提供无效的令牌，会发生什么情况？ 例如，你可以尝试删除 `Authorization` 标头，然后会发现仍可调用该 API。 原因是 APIM 暂时不会验证访问令牌。 它只会将 `Auhtorization` 标头传递给后端 API。

可以使用[验证 JWT](api-management-access-restriction-policies.md#ValidateJWT) 策略通过验证每个传入请求的访问令牌，对 APIM 中的请求进行预授权。 如果某个请求没有有效的令牌，API 管理会阻止该请求，不会将其传递给后端。 可将以下策略添加到 `Echo API`。 

```xml
<validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">
    <openid-config url="https://login.microsoftonline.com/{aad-tenant}/.well-known/openid-configuration" />
    <required-claims>
        <claim name="aud">
            <value>{Application ID of backend-app}</value>
        </claim>
    </required-claims>
</validate-jwt>
```

## <a name="next-steps"></a>后续步骤
* 有关保护后端服务的其他方法，请参阅[使用证书进行相互身份验证](api-management-howto-mutual-certificates.md)。

[Create an API Management service instance]: get-started-create-service-instance.md
[Manage your first API]: import-and-publish.md
