---
title: Azure Active Directory ID 令牌参考 | Microsoft Docs
description: 了解如何使用 Azure AD 的 v1.0 和 v2.0 终结点发出的 id_tokens。
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 10/05/2018
ms.date: 12/03/2018
ms.author: v-junlch
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: a326ca6a1e6240703f18fc357a3203a02795e7e1
ms.sourcegitcommit: 5f2849d5751cb634f1cdc04d581c32296e33ef1b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2018
ms.locfileid: "53029024"
---
# <a name="id-tokens"></a>ID 令牌

`id_tokens` 是在 [OpenID Connect](v1-protocols-openid-connect-code.md) 流中发送到客户端应用程序的。 它们可以一起发送来代替访问令牌，可供客户端用来对用户进行身份验证。 

## <a name="using-the-idtoken"></a>使用 id_token

ID 令牌应该用来验证某个用户是否符合其声称的身份，以及用来获取该用户的其他有用信息 - 它不应该用来替代[访问令牌](access-tokens.md)进行授权。 它提供的声明可以用于应用程序内部的用户体验、数据库键控以及提供客户端应用程序访问权限。 

## <a name="claims-in-an-idtoken"></a>id_token 中的声明

Microsoft 标识的 `id_tokens` 为 [JWT](https://tools.ietf.org/html/rfc7519)，这意味着它们由标头、有效负载和签名部分组成。 可以使用标头和有效负载来验证令牌的真实性，而有效负载则包含客户端请求的用户信息。 除非另有说明，否则此处列出的所有声明均出现在 v1.0 和 v2.0 令牌中。

### <a name="v10"></a>v1.0

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6IjdfWnVmMXR2a3dMeFlhSFMzcTZsVWpVWUlHdyIsImtpZCI6IjdfWnVmMXR2a3dMeFlhSFMzcTZsVWpVWUlHdyJ9.eyJhdWQiOiJiMTRhNzUwNS05NmU5LTQ5MjctOTFlOC0wNjAxZDBmYzljYWEiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC9mYTE1ZDY5Mi1lOWM3LTQ0NjAtYTc0My0yOWYyOTU2ZmQ0MjkvIiwiaWF0IjoxNTM2Mjc1MTI0LCJuYmYiOjE1MzYyNzUxMjQsImV4cCI6MTUzNjI3OTAyNCwiYWlvIjoiQVhRQWkvOElBQUFBcXhzdUIrUjREMnJGUXFPRVRPNFlkWGJMRDlrWjh4ZlhhZGVBTTBRMk5rTlQ1aXpmZzN1d2JXU1hodVNTajZVVDVoeTJENldxQXBCNWpLQTZaZ1o5ay9TVTI3dVY5Y2V0WGZMT3RwTnR0Z2s1RGNCdGsrTExzdHovSmcrZ1lSbXY5YlVVNFhscGhUYzZDODZKbWoxRkN3PT0iLCJhbXIiOlsicnNhIl0sImVtYWlsIjoiYWJlbGlAbWljcm9zb2Z0LmNvbSIsImZhbWlseV9uYW1lIjoiTGluY29sbiIsImdpdmVuX25hbWUiOiJBYmUiLCJpZHAiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDcvIiwiaXBhZGRyIjoiMTMxLjEwNy4yMjIuMjIiLCJuYW1lIjoiYWJlbGkiLCJub25jZSI6IjEyMzUyMyIsIm9pZCI6IjA1ODMzYjZiLWFhMWQtNDJkNC05ZWMwLTFiMmJiOTE5NDQzOCIsInJoIjoiSSIsInN1YiI6IjVfSjlyU3NzOC1qdnRfSWN1NnVlUk5MOHhYYjhMRjRGc2dfS29vQzJSSlEiLCJ0aWQiOiJmYTE1ZDY5Mi1lOWM3LTQ0NjAtYTc0My0yOWYyOTU2ZmQ0MjkiLCJ1bmlxdWVfbmFtZSI6IkFiZUxpQG1pY3Jvc29mdC5jb20iLCJ1dGkiOiJMeGVfNDZHcVRrT3BHU2ZUbG40RUFBIiwidmVyIjoiMS4wIn0=.UJQrCA6qn2bXq57qzGX_-D3HcPHqBMOKDPx4su1yKRLNErVD8xkxJLNLVRdASHqEcpyDctbdHccu6DPpkq5f0ibcaQFhejQNcABidJCTz0Bb2AbdUCTqAzdt9pdgQvMBnVH1xk3SCM6d4BbT4BkLLj10ZLasX7vRknaSjE_C5DI7Fg4WrZPwOhII1dB0HEZ_qpNaYXEiy-o94UJ94zCr07GgrqMsfYQqFR7kn-mn68AjvLcgwSfZvyR_yIK75S_K37vC3QryQ7cNoafDe9upql_6pB2ybMVlgWPs_DmbJ8g0om-sPlwyn74Cc1tW3ze-Xptw_2uVdPgWyqfuWAfq6Q
```

在 [jwt.ms](https://jwt.ms/#id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6IjdfWnVmMXR2a3dMeFlhSFMzcTZsVWpVWUlHdyIsImtpZCI6IjdfWnVmMXR2a3dMeFlhSFMzcTZsVWpVWUlHdyJ9.eyJhdWQiOiJiMTRhNzUwNS05NmU5LTQ5MjctOTFlOC0wNjAxZDBmYzljYWEiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC9mYTE1ZDY5Mi1lOWM3LTQ0NjAtYTc0My0yOWYyOTU2ZmQ0MjkvIiwiaWF0IjoxNTM2Mjc1MTI0LCJuYmYiOjE1MzYyNzUxMjQsImV4cCI6MTUzNjI3OTAyNCwiYWlvIjoiQVhRQWkvOElBQUFBcXhzdUIrUjREMnJGUXFPRVRPNFlkWGJMRDlrWjh4ZlhhZGVBTTBRMk5rTlQ1aXpmZzN1d2JXU1hodVNTajZVVDVoeTJENldxQXBCNWpLQTZaZ1o5ay9TVTI3dVY5Y2V0WGZMT3RwTnR0Z2s1RGNCdGsrTExzdHovSmcrZ1lSbXY5YlVVNFhscGhUYzZDODZKbWoxRkN3PT0iLCJhbXIiOlsicnNhIl0sImVtYWlsIjoiYWJlbGlAbWljcm9zb2Z0LmNvbSIsImZhbWlseV9uYW1lIjoiTGluY29sbiIsImdpdmVuX25hbWUiOiJBYmUiLCJpZHAiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDcvIiwiaXBhZGRyIjoiMTMxLjEwNy4yMjIuMjIiLCJuYW1lIjoiYWJlbGkiLCJub25jZSI6IjEyMzUyMyIsIm9pZCI6IjA1ODMzYjZiLWFhMWQtNDJkNC05ZWMwLTFiMmJiOTE5NDQzOCIsInJoIjoiSSIsInN1YiI6IjVfSjlyU3NzOC1qdnRfSWN1NnVlUk5MOHhYYjhMRjRGc2dfS29vQzJSSlEiLCJ0aWQiOiJmYTE1ZDY5Mi1lOWM3LTQ0NjAtYTc0My0yOWYyOTU2ZmQ0MjkiLCJ1bmlxdWVfbmFtZSI6IkFiZUxpQG1pY3Jvc29mdC5jb20iLCJ1dGkiOiJMeGVfNDZHcVRrT3BHU2ZUbG40RUFBIiwidmVyIjoiMS4wIn0=.UJQrCA6qn2bXq57qzGX_-D3HcPHqBMOKDPx4su1yKRLNErVD8xkxJLNLVRdASHqEcpyDctbdHccu6DPpkq5f0ibcaQFhejQNcABidJCTz0Bb2AbdUCTqAzdt9pdgQvMBnVH1xk3SCM6d4BbT4BkLLj10ZLasX7vRknaSjE_C5DI7Fg4WrZPwOhII1dB0HEZ_qpNaYXEiy-o94UJ94zCr07GgrqMsfYQqFR7kn-mn68AjvLcgwSfZvyR_yIK75S_K37vC3QryQ7cNoafDe9upql_6pB2ybMVlgWPs_DmbJ8g0om-sPlwyn74Cc1tW3ze-Xptw_2uVdPgWyqfuWAfq6Q) 中查看此 v1.0 示例令牌。



### <a name="header-claims"></a>标头声明

|声明 | 格式 | 说明 |
|-----|--------|-------------|
|`typ` | 字符串 - 始终为“JWT” | 指示令牌是 JWT。|
|`alg` | String | 指示用于对令牌签名的算法。 示例：“RS256” |
|`kid` | String | 用于对此令牌进行签名的公钥的指纹。 已在 v1.0 和 v2.0 `id_tokens` 中发出。 |
|`x5t` | String | 与 `kid` 相同（在用法和值方面）。 但是，这是在 v1.0 `id_tokens` 中仅出于兼容目的而发出的旧式声明。 |

### <a name="payload-claims"></a>有效负载声明

|声明 | 格式 | 说明 |
|-----|--------|-------------|
|`aud` |  字符串，应用 ID URI | 标识令牌的目标接收方。 在 `id_tokens` 中，受众是在 Azure 门户中分配给应用的应用程序 ID。 应用应该验证此值，如果此值不匹配，应该拒绝该令牌。 |
|`iss` |  字符串，STS URI | 标识构造并返回令牌的安全令牌服务 (STS)，以及对用户进行身份验证的 Azure AD 租户。 表示用户是来自 Microsoft 帐户的使用者用户的 GUID 为 `9188040d-6c67-4c5b-b112-36a304b66dad`。 应用应该使用声明的 GUID 部分限制可登录应用的租户集（如果适用）。 |
|`iat` |  int，UNIX 时间戳 | “Issued At”表示针对此令牌进行身份验证的时间。  |
|`idp`|字符串，通常是 STS URI | 记录对令牌使用者进行身份验证的标识提供者。 除非用户帐户与颁发者不在同一租户中，否则此值与颁发者声明的值相同 - 例如，来宾。 如果声明不存在，则意味着可以改用 `iss` 的值。  对于在组织上下文中使用的个人帐户（例如，受邀加入 Azure AD 租户的个人帐户），`idp` 声明可能是“live.com”或包含 Microsoft 帐户租户 `9188040d-6c67-4c5b-b112-36a304b66dad` 的 STS URI。 |
|`nbf` |  int，UNIX 时间戳 | “nbf”（不早于）声明指定只能在哪个时间之后接受 JWT 的处理。|
|`exp` |  int，UNIX 时间戳 | “exp”（过期时间）声明指定只能在哪个时间（含）之前接受 JWT 的处理。  请务必注意，资源也可以在此时间之前拒绝令牌，例如，需要对身份验证进行更改，或者检测到令牌已吊销。 |
| `c_hash`| String |仅当 ID 令牌随 OAuth 2.0 授权代码一起颁发时，代码哈希才包含在 ID 令牌中。 它可用于验证授权代码的真实性。 有关执行此验证的详细信息，请参阅 [OpenID Connect 规范](http://openid.net/specs/openid-connect-core-1_0.html)。 |
|`at_hash`| String |仅当 ID 令牌随 OAuth 2.0 访问令牌一起颁发时，访问令牌哈希才包含在 ID 令牌中。 它可用于验证访问令牌的真实性。 有关执行此验证的详细信息，请参阅 [OpenID Connect 规范](http://openid.net/specs/openid-connect-core-1_0.html)。 |
|`aio` | 不透明字符串 | 一个内部声明，Azure AD 用它来记录有关重复使用令牌的数据。 应忽略。|
|`preferred_username` | String | 表示用户的主用户名。 可以是电子邮件地址、电话号码或未指定格式的一般用户名。 其值是可变的，可能随时改变。 由于此值是可变的，因此它不能用于做出授权决定。 需要 `profile` 范围才能接收此声明。|
|`email` | String | 对于具有电子邮件地址的来宾帐户，默认情况下会提供 `email` 声明。 电子邮件声明仅支持来自用户个人资料信息的可寻址邮件。 |
|`name` | String | 此 `name` 声明提供了标识令牌使用者的用户可读值。 该值不一定唯一，而且可变，只能用于显示目的。 需要 `profile` 范围才能接收此声明。 |
|`nonce`| String | nonce 与发送给 IDP 的原始 /authorize 请求中包含的参数匹配。 如果不匹配，应用程序会拒绝此令牌。 |
|`oid` | 字符串，GUID | 在 Microsoft 标识系统中，对象的不可变标识符在这种情况下是用户帐户。 此 ID 唯一标识应用程序中的用户 - 同一个用户登录两个不同的应用程序会在 `oid` 声明中收到相同值。 Microsoft Graph 将返回此 ID 作为给定用户帐户的 `id` 属性。 因为 `oid` 允许多个应用关联用户，需要 `profile` 作用域才能收到此声明。 请注意，如果单个用户存在于多个租户中，该用户将包含每个租户中的不同对象 ID - 它们将视为不同帐户，即使用户使用相同的凭据登录到每个帐户，也是如此。 |
|`rh` | 不透明字符串 |Azure 用来重新验证令牌的内部声明。 应忽略。 |
|`sub` | 字符串，GUID | 令牌针对其断言信息的主体，例如应用的用户。 此值固定不变，无法重新分配或重复使用。 使用者是成对标识符 - 它对特定应用程序 ID 是唯一的。 因此，如果单个用户使用两个不同的客户端 ID 登录到两个不同的应用，这些应用将收到两个不同的使用者声明值。 这不一定是所需的，具体取决于体系结构和隐私要求。 |
|`tid` | 字符串，GUID | 表示用户所属的 Azure AD 租户的 GUID。 对于工作和学校帐户，该 GUID 就是用户所属组织的不可变租户 ID。 对于个人帐户，该值为 `9188040d-6c67-4c5b-b112-36a304b66dad`。 需要 `profile` 范围才能接收此声明。 |
|`unique_name` | String | 提供一个用户可读值，用于标识令牌使用者。 不保证此值在租户中唯一，只应该用于显示目的。 仅在 v1.0 `id_tokens` 中颁发。 |
|`uti` | 不透明字符串 | Azure 用来重新验证令牌的内部声明。 应忽略。 |
|`ver` | 字符串，1.0 或 2.0 | 指示 id_token 的版本。 |

## <a name="validating-an-idtoken"></a>验证 id_token

验证 `id_token` 与[验证访问令牌](access-tokens.md#validating-tokens)的第一步十分类似 - 客户端应验证是否是正确的颁发者发送回令牌且令牌未遭篡改。 由于 `id_tokens` 始终是 JWT，因此可以使用许多现有的库来验证这些令牌 - 建议使用这其中的一个库来验证，而不要自行进行验证。 

若要手动验证令牌，请参阅[验证访问令牌](access-tokens.md#validating-tokens)中详述的步骤。 验证令牌上的签名以后，应验证 id_token 中的以下声明（这些也可以由令牌验证库来完成）：

- 时间戳：`iat`、`nbf` 和 `exp` 时间戳全都应位于当前时间之前或之后（具体取决于需要）。 
- 受众：`aud` 声明应该与应用程序的应用 ID 匹配。
- Nonce：有效负载中的 `nonce` 声明必须与进行初始请求时传递到 /authorize 终结点中的 nonce 参数匹配。

## <a name="next-steps"></a>后续步骤

- 了解 [Azure AD 访问令牌](access-tokens.md)

<!-- Update_Description: wording update -->