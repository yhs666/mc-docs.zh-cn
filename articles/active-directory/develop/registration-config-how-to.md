---
title: 如何为给定 API 选择权限 | Microsoft Docs
description: 如何使用 Azure AD 查找要开发或注册的自定义应用程序的身份验证终结点。
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
origin.date: 09/11/2018
ms.date: 12/04/2018
ms.author: v-junlch
ms.openlocfilehash: 1daf6401b6911299c21245c401a2659b49be9c7f
ms.sourcegitcommit: 5f2849d5751cb634f1cdc04d581c32296e33ef1b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2018
ms.locfileid: "53028807"
---
# <a name="how-to-configure-endpoints"></a>如何配置终结点

可以在 [Azure 门户](https://portal.azure.cn)中找到应用程序的身份验证终结点。

-   导航到 [Azure 门户](https://portal.azure.cn)。

-   在左侧导航窗格中，单击“Azure Active Directory”。

-   单击“应用注册”并选择“终结点”。

-   这会打开“终结点”页，该页会列出租户的所有身份验证终结点。

-   将特定于要使用的身份验证协议的终结点与应用程序 ID 结合使用，生成特定于应用程序的身份验证请求。

## <a name="next-steps"></a>后续步骤
[Azure Active Directory 开发人员指南](/active-directory/develop/active-directory-developers-guide#authentication-and-authorization-protocols)

<!-- Update_Description: wording update -->