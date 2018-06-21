---
title: 登录自定义开发的应用程序时出现的问题 | Azure
description: 可能导致用户无法登录到已使用 Azure AD 开发的应用程序的常见错误
services: active-directory
documentationcenter: ''
author: yunan2016
manager: digimobile
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 07/11/2017
ms.date: 1/1/2018
ms.author: v-nany
ms.openlocfilehash: 3603a878539d368e3626ea892fee8cd57549e4ea
ms.sourcegitcommit: f02cdaff1517278edd9f26f69f510b2920fc6206
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/05/2018
ms.locfileid: "27604193"
---
# <a name="problems-signing-in-to-an-custom-developed-application"></a>登录自定义开发的应用程序时出现的问题

存在多个错误可能导致用户无法登录到应用。 出现此问题的最主要原因是应用配置错误。

## <a name="errors-related-to--misconfigured-apps"></a>与应用配置错误相关的错误

* 确认门户中的配置与应用中的配置相匹配。 具体而言，比较客户端/应用程序 ID、回复 URL、客户端密码/密钥和应用 ID URI。

* 将在代码中请求访问的资源与“所需资源”选项卡中的已配置权限进行比较，确保仅请求已配置的资源。


## <a name="next-steps"></a>后续步骤

[Azure AD 开发人员指南](./develop/active-directory-developers-guide.md)<br>

[同意并将应用集成到 Azure AD](./develop/active-directory-integrating-applications.md)<br>




