---
title: "如何在 Azure Active Directory 中调试对应用程序进行的基于 SAML 的单一登录 | Microsoft Docs"
description: "了解如何在 Azure Active Directory 中调试对应用程序进行基于 SAML 的单一登录 "
services: active-directory
author: alexchen2016
documentationcenter: na
manager: digimobile
ms.assetid: edbe492b-1050-4fca-a48a-d1fa97d47815
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
origin.date: 07/20/2017
ms.date: 08/24/2017
ms.author: v-junlch
ms.custom: aaddev
ms.reviewer: dastrock
ms.openlocfilehash: a494dd61c9ca20a3c158cfa9691678d633b55c1d
ms.sourcegitcommit: 1ca439ddc22cb4d67e900e3f1757471b3878ca43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/25/2017
---
# <a name="how-to-debug-saml-based-single-sign-on-to-applications-in-azure-active-directory"></a>如何在 Azure Active Directory 中调试对应用程序进行基于 SAML 的单一登录
在调试基于 SAML 的应用程序集成时，使用 [Fiddler](http://www.telerik.com/fiddler) 之类的工具查看 SAML 请求、SAML 响应和颁发给应用程序的实际 SAML 令牌通常很有帮助。 通过检查 SAML 令牌，可以确保按预期传递所有所需的属性、SAML 主题中的用户名和颁发者 URI。

![][1]

包含 SAML 令牌的 Azure AD 响应通常是在从 https://login.chinacloudapi.cn 发出 HTTP 302 重定向之后发生的响应，它将发送到应用程序的已配置**回复 URL**。 

可以通过选择此行，然后在右窗格中选择“检查器”>“WebForms”，来查看 SAML 令牌。 在此处右键单击“SAMLResponse”值并选择“发送到 TextWizard”。 然后在“转换”菜单中选择“从 Base64”，解码令牌并查看其内容。


            **注意**：查看此 HTTP 请求的内容时，Fiddler 可能会提示配置 HTTPS 流量解密，此时需要执行此操作。

## <a name="related-articles"></a>相关文章

- [有关 Azure Active Directory 中应用程序管理的文章索引](../active-directory-apps-index.md)

<!--Image references-->
[1]: ./media/active-directory-saml-debugging/fiddler.png

<!--Update_Description: update metadata properties -->