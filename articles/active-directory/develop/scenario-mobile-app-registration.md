---
title: 调用 Web API 的移动应用 - 应用的代码配置 | Microsoft 标识平台
description: 了解如何构建调用 Web API 的移动应用（应用的代码配置）
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
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
ms.reviwer: brandwe
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: dfe65755f49806ca99a954296a989e287f64139f
ms.sourcegitcommit: 9d5fd3184b6a47bf3b60ffdeeee22a08354ca6b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2019
ms.locfileid: "67305849"
---
# <a name="mobile-app-that-calls-web-apis---app-registration"></a>调用 Web API 的移动应用 - 应用注册

本文包含用于创建移动应用程序的应用注册说明。

## <a name="supported-account-types"></a>支持的帐户类型

在移动应用程序中支持的帐户类型取决于你想要启用的体验，以及应用的目标用户。

## <a name="platform-configuration-and-redirect-uris"></a>平台配置和重定向 URI  

构建移动应用时，最关键的注册步骤是重定向 URI。 可以通过[“身份验证”边栏选项卡中的平台配置](https://portal.azure.cn/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview)对此进行设置。

此体验将允许应用通过 Microsoft Authenticator（以及 Android 上的 Intune 公司门户）获取单一登录 (SSO)，以及支持设备管理策略。

如果首选以手动方式配置重定向 URI，则可通过应用程序清单来这样做。 建议的格式如下：

- ***iOS***：`msauth.<BUNDLE_ID>://auth`
- ***Android***：`msauth://<PACKAGE_NAME>/<SIGNATURE_HASH>`
  - Android 签名哈希可以通过 KeyTool 命令使用释放键或调试键来生成。

## <a name="api-permissions"></a>API 权限

移动应用程序代表已登录用户调用 API。 应用需要请求委托的权限（也称为作用域）。 根据所需体验，这可以通过 Azure 门户静态完成，也可以在运行时动态完成。 静态注册权限方便管理员批准应用，值得推荐。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [获取令牌](scenario-mobile-acquire-token.md)

