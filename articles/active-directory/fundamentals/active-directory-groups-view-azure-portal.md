---
title: 快速入门：使用 Azure Active Directory 搜索并查看组和分配的成员 | Microsoft Docs
description: 本快速入门提供有关如何使用 Azure Active Directory 搜索并查看所有组及其分配的成员的步骤。
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: quickstart
origin.date: 08/30/2018
ms.date: 10/09/2018
ms.author: v-junlch
ms.custom: it-pro
ms.reviewer: krbain
ms.openlocfilehash: 08eb41c2fcf5c007e9ebd645740f700ac55583c8
ms.sourcegitcommit: d8b4e1fbda8720bb92cc28631c314fa56fa374ed
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/10/2018
ms.locfileid: "48913750"
---
# <a name="quickstart-search-for-a-specific-group-and-view-its-members-using-azure-active-directory"></a>快速入门：使用 Azure Active Directory 搜索特定组并查看其成员
可以使用 Azure Active Directory 搜索特定组并查看分配的成员。

在此快速入门中，你将查看所有现有组，选择“MDM 策略 - 西部”组，然后查看分配的成员。

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial/)。 

## <a name="prerequisites"></a>先决条件
在开始之前，需要：

1. 创建一个 Azure Active Directory 租户。 有关详细信息，请参阅[访问 Azure Active Directory 门户并创建新租户](active-directory-access-create-new-tenant.md)。

2.  创建 _MDM 策略 - 西部_组，并添加成员 _Alain Charon_。 有关详细信息，请参阅[创建基本组并添加成员](active-directory-groups-create-azure-portal.md)。

## <a name="sign-in-to-the-azure-portal"></a>登录到 Azure 门户
必须使用目录的全局管理员帐户登录到 [Azure 门户](https://portal.azure.cn/)。

## <a name="view-your-existing-groups"></a>查看现有组
可在 Azure 门户的“组 - 所有组”页中查看组织的所有组。

- 依次选择“Azure Active Directory”、“组”。

    此时会出现“组 - 所有组”页，其中显示了所有活动的组。

    ![“组 - 所有组”页，其中显示了所有现有的组](./media/active-directory-groups-view-azure-portal/groups-all-groups-blade-with-all-groups.png)

## <a name="search-for-a-specific-group"></a>搜索特定组
搜索“组 - 所有组”页，找到“MDM 策略 - 西部”组。

1. 在“组 - 所有组”页上的“搜索”框中键入 _MDM_。

    搜索结果将显示在“搜索”框下面，其中包括“MDM 策略 - 西部”组。

    ![已填写搜索框的“组 - 所有组”页](./media/active-directory-groups-view-azure-portal/search-for-specific-group.png)

3. 选择“MDM 策略 - 西部”组。

4. 在“MDM 策略 - 西部概述”页上查看组信息，包括该组的成员数目。

    ![包含成员信息的“MDM 策略 - 西部概述”页](./media/active-directory-groups-view-azure-portal/group-overview-blade.png)

## <a name="view-members-of-the-mdm-policy---west-group"></a>查看“MDM 策略 - 西部”组的成员
找到组后，可以查看所有已分配的成员。

- 从“管理”区域中选择“成员”，然后查看分配到该特定组的完整成员名称列表，包括“Alain Charon”。

    ![分配到“MDM 策略 - 西部”组的成员列表](./media/active-directory-groups-view-azure-portal/groups-all-members.png)

## <a name="clean-up-resources"></a>清理资源
如果你不打算继续使用此应用程序，可以使用以下步骤删除该组及其分配的成员：

1. 在“组 - 所有组”页上，搜索“MDM 策略 - 西部”组。

2.  选择“MDM 策略 - 西部”组。

    此时会显示“MDM 策略 - 西部概述”页。

3. 选择“删除” 。

    随即会删除该组及其关联的成员。

    ![“MDM 策略 - 西部概述”页，其中突出显示了“删除”链接](./media/active-directory-groups-view-azure-portal/group-overview-blade-delete.png)

    >[!Important]
    >这不会删除用户 Alain Charon，而只会删除他在该组中的成员身份。

## <a name="next-steps"></a>后续步骤
找到组并查看成员后，可以继续执行以下操作：
- [添加或删除成员](active-directory-groups-members-azure-portal.md)

- [使用组来管理对资源的访问权限](active-directory-manage-groups.md)

- [添加组所有者](active-directory-accessmanagement-managing-group-owners.md)

<!-- Update_Description: wording update -->