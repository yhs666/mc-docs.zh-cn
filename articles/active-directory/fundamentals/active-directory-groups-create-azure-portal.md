---
title: 如何使用 Azure Active Directory 创建基本组并添加成员 | Microsoft Docs
description: 了解如何使用 Azure Active Directory 创建基本组。
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: quickstart
origin.date: 08/22/2018
ms.date: 10/09/2018
ms.author: v-junlch
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: df0e6de01d5dbe47124388e94b767df0cad95573
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52659772"
---
# <a name="how-to-create-a-basic-group-and-add-members-using-azure-active-directory"></a>如何：使用 Azure Active Directory 创建基本组并添加成员

可以使用 Azure Active Directory (Azure AD) 门户创建基本组。 对于本文而言，将由资源所有者（管理员）向单个资源中添加一个基本组，该组中将包括需要访问该资源的特定成员（员工）。 对于更复杂的方案，请参阅 [Azure Active Directory 用户管理文档](../users-groups-roles/index.yml)。

## <a name="create-a-basic-group-and-add-members"></a>创建基本组并添加成员
可以同时创建基本组并添加成员。

### <a name="to-create-a-basic-group-and-add-members"></a>创建基本组并添加成员
1. 使用目录的全局管理员帐户登录到 [Azure 门户](https://portal.azure.cn)。

2. 依次选择“Azure Active Directory”、“组”、“新建组”。

    ![显示组的 Azure AD](./media/active-directory-groups-create-azure-portal/group-full-screen.png)

3. 在“组”页面上，填写所需的信息。

    ![“新建组”页面，其中填写了示例信息](./media/active-directory-groups-create-azure-portal/new-group-blade.png)

    - **组类型（必填）。** 选择一个预定义的组类型。 这包括：
        
        - **安全性**。 用来为一组用户管理成员和计算机对共享资源的访问权限。 例如，你可以创建一个安全组来实施特定的安全策略。 这样，你可以一次向所有成员授予一组权限，而不需要分别为每个成员添加权限。 有关管理对资源的访问权限的详细信息，请参阅[使用 Azure Active Directory 组管理对资源的访问权限](active-directory-manage-groups.md)。
        
        - **Office 365**。 通过向成员授予对共享邮箱、日历、文件、SharePoint 站点和其他内容的访问权限，提供了协作机会。 此选项还允许向组织外部的人授予对该组的访问权限。 有关 Office 365 组的详细信息，请参阅[了解 Office 365 组](https://support.office.com/article/learn-about-office-365-groups-b565caa1-5c40-40ef-9915-60fdb2d97fa2)。

    - **组名称（必填）。** 添加组名称，可以使用容易记住且具有某种意义的名称作为组名称。

    - **组说明。** 向组添加说明（可选）。

    - **成员身份类型（必填）。** 选择一个预定义的成员身份类型。 这包括：

        - **已分配。** 允许将特定用户添加为该组的成员并获得独特权限。 对于本文，我们将使用此选项。

4. 选择“创建” 。

    随即将创建组，该组将准备就绪，可供添加成员。

5. 从“组”页面选择“成员”区域，然后从“选择成员”页面中开始搜索要添加到组的成员。

    ![在组创建过程中选择你的组成员](./media/active-directory-groups-create-azure-portal/select-members-create-group.png)

6. 完成添加成员后，选择“选择”。

    “组概述”页面将更新，以显示当前添加到组的成员数。

    ![“组概述”页面，其中突出显示了成员数](./media/active-directory-groups-create-azure-portal/group-overview-blade-number-highlight.png)

## <a name="next-steps"></a>后续步骤
现在，你已添加了一个组和至少一个用户，你可以：

- [查看组和成员](active-directory-groups-view-azure-portal.md)

- [管理组成员身份](active-directory-groups-membership-azure-portal.md)

- [编辑组设置](active-directory-groups-settings-azure-portal.md)

- [使用组管理对资源的访问权限](active-directory-manage-groups.md)

- [使用 PowerShell 命令管理组](../users-groups-roles/groups-settings-v2-cmdlets.md)
 
<!-- Update_Description: wording update -->
