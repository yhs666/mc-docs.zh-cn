---
title: 登录到应用程序时出现的意外许可提示 | Microsoft Docs
description: 如何对用户遇到的与 Azure AD 集成的应用程序出现意外许可提示这一情形进行故障排除
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
ms.date: 09/06/2018
ms.author: v-junlch
ms.reviewer: asteen
ms.openlocfilehash: e039b8f431bf50cd99f3d6dc0e1280de983c0350
ms.sourcegitcommit: fd49281c58f34de20cc310d6cefb4568992cd675
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/06/2018
ms.locfileid: "43858423"
---
# <a name="unexpected-consent-prompt-when-signing-in-to-an-application"></a>登录到应用程序时出现的意外许可提示

许多与 Azure Active Directory 集成的应用程序都需要各种资源的权限才能运行。 当这些资源也与 Azure Active Directory 集成时，使用 Azure AD 许可框架请求其访问权限。 

这会导致首次使用应用程序时显示许可提示，这通常是一次性操作。 

## <a name="scenarios-in-which-users-see-consent-prompts"></a>用户看到许可提示的情况

可能在各种情况下看到其他提示：

- 应用程序所需的权限集已更改。

- 最初对应用程序进行许可的用户不是管理员，现在其他（非管理员）用户首次使用该应用程序。

- 最初对应用程序进行许可的用户是管理员，但他们未代表整个组织进行许可。

- 已在最初授予许可后将其吊销。

- 开发人员已对应用程序作了如下配置：每次使用时，都需要许可提示（注意：这并非最佳做法）。

## <a name="next-steps"></a>后续步骤

-   [Azure Active Directory（v1.0 终结点）中的应用、权限和许可](/active-directory/develop/active-directory-integrating-applications)

<!-- Update_Description: update metedata properties -->