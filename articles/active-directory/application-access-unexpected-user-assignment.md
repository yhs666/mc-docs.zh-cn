---
title: 如何将用户分配给应用程序 | Microsoft Docs
description: 了解如何将用户分配给租户中的应用程序
services: active-directory
documentationcenter: ''
author: ajamess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 07/11/2017
ms.date: 06/20/2018
ms.author: v-junlch
ms.openlocfilehash: b24d01e95a4be57b5f1249648dfab716cb425f01
ms.sourcegitcommit: d744d18624d2188adbbf983e1c1ac1110d53275c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/22/2018
ms.locfileid: "36314242"
---
# <a name="how-to-assign-users-to-applications"></a>如何将用户分配给应用程序

本文介绍如何将用户分配给租户中的应用程序。

## <a name="how-do-users-get-assigned-to-an-application-in-azure-ad"></a>如何将用户分配给 Azure AD 中的应用程序？

用户要访问应用程序，必须先以某种方式将其分配给该应用程序。 可使用管理员、业务委托，或有时使用用户本身的身份执行分配。 下文介绍了可以将用户分配给应用程序的方式：

1.  管理员直接[将用户分配给](active-directory-coreapps-assign-user-azure-portal.md)应用程序


2.  对于第一方应用程序，如 [ Microsoft Office 365](http://products.office.com/)，管理员直接为用户分配许可证

3.  对于第一方应用程序，如 [ Microsoft Office 365](http://products.office.com/)，管理员直接为用户是其成员的组分配许可证

4.  [管理员同意](./develop/active-directory-devhowto-multi-tenant-overview.md#understanding-user-and-admin-consent)所有用户均可使用某个应用程序，然后用户登录该应用程序

5.  通过登录应用程序，用户自己[同意使用应用程序](./develop/active-directory-devhowto-multi-tenant-overview.md#understanding-user-and-admin-consent)

## <a name="next-steps"></a>后续步骤
[使用 Azure Active Directory 管理应用程序](active-directory-enable-sso-scenario.md)

<!-- Update_Description: update metedata properties -->