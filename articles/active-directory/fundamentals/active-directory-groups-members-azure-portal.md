---
title: 添加或删除组成员 - Azure Active Directory | Microsoft Docs
description: 介绍如何使用 Azure Active Directory 在组中添加或删除组成员。
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
origin.date: 08/23/2018
ms.date: 01/02/2019
ms.author: v-junlch
ms.custom: it-pro, seodec18
ms.reviewer: krbain
ms.openlocfilehash: dd50e712fb231181559e8fc594fb5cc473408c61
ms.sourcegitcommit: 4f91d9bc4c607cf254479a6e5c726849caa95ad8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2019
ms.locfileid: "53996186"
---
# <a name="add-or-remove-group-members-using-azure-active-directory"></a>使用 Azure Active Directory 添加或删除组成员
使用 Azure Active Directory，可以继续添加和删除组成员。

## <a name="to-add-group-members"></a>添加组成员

1. 使用目录的全局管理员帐户登录到 [Azure 门户](https://portal.azure.cn)。

2. 选择“Azure Active Directory”，然后选择“组”。

3. 在“组 - 所有组”页中，搜索并选择要添加成员的组。 在此示例中，使用我们之前创建的组“MDM 策略 - 西部”。

    ![“组 - 所有组”页，其中突出显示了组名称](./media/active-directory-groups-members-azure-portal/group-all-groups-screen.png)

4. 在“MDM 策略 - 西部概述”页中，从“管理”区域选择“成员”。

    ![“MDM 策略 – 西部概述”页，其中突出显示了“成员”选项](./media/active-directory-groups-members-azure-portal/group-overview-blade.png)

5. 选择“添加成员”，然后搜索并选择要添加到该组的每个成员，然后选择“选择”。

    你将收到一条消息，指出成员已成功添加。

    ![“添加成员”页，其中显示了已搜索到的成员](./media/active-directory-groups-members-azure-portal/update-members.png)

6. 刷新屏幕以查看添加到组中的所有成员名称。

## <a name="to-remove-group-members"></a>删除组成员

1. 在“组 - 所有组”页中，搜索并选择要从中删除成员的组。 我们将再次使用“MDM 策略 - 西部”。

2. 从“管理”区域中选择“成员”，搜索并选择要删除的成员的名称，然后选择“删除”。

    ![带有“删除”选项的“成员信息”页](./media/active-directory-groups-members-azure-portal/remove-members-from-group.png)

## <a name="next-steps"></a>后续步骤

- [查看组和成员](active-directory-groups-view-azure-portal.md)

- [编辑组设置](active-directory-groups-settings-azure-portal.md)

- [使用组管理对资源的访问权限](active-directory-manage-groups.md)

<!-- Update_Description: wording update -->