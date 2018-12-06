---
title: 将 Active Directory 与 Azure Active Directory 连接。 | Microsoft 文档
description: Azure AD Connect 会将本地目录与 Azure Active Directory 集成。 这样便可以为集成到 Azure AD 的 Office 365、Azure 和 SaaS 应用程序提供一个通用标识。
keywords: Azure AD Connect 介绍, Azure AD Connect 概述, 什么是 Azure AD Connect, 安装 active directory
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: 59bd209e-30d7-4a89-ae7a-e415969825ea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
origin.date: 09/18/2018
ms.date: 11/12/2018
ms.component: hybrid
ms.author: v-junlch
ms.openlocfilehash: 8bf3eced55b250624a34cd0aa0fc51b5119961d3
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52653667"
---
# <a name="hybrid-identity-and-microsofts-identity-solutions"></a>混合标识和 Microsoft 的标识解决方案
如今，企业和公司越来越成为本地应用程序和云应用程序的混合体。  如果应用程序和用户需要访问位于本地和云中的那些应用程序，这将成为一个具有挑战性的场景。

Microsoft 的标识解决方案跨越本地和基于云的功能，创建单一用户标识对所有资源进行身份验证和授权，而不考虑其位置。 我们称此为混合标识。

## <a name="what-is-azure-ad-connect"></a>什么是 Azure AD Connect？

Azure AD Connect 专用于满足和完成混合标识目标的 Microsoft 工具。  这样便可以为集成到 Azure AD 的 Office 365、Azure 应用程序的用户提供一个通用标识。  它提供以下功能：
    
- [同步](how-to-connect-sync-whatis.md) - 此组件负责创建用户、组和其他对象。 它还负责确保本地用户和组的标识信息与云匹配。  它负责将密码哈希与 Azure AD 进行同步。
-   [AD FS 和联合身份验证集成](how-to-connect-fed-whatis.md) - 联合身份验证是 Azure AD Connect 的可选部件，可用于使用本地 AD FS 基础结构配置混合环境。 它还提供了 AD FS 管理功能，例如证书续订和其他 AD FS 服务器部署。

![什么是 Azure AD Connect](./media/whatis-hybrid-identity/arch.png)
 
## <a name="why-use-azure-ad-connect"></a>为何使用 Azure AD Connect？
将本地目录与 Azure AD 集成可提供用于访问云和本地资源的通用标识，来提高用户的工作效率。 用户和组织可以享受到以下好处：

- 用户可以使用单个标识来访问本地应用程序和云服务，例如 Office 365。
- 单个工具即可提供轻松同步和登录的部署体验。

## <a name="next-steps"></a>后续步骤

- [硬件和先决条件](how-to-connect-install-prerequisites.md) 
- [快速设置](how-to-connect-install-express.md)
- [自定义设置](how-to-connect-install-custom.md)
- [密码哈希同步](how-to-connect-password-hash-synchronization.md)|
- [Azure AD Connect 和联合身份验证](how-to-connect-fed-whatis.md)
- [Azure AD Connect 同步](how-to-connect-sync-whatis.md)
- [版本历史记录](reference-connect-version-history.md)
- [Azure AD Connect 常见问题解答](reference-connect-faq.md)










