---
title: 受保护的 Web API - 概述 | Azure
description: 了解如何构建受保护的 Web API（概述）。
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
origin.date: 05/07/2019
ms.date: 06/20/2019
ms.author: v-junlch
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: c55e99795d2d0698ed314e7c05b8ae65e9b8cbcd
ms.sourcegitcommit: 9d5fd3184b6a47bf3b60ffdeeee22a08354ca6b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2019
ms.locfileid: "67305879"
---
# <a name="scenario-protected-web-api"></a>方案：受保护的 Web API

在此方案中，我们将展示如何公开 Web API 以及如何对其进行保护，以便只有经过身份验证的用户才能访问该 API。 你将希望允许具有工作和学校帐户的经过身份验证的用户使用你的 Web API。

## <a name="prerequisites"></a>先决条件

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="specifics"></a>详情

以下是保护 Web API 需要了解的一些详细信息：

- 你的应用注册必须至少公开一个范围。 Web API 接受的令牌版本取决于登录受众。
- Web API 的代码配置必须验证调用 Web API 时使用的令牌。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [应用注册](scenario-protected-web-api-app-registration.md)

