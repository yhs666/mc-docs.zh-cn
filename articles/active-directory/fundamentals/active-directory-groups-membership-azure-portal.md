---
title: 如何在 Azure Active Directory 中的组中添加或删除其他组 | Microsoft Docs
description: 了解如何使用 Azure Active Directory 在组中添加或删除其他组。
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
origin.date: 08/28/2018
ms.date: 10/09/2018
ms.author: v-junlch
ms.custom: it-pro
ms.reviewer: krbain
ms.openlocfilehash: 51535943ec62fbef3d248fcc08d5718ac5ee551e
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52648163"
---
# <a name="how-to-add-or-remove-a-group-from-another-group-using-azure-active-directory"></a>如何：使用 Azure Active Directory 在组中添加或删除其他组
本文帮助你使用 Azure Active Directory 在组中添加和删除其他组。

>[!Note]
>如果尝试删除父组，请参阅[如何更新或删除组及其成员](active-directory-groups-delete-group.md)。

## <a name="add-a-group-as-a-member-to-another-group"></a>将一个组作为成员添加到另一个组
可以将现有组添加到另一个组，从而创建成员组（子组）和父组。 成员组继承父组的特性和属性，节省了配置时间。

### <a name="to-add-a-group-as-a-member-to-another-group"></a>要将组作为成员添加到其他组

1. 使用目录的全局管理员帐户登录到 [Azure 门户](https://portal.azure.cn)。

2. 选择“Azure Active Directory”，然后选择“组”。

3. 在“组 - 所有组”页面上，搜索并选择要成为另一个组的成员的组。 对于本练习，我们将使用“MDM 策略 - 西部”组。

    >[!Note]
    >一次只能将你的组作为成员添加到一个组。 另外，“选择组”框会通过将输入内容与用户或设备名称的任何部分进行匹配来筛选显示内容。 但是，不支持通配符字符。

    ![“组 - 所有组”页面，其中选择了“MDM 策略 - 西部”组](./media/active-directory-groups-membership-azure-portal/group-all-groups-screen.png)

4. 在“MDM 策略 - 西部 - 组成员身份”页面上，选择“组成员身份”，选择“添加”，找到要使你的组成为其成员的组，然后选择“选择”。 对于本练习，我们将使用“MDM 策略 - 所有组织”组。

    “MDM 策略 - 西部”组现在是“MDM 策略 - 所有组织”组的一个成员，继承了“MDM 策略 - 所有组织”组的所有属性和配置。

    ![通过将组添加到另一个组中创建组成员身份](./media/active-directory-groups-membership-azure-portal/add-group-membership.png)

5. 查看“MDM 策略 - 西部 - 组成员身份”页面来查看组和成员身份。

    ![“MDM 策略 - 西部 - 组成员身份”页面，其中显示了父组](./media/active-directory-groups-membership-azure-portal/group-membership-blade.png)

6. 若要获得组和成员身份的详细视图，请选择组名称（“MDM 策略 - 所有组织”）并查看“MDM 策略 - 西部”页面详细信息。

    ![组成员身份页面，其中显示了成员和组详细信息](./media/active-directory-groups-membership-azure-portal/group-membership-review.png)

## <a name="remove-a-member-group-from-another-group"></a>删除组中的成员组
可以删除组中的现有成员组。 但是，删除成员身份还会为用户删除任何继承的特性和属性。

### <a name="to-remove-a-member-group-from-another-group"></a>删除组中的成员组
1. 在“组 - 所有组”页面上，搜索并选择要删除的属于另一个组的成员的组。 对于本练习，我们再次使用“MDM 策略 - 西部”组。

2. 在“MDM 策略 - 西部”概述页面上，选择“组成员身份”。

    ![“MDM 策略 - 西部”概述页面](./media/active-directory-groups-membership-azure-portal/group-membership-overview.png)

3. 从“MDM 策略 - 西部 - 组成员身份”页面上选择“MDM 策略 - 所有组织”，然后从“MDM 策略 - 西部”页面详细信息中选择“删除”。

    ![显示成员和组详细信息的“组成员身份”页](./media/active-directory-groups-membership-azure-portal/group-membership-remove.png)


## <a name="additional-information"></a>其他信息
这些文章提供了有关 Azure Active Directory 的更多信息。

- [查看组和成员](active-directory-groups-view-azure-portal.md)

- [创建基本组并添加成员](active-directory-groups-create-azure-portal.md)

- [从组中添加或删除成员](active-directory-groups-members-azure-portal.md)

- [编辑组设置](active-directory-groups-settings-azure-portal.md)

<!-- Update_Description: wording update -->