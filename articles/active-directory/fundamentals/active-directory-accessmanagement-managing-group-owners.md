---
title: 如何添加或删除 Azure Active Directory 组所有者 | Microsoft Docs
description: 了解如何使用 Azure Active Directory 添加或删除组所有者。
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
origin.date: 09/11/2018
ms.date: 11/12/2018
ms.author: v-junlch
ms.custom: it-pro
ms.openlocfilehash: 66b5c006674b2acce14e7560d4e1deea1ed1ecf0
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52647735"
---
# <a name="how-to-add-or-remove-group-owners-in-azure-active-directory"></a>如何：在 Azure Active Directory 中添加或删除组所有者
Azure Active Directory (Azure AD) 组由组所有者拥有和管理。 组所有者是由资源所有者（管理员）分配的，用来管理组及其成员。 组所有者并非必须是组的成员。 在分配组所有者后，只有资源所有者可以添加或删除这些所有者。

某些情况下，你作为管理员可能会决定不分配组所有者。 在这种情况下，你将成为组所有者。 另外，所有者可以为其组分配其他所有者，除非你在组设置中限制了此权限。

## <a name="add-an-owner-to-a-group"></a>向组添加所有者
使用 Azure AD 向组添加其他组所有者。

### <a name="to-add-a-group-owner"></a>添加组所有者
1. 使用目录的全局管理员帐户登录到 [Azure 门户](https://portal.azure.cn)。

2. 依次选择“Azure Active Directory”、“组”以及要添加所有者的组（例如此示例为“MDM 策略 - 西部”）。

3. 在“MDM 策略 - 西部概述”页面上选择“所有者”。

    ![“MDM 策略 - 西部概述”页，其中突出显示了“所有者”选项](./media/active-directory-accessmanagement-managing-group-owners/add-owners-option-overview-blade.png)

4. 在“MDM 策略 - 西部 - 所有者”页面上，选择“添加所有者”，搜索并选择将成为新的组所有者的用户，然后选择“选择”。

    ![“MDM 策略 - 西部 - 所有者”页面，其中突出显示了“添加所有者”选项](./media/active-directory-accessmanagement-managing-group-owners/add-owners-owners-blade.png)

    选择新的所有者后，可以刷新“所有者”页面，并且会看到该名称已添加到所有者列表中。

## <a name="remove-an-owner-from-a-group"></a>删除组所有者
使用 Azure AD 从组中删除所有者

### <a name="to-remove-an-owner"></a>删除所有者
1. 使用目录的全局管理员帐户登录到 [Azure 门户](https://portal.azure.cn)。

2. 依次选择“Azure Active Directory”、“组”以及要添加所有者的组（例如此示例为“MDM 策略 - 西部”）。

3. 在“MDM 策略 - 西部概述”页面上选择“所有者”。

    ![“MDM 策略 - 西部概述”页，其中突出显示了“所有者”选项](./media/active-directory-accessmanagement-managing-group-owners/remove-owners-option-overview-blade.png)

4. 在“MDM 策略 - 西部 - 所有者”页面上，选择要删除的作为组所有者的用户，从该用户的信息页面上选择“删除”，然后选择“是”来确认你的决策。

    ![用户的信息页面，其中突出显示了“删除”选项](./media/active-directory-accessmanagement-managing-group-owners/remove-owner-info-blade.png)

    删除所有者后，可以返回到“所有者”页面，并且会看到该名称已从所有者列表中删除。

## <a name="next-steps"></a>后续步骤
- [使用 Azure Active Directory 组管理对资源的访问](active-directory-manage-groups.md)

- [将本地标识与 Azure Active Directory 集成](../hybrid/whatis-hybrid-identity.md)

- [用于配置组设置的 Azure Active Directory cmdlet](../users-groups-roles/groups-settings-v2-cmdlets.md)

<!-- Update_Description: link update -->