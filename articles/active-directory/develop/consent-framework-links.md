---
title: 应用程序许可工作原理 | Microsoft Docs
description: 详细了解 Azure AD 许可框架工作原理，确定如何在开发基于 Azure AD 的应用程序时使用它
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
ms.date: 11/06/2018
ms.author: v-junlch
ms.openlocfilehash: bf3921d418227129bb092c6d53176df4436227fc
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52653604"
---
# <a name="how-application-consent-works"></a>应用程序许可工作原理

本文旨在帮助读者深入了解 Azure AD 同意框架的工作原理以便更有效地开发应用程序。

## <a name="recommended-documents"></a>建议的文档

- 概括性了解：[允许资源所有者如何通过同意操作管理应用程序对资源的访问权限](/active-directory/develop/active-directory-dev-glossary#consent)。
- 获取有关 [Azure AD 同意框架如何实现同意](/active-directory/develop/active-directory-integrating-applications#overview-of-the-consent-framework)的分步概述。
- 有关详细信息，请参阅[多租户应用程序如何使用同意框架](/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent)实现“用户”和“管理员”同意（支持更多高级多层应用程序模式）。
- 如需更深入的了解，请参阅[如何在授权代码授予流程中在 OAuth 2.0 协议层提供许可支持](/active-directory/develop/active-directory-protocols-oauth-code#request-an-authorization-code)。

## <a name="next-steps"></a>后续步骤
[AzureAD StackOverflow](http://stackoverflow.com/questions/tagged/azure-active-directory)

