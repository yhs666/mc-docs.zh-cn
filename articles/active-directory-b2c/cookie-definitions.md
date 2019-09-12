---
title: Cookie 定义 - Azure Active Directory B2C | Microsoft Docs
description: 为 Azure Active Directory B2C 中使用的 Cookie 提供定义。
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
origin.date: 03/18/2019
ms.date: 09/02/2019
ms.author: v-junlch
ms.subservice: B2C
ms.openlocfilehash: 563f52235fc04c1dd96ea0288f61eef551b1b8ae
ms.sourcegitcommit: 7fcf656522eec95d41e699cb257f41c003341f64
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2019
ms.locfileid: "70310870"
---
# <a name="cookies-definitions-for-azure-active-directory-b2c"></a>Azure Active Directory B2C 的 Cookie 定义

下表列出了 Azure Active Directory B2C 中使用的 Cookie。

| Name | 域 | 过期时间 | 目的 |
| ----------- | ------ | -------------------------- | --------- |
| x-ms-cpim-admin | main.b2cadmin.ext.azure.com | [浏览器会话](active-directory-b2c-token-session-sso.md)结束 | 保存各个租户的用户成员身份数据。 用户所属的租户，以及成员身份级别（管理员或用户）。 |
| x-ms-cpim-slice | login.partner.microsoftonline.cn、b2clogin.cn、品牌域 | [浏览器会话](active-directory-b2c-token-session-sso.md)结束 | 用于将请求路由到相应的生产实例。 |
| x-ms-cpim-trans | login.partner.microsoftonline.cn、b2clogin.cn、品牌域 | [浏览器会话](active-directory-b2c-token-session-sso.md)结束 | 用于跟踪事务（对 Azure AD B2C 发出的身份验证请求数）和当前事务。 |
| x-ms-cpim-sso:{Id} | login.partner.microsoftonline.cn、b2clogin.cn、品牌域 | [浏览器会话](active-directory-b2c-token-session-sso.md)结束 | 用于保留 SSO 会话。 |
| x-ms-cpim-cache:{id}_n | login.partner.microsoftonline.cn、b2clogin.cn、品牌域 | [浏览器会话](active-directory-b2c-token-session-sso.md)结束，身份验证成功 | 用于保留请求状态。 |
| x-ms-cpim-csrf | login.partner.microsoftonline.cn、b2clogin.cn、品牌域 | [浏览器会话](active-directory-b2c-token-session-sso.md)结束 | 用于实现 CRSF 保护的跨网站请求伪造令牌。 |
| x-ms-cpim-dc | login.partner.microsoftonline.cn、b2clogin.cn、品牌域 | [浏览器会话](active-directory-b2c-token-session-sso.md)结束 | 用于 Azure AD B2C 网络路由。 |
| x-ms-cpim-ctx | login.partner.microsoftonline.cn、b2clogin.cn、品牌域 | [浏览器会话](active-directory-b2c-token-session-sso.md)结束 | 上下文 |
| x-ms-cpim-rp | login.partner.microsoftonline.cn、b2clogin.cn、品牌域 | [浏览器会话](active-directory-b2c-token-session-sso.md)结束 | 用于存储资源提供程序租户的成员身份数据。 |
| x-ms-cpim-rc | login.partner.microsoftonline.cn、b2clogin.cn、品牌域 | [浏览器会话](active-directory-b2c-token-session-sso.md)结束 | 用于存储中继 Cookie。 |


