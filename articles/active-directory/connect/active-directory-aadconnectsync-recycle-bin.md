---
title: Azure AD Connect 同步：启用 AD 回收站 | Microsoft Docs
description: 本主题提供有关使用 Azure AD Connect 的 AD 回收站功能的建议。
services: active-directory
keywords: AD 回收站, 意外删除, 源定位点
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: afec4207-74f7-4cdd-b13a-574af5223a90
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 07/13/2017
ms.date: 06/25/2018
ms.component: hybrid
ms.author: v-junlch
ms.openlocfilehash: 3cb2ce5b84aff58d92314a6208d423132547cf87
ms.sourcegitcommit: 8b36b1e2464628fb8631b619a29a15288b710383
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/26/2018
ms.locfileid: "36947928"
---
# <a name="azure-ad-connect-sync-enable-ad-recycle-bin"></a>Azure AD Connect 同步：启用 AD 回收站
建议为同步到 Azure AD 的本地 Active Directory 启用 AD 回收站功能。 

如果用户意外删除了某个本地 AD 用户对象，并且该功能还原该对象，Azure AD 会还原相应的 Azure AD 用户对象。  有关 AD 回收站功能的信息，请参阅[有关还原已删除的 Active Directory 对象的方案概述](https://technet.microsoft.com/library/dd379542.aspx)一文。

## <a name="benefits-of-enabling-the-ad-recycle-bin"></a>启用 AD 回收站的好处
此功能通过以下方式帮助还原 Azure AD 用户对象：

- 如果你意外删除了某个本地 AD 用户对象，相应的 Azure AD 用户对象会在下一同步周期被删除。 默认情况下，Azure AD 会以软删除状态保存已删除的 Azure AD 用户对象 30 天。

- 如果已启用本地 AD 回收站功能，无需更改已删除的本地 AD 用户对象的“源定位点”值，即可将它还原。 将恢复的本地 AD 用户对象同步到 Azure AD 后，Azure AD 将还原已软删除的相应 Azure AD 用户对象。 有关“源定位点”属性的信息，请参阅 [Azure AD Connect：设计概念](/active-directory/connect/active-directory-aadconnect-design-concepts#sourceanchor)一文。

- 如果未启用本地 AD 回收站功能，可能需要创建一个 AD 用户对象来替换已删除的对象。 如果 Azure AD Connect 同步服务配置为对“源定位点”属性使用系统生成的 AD 属性（例如 ObjectGuid），则新建的 AD 用户对象与已删除的 AD 用户对象的“源定位点”值不相同。 新建的 AD 用户对象同步到 Azure AD 后，Azure AD 会创建新的 Azure AD 用户对象，而不是还原处于软删除状态的 Azure AD 用户对象。

> [!NOTE]
> 默认情况下，Azure AD 会将处于软删除状态中的已删除 Azure AD 用户对象保留 30 天，此期限过后，会永久删除这些对象。 但是，管理员可以提前删除此类对象。 永久删除这些对象后，即使已启用在本地 AD 回收站功能，也不再可以恢复它们。



## <a name="next-steps"></a>后续步骤
**概述主题**

- [Azure AD Connect 同步：理解和自定义同步](active-directory-aadconnectsync-whatis.md)

- [将本地标识与 Azure Active Directory 集成](active-directory-aadconnect.md)

<!-- Update_Description: update metedata properties -->