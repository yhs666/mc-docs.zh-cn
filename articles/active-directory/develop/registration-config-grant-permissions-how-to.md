---
title: 如何向自定义开发的应用程序授予权限 | Microsoft Docs
description: 如何使用 Azure AD 门户或 URL 参数向自定义开发的应用程序授予权限
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
ms.date: 10/08/2018
ms.author: v-junlch
ms.openlocfilehash: 17a7d74dbf0b07df65f0be73669a60f0082f8225
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52658728"
---
# <a name="how-to-grant-permissions-to-a-custom-developed-application"></a>如何向自定义开发的应用程序授予权限

如果希望在应用上抢先授予同意，或在未同意应用时报错，请尝试执行以下步骤。

## <a name="how-to-perform-admin-consent-for-your-application"></a>如何为应用程序执行管理员许可

这等同于为组织中所有用户授予对应用程序的同意。

1. 以“全局管理员”身份导航到“应用注册”边栏选项卡，并选择应用。

2. 选择“所需的权限”，最后点击边栏选项卡顶部的“授予权限”按钮。

此外，也可通过应用配置来构造对 *login.partner.microsoftonline.cn* 的请求，然后将其追加到 *&prompt=admin\_consent*。 使用管理员凭据登录后，该应用即已授予所有用户的同意。

## <a name="how-to-force-user-consent-for-your-application"></a>如何为应用程序强制执行用户同意

- 附加到身份验证请求 &prompt=consent，此验证要求最终用户在每次进行身份验证时同意。

## <a name="next-steps"></a>后续步骤

[同意并将应用集成到 AzureAD](/active-directory/develop/active-directory-integrating-applications)

[AzureAD StackOverflow](http://stackoverflow.com/questions/tagged/azure-active-directory)

