---
title: "Azure API 管理策略示例 - 使用 Google OAuth 令牌授予访问权限"
description: "Azure API 管理策略示例 - 演示如何使用 Google 作为 OAuth 令牌提供程序授予对终结点的访问权限。"
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 10/13/2017
ms.date: 02/26/2018
ms.author: v-yiso
ms.openlocfilehash: a95d84396c13ff3b85f5f4c88c8353be0867463d
ms.sourcegitcommit: 3629fd4a81f66a7d87a4daa00471042d1f79c8bb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2018
---
# <a name="authorize-access-using-google-oauth-token"></a>使用 Google OAuth 令牌授予访问权限

本文介绍 Azure API 管理策略示例，该示例演示如何使用 Google 作为 OAuth 令牌提供程序授予对终结点的访问权限。 若要设置或编辑策略代码，请执行[设置或编辑策略](../set-edit-policies.md)中所述的步骤。 若要查看其他示例，请参阅[策略示例](../policy-samples.md)。

## <a name="policy"></a>策略

将代码粘贴到“入站”块中。

```xml
<!-- The policy defined in this file demonstrates how to add a Forwarded header in the inbound request to allow the backend API to construct proper URLs.

<!-- Forwarded header is defined in the IETF RFC 7239 https://tools.ietf.org/html/rfc7239  -->

<!-- Copy this snippet into the inbound section. -->

<policies>
  <inbound>
    <base />
    <set-header exists-action="override" name="Forwarded">
      <value>@("proto=" + context.Request.OriginalUrl.Scheme + ";host=" + context.Request.OriginalUrl.Host + ";")</value>
    </set-header>
  </inbound>
  <backend>
    <base />
  </backend>
  <outbound>
    <base />
  </outbound>
  <on-error>
    <base />
  </on-error>
</policies>
```

## <a name="next-steps"></a>后续步骤

了解有关 APIM 策略的详细信息：

+ [转换策略](../api-management-transformation-policies.md)
+ [策略示例](../policy-samples.md)

