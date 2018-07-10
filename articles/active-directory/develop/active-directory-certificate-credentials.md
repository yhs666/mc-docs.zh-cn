---
title: Azure AD 中的证书凭据 | Microsoft Docs
description: 本文介绍如何注册和使用用于应用程序身份验证的证书凭据
services: active-directory
documentationcenter: .net
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 88f0c64a-25f7-4974-aca2-2acadc9acbd8
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 03/15/2018
ms.date: 07/03/2018
ms.author: v-junlch
ms.reviewer: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 739a29d7cc36ea37b8d8d0a012b4c3f916fff1a5
ms.sourcegitcommit: da6168fdb4abc6e5e4dd699486b406b16cd45801
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/05/2018
ms.locfileid: "37800495"
---
# <a name="certificate-credentials-for-application-authentication"></a>用于应用程序身份验证的证书凭据

Azure Active Directory 允许应用程序使用自己的凭据进行身份验证，例如，在 OAuth 2.0 客户端凭据授予流 ([v1](active-directory-protocols-oauth-service-to-service.md)) 和代理流 ([v1](active-directory-protocols-oauth-on-behalf-of.md)) 中就是如此。
可以使用的凭据格式之一是使用应用程序拥有的证书签名的 JSON Web 令牌 (JWT) 断言。

## <a name="format-of-the-assertion"></a>断言的格式
若要计算断言，需使用选择的语言中的其中一个 [JSON Web 令牌](https://jwt.ms/)库。 令牌附带的信息包括：

#### <a name="header"></a>标头

| 参数 |  备注 |
| --- | --- |
| `alg` | 应为 **RS256** |
| `typ` | 应为 **JWT** |
| `x5t` | 应为 X.509 证书 SHA-1 指纹 |

#### <a name="claims-payload"></a>声明（有效负载）

| 参数 |  备注 |
| --- | --- |
| `aud` | 受众：应为 **https://login.partner.microsoftonline.cn/*tenant_Id*/oauth2/token** |
| `exp` | 过期日期：令牌过期的日期。 该时间表示为自 1970 年 1 月 1 日 (1970-01-01T0:0:0Z) UTC 至令牌失效时间的秒数。|
| `iss` | 颁发者：应为 client_id（客户端服务的应用程序 ID） |
| `jti` | GUID：JWT ID |
| `nbf` | 生效时间：此日期之前不能使用令牌 该时间表示为自 1970 年 1 月 1 日 (1970-01-01T0:0:0Z) UTC 至令牌颁发时间的秒数。 |
| `sub` | 使用者：`iss` 应为 client_id（客户端服务的应用程序 ID） |

#### <a name="signature"></a>签名

使用证书计算签名，如 [JSON Web 令牌 RFC7519 规范](https://tools.ietf.org/html/rfc7519)中所述

### <a name="example-of-a-decoded-jwt-assertion"></a>已解码的 JWT 断言示例

```
{
  "alg": "RS256",
  "typ": "JWT",
  "x5t": "gx8tGysyjcRqKjFPnd7RFwvwZI0"
}
.
{
  "aud": "https: //login.partner.microsoftonline.cn/contoso.partner.onmschina.cn/oauth2/token",
  "exp": 1484593341,
  "iss": "97e0a5b7-d745-40b6-94fe-5f77d35c6e05",
  "jti": "22b3bb26-e046-42df-9c96-65dbd72c1c81",
  "nbf": 1484592741,  
  "sub": "97e0a5b7-d745-40b6-94fe-5f77d35c6e05"
}
.
"Gh95kHCOEGq5E_ArMBbDXhwKR577scxYaoJ1P{a lot of characters here}KKJDEg"

```

### <a name="example-of-an-encoded-jwt-assertion"></a>已编码的 JWT 断言示例

以下字符串是已编码的断言的示例。 仔细查看会发现由句点 (.) 分隔的三部分。
第一部分编码标头，第二部分编码有效负载，最后一部分是使用前两部分内容中的证书进行计算的签名。
```
"eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJhdWQiOiJodHRwczpcL1wvbG9naW4ubWljcm9zb2Z0b25saW5lLmNvbVwvam1wcmlldXJob3RtYWlsLm9ubWljcm9zb2Z0LmNvbVwvb2F1dGgyXC90b2tlbiIsImV4cCI6MTQ4NDU5MzM0MSwiaXNzIjoiOTdlMGE1YjctZDc0NS00MGI2LTk0ZmUtNWY3N2QzNWM2ZTA1IiwianRpIjoiMjJiM2JiMjYtZTA0Ni00MmRmLTljOTYtNjVkYmQ3MmMxYzgxIiwibmJmIjoxNDg0NTkyNzQxLCJzdWIiOiI5N2UwYTViNy1kNzQ1LTQwYjYtOTRmZS01Zjc3ZDM1YzZlMDUifQ.
Gh95kHCOEGq5E_ArMBbDXhwKR577scxYaoJ1P{a lot of characters here}KKJDEg"
```

### <a name="register-your-certificate-with-azure-ad"></a>向 Azure AD 注册证书

可以使用以下任意方法通过 Azure 门户将证书凭据与 Azure AD 中的客户端应用程序相关联：

**上传证书文件**

在客户端应用程序的 Azure 应用注册中，依次单击“设置”、“密钥”和“上传公钥”。 选择要上传的证书文件并单击“保存”。 保存后，证书将上传并显示指纹、开始日期和过期值。 

**更新应用程序清单**

拥有证书后需计算：

- `$base64Thumbprint`，证书哈希的 base64 编码
- `$base64Value`，证书原始数据的 base64 编码

还需要提供 GUID 来标识应用程序清单中的密钥 (`$keyId`)。

在客户端应用程序的 Azure 门户应用注册中打开应用程序清单，并使用以下架构将 *keyCredentials* 属性替换为新的证书信息：

```
"keyCredentials": [
    {
        "customKeyIdentifier": "$base64Thumbprint",
        "keyId": "$keyid",
        "type": "AsymmetricX509Cert",
        "usage": "Verify",
        "value":  "$base64Value"
    }
]
```

将所做编辑保存到应用程序清单，并上传到 Azure AD。 keyCredentials 属性具有多个值，因此可上传多个证书实现更丰富的密钥管理。

<!-- Update_Description: link update -->