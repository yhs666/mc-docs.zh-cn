---
title: "登录到应用程序时出现的意外许可提示 | Azure"
description: "如何对用户遇到的与 Azure AD 集成的应用程序出现意外许可提示这一情形进行故障排除"
services: active-directory
documentationcenter: 
author: yunan2016
manager: digimobile
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 07/11/2017
ms.date: 1/1/2018
ms.author: v-nany
ms.openlocfilehash: 8ced446b5ba81a62c6f15d4709bb5415452c6de5
ms.sourcegitcommit: 40b20646a2d90b00d488db2f7e4721f9e8f614d5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/12/2018
---
# <a name="unexpected-consent-prompt-when-signing-in-to-an-application"></a>登录到应用程序时出现的意外许可提示

许多与 Azure Active Directory 集成的应用程序都需要各种资源的权限才能运行。 当这些资源也与 Azure Active Directory 集成时，使用 Azure AD 许可框架请求其访问权限。 

这会导致首次使用应用程序时显示许可提示，这通常是一次性操作。 

## <a name="scenarios-in-which-users-see-consent-prompts"></a>用户看到许可提示的情况

可能在各种情况下看到其他提示：

* 应用程序所需的权限集已更改。

* 最初对应用程序进行许可的用户不是管理员，现在其他（非管理员）用户首次使用该应用程序。

* 最初对应用程序进行许可的用户是管理员，但他们未代表整个组织进行许可。


* 已在最初授予许可后将其吊销。

* 开发人员已对应用程序作了如下配置：每次使用时，都需要许可提示（注意：这并非最佳做法）。


