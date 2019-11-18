---
title: 使用 Privileged Identity Management 所要满足的许可证要求 - Azure Active Directory | Microsoft Docs
description: 介绍使用 Azure AD Privileged Identity Management (PIM) 所要满足的许可要求。
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: markwahl-msft
ms.assetid: 34367721-8b42-4fab-a443-a2e55cdbf33d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: pim
origin.date: 10/23/2019
ms.date: 11/05/2019
ms.author: v-junlch
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 90339f32434cea4cfbdc784c8811baf32eb906b6
ms.sourcegitcommit: a88cc623ed0f37731cb7cd378febf3de57cf5b45
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/08/2019
ms.locfileid: "73830732"
---
# <a name="license-requirements-to-use-privileged-identity-management"></a>使用 Privileged Identity Management 所要满足的许可证要求

若要使用 Azure Active Directory (Azure AD) Privileged Identity Management (PIM)，目录必须具有有效的许可证。 此外，必须将许可证分配给管理员和相关用户。 本文介绍使用 Privileged Identity Management 所要满足的许可证要求。

## <a name="prerequisites"></a>先决条件

若要使用 Privileged Identity Management，目录必须具有以下付费或试用许可证之一：

- Azure AD Premium P2
- 企业移动性 + 安全性 (EMS) E5
- Microsoft 365 M5

有关详细信息，请参阅[什么是 Azure Active Directory？](../fundamentals/active-directory-whatis.md)。

## <a name="which-users-must-have-licenses"></a>哪些用户必须有许可证？

与 Privileged Identity Management 交互或者想要获得 Privileged Identity Management 优势的每个管理员或用户必须有一个许可证。 示例包括：

- 其 Azure AD 角色由 PIM 管理的管理员
- 其 Azure 资源角色由 PIM 管理的管理员
- 已分配到“特权角色管理员”角色的管理员
- 已分配到使用 PIM 管理的 Azure AD 角色的合格用户
- 能够在 PIM 中批准/拒绝请求的用户
- 已通过实时或直接（基于时间）分配方法分配到 Azure 资源角色的用户  

有关如何将许可证分配给用户的信息，请参阅[使用 Azure Active Directory 门户分配或删除许可证](../fundamentals/license-users-groups.md)。

## <a name="what-happens-when-a-license-expires"></a>许可证过期时会发生什么情况？

如果 Azure AD Premium P2、EMS E5 或试用许可证过期，则不再可以在目录中使用 Privileged Identity Management 功能：

- 对 Azure AD 角色的永久角色分配会受到影响。
- 用户不再可以使用 Azure 门户中的 Privileged Identity Management 服务以及 Privileged Identity Management 的图形 API cmdlet 和 PowerShell 接口来激活特权角色、管理特权访问权限。
- 将删除 Azure AD 角色的符合条件的角色分配，因为用户不再能够激活特权角色。
- 角色分配更改时，Privileged Identity Management 将不再发送电子邮件。

## <a name="next-steps"></a>后续步骤

- [部署 Privileged Identity Management](pim-deployment-plan.md)
- [开始使用 Privileged Identity Management](pim-getting-started.md)
- [无法在 Privileged Identity Management 中管理的角色](pim-roles.md)

<!-- Update_Description: wording update -->