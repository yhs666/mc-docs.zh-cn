---
title: Azure API 管理身份验证策略
description: 了解可在 Azure API 管理中使用的身份验证策略。
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 061702a7-3a78-472b-a54a-f3b1e332490d
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 11/27/2017
ms.author: v-yiso
ms.date: 02/26/2018
ms.openlocfilehash: e11c1cdd3c24d57f2791f6622ce3715a7cb74ec4
ms.sourcegitcommit: 3629fd4a81f66a7d87a4daa00471042d1f79c8bb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2018
ms.locfileid: "29285039"
---
# <a name="api-management-authentication-policies"></a>API 管理身份验证策略
本主题提供以下 API 管理策略的参考。 有关添加和配置策略的信息，请参阅 [API 管理中的策略](http://go.microsoft.com/fwlink/?LinkID=398186)。  
  
##  <a name="AuthenticationPolicies"></a> 身份验证策略  
  
-   [使用基本方法进行身份验证](./api-management-authentication-policies.md#Basic) - 使用基本身份验证方法向后端服务进行身份验证。  
  
-   [使用客户端证书进行身份验证](./api-management-authentication-policies.md#ClientCertificate) - 使用客户端证书向后端服务进行身份验证。  
  
##  <a name="Basic"></a> 使用基本方法进行身份验证  
 通过 `authentication-basic` 策略使用基本身份验证方法向后端服务进行身份验证。 此策略有效地将 HTTP 授权标头设置为与策略中提供的凭据对应的值。  
  
### <a name="policy-statement"></a>策略语句  
  
```xml  
<authentication-basic username="username" password="password" />  
```  
  
### <a name="example"></a>示例  
  
```xml  
<authentication-basic username="testuser" password="testpassword" />  
```  
  
### <a name="elements"></a>元素  
  
|Name|说明|必须|  
|----------|-----------------|--------------|  
|authentication-basic|根元素。|是|  
  
### <a name="attributes"></a>属性  
  
|Name|说明|必须|默认|  
|----------|-----------------|--------------|-------------|  
|username|指定基本凭据的用户名。|是|不适用|  
|password|指定基本凭据的密码。|是|不适用|  
  
### <a name="usage"></a>使用情况  
 此策略可在以下策略[段](./api-management-howto-policies.md#sections)和[范围](./api-management-howto-policies.md#scopes)中使用。  
  
-   **策略段：** inbound  
  
-   **策略范围：** API  
  
##  <a name="ClientCertificate"></a> 使用客户端证书进行身份验证  
 通过 `authentication-certificate` 策略使用客户端证书向后端服务进行身份验证。 需要首先将证书[安装到 API 管理](http://go.microsoft.com/fwlink/?LinkID=511599)，并由其指纹进行标识。  
  
### <a name="policy-statement"></a>策略语句  
  
```xml  
<authentication-certificate thumbprint="thumbprint" />  
```  
  
### <a name="example"></a>示例  
  
```xml  
<authentication-certificate thumbprint="....." />  
```  
  
### <a name="elements"></a>元素  
  
|Name|说明|必须|  
|----------|-----------------|--------------|  
|authentication-certificate|根元素。|是|  
  
### <a name="attributes"></a>属性  
  
|Name|说明|必须|默认|  
|----------|-----------------|--------------|-------------|  
|thumbprint|客户端证书的指纹。|是|不适用|  
  
### <a name="usage"></a>使用情况  
 此策略可在以下策略[段](./api-management-howto-policies.md#sections)和[范围](./api-management-howto-policies.md#scopes)中使用。  
  
-   **策略段：** inbound  
  
-   **策略范围：** API  
  

## <a name="next-steps"></a>后续步骤
有关如何使用策略的详细信息，请参阅：

+ [API 管理中的策略](api-management-howto-policies.md)
+ [转换 API](transform-api.md)
+ [策略参考](api-management-policy-reference.md)，获取策略语句及其设置的完整列表
+ [策略示例](policy-samples.md)   
