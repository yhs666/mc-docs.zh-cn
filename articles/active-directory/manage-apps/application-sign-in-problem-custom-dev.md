---
title: 登录自定义开发的应用程序时出现的问题 | Microsoft Docs
description: 可能导致用户无法登录到已使用 Azure AD 开发的应用程序的常见错误
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
origin.date: 07/11/2017
ms.date: 01/03/2019
ms.author: v-junlch
ms.reviewer: asteen
ms.openlocfilehash: 72682dba4d3a3da90de0f36771aa27f452a5bf7c
ms.sourcegitcommit: 1456ace86f950acc6908f4f5a9c773b93a4d6acc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/04/2019
ms.locfileid: "54029108"
---
# <a name="problems-signing-in-to-an-custom-developed-application"></a>登录自定义开发的应用程序时出现的问题

存在多个错误可能导致用户无法登录到应用。 出现此问题的最主要原因是应用配置错误。

## <a name="errors-related-to--misconfigured-apps"></a>与应用配置错误相关的错误

- 确认门户中的配置与应用中的配置相匹配。 具体而言，比较客户端/应用程序 ID、回复 URL、客户端密码/密钥和应用 ID URI。

- 将在代码中请求访问的资源与“所需资源”选项卡中的已配置权限进行比较，确保仅请求已配置的资源。

- 有关任何类似的错误或问题，请参阅 [Azure AD StackOverflow](https://stackoverflow.com/questions/tagged/azure-active-directory)。

## <a name="next-steps"></a>后续步骤

[Azure AD 开发人员指南](/active-directory/develop/active-directory-developers-guide)<br>

[同意并将应用集成到 Azure AD](/active-directory/develop/active-directory-integrating-applications)<br>

[Azure AD StackOverflow](https://stackoverflow.com/questions/tagged/azure-active-directory)

<!-- Update_Description: link update -->