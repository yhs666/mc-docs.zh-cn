---
title: 添加或更新用户的配置文件信息 - Azure Active Directory | Microsoft Docs
description: 有关如何在 Azure Active Directory 中向用户配置文件添加信息（包括图片和作业详细信息）的说明。
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
origin.date: 09/05/2018
ms.date: 01/02/2019
ms.author: v-junlch
ms.reviewer: jeffsta
ms.openlocfilehash: 42569322f00e790ab1d272dc7d04fabf57682454
ms.sourcegitcommit: 4f91d9bc4c607cf254479a6e5c726849caa95ad8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2019
ms.locfileid: "53996287"
---
# <a name="add-or-update-a-users-profile-information-using-azure-active-directory"></a>使用 Azure Active Directory 添加或更新用户的配置文件信息
使用 Azure Active Directory (Azure AD) 添加用户个人资料信息，包括个人资料图片、工作特定信息以及某些设置。 有关添加新用户的详细信息，请参阅[如何在 Azure Active Directory 中添加或删除用户](add-users-azure-active-directory.md)。

## <a name="add-or-change-profile-information"></a>添加或更改个人资料信息
如你所见，用户的个人资料中提供的信息要多于你在创建用户期间能够添加的信息。 所有这些额外的信息都是可选的，可以由你的组织根据需要进行添加。

## <a name="to-add-or-change-profile-information"></a>添加或更改个人资料信息
1. 以目录的全局管理员或用户管理员身份登录到 [Azure 门户](https://portal.azure.cn/)。

2. 依次选择“Azure Active Directory”、“用户”，然后选择一个用户。 例如，_Alain Charon_。

    此时将出现“Alain Charon - 个人资料”页面。

    ![用户的个人资料页面，其中包括可编辑的信息](./media/active-directory-users-profile-azure-portal/user-profile-all-blade.png)

3. 选择“编辑”以有选择地添加或更新每个可用部分中包括的信息。

    ![用户的个人资料页面，其中显示了可编辑的区域](./media/active-directory-users-profile-azure-portal/user-profile-edit.png)

    - **个人资料图片。** 为用户的帐户选择一个缩略图图像。 此图片将显示在 Azure Active Directory 中和用户的个人页面上。

    - **标识。** 添加任何帐户相关信息，例如，婚后姓氏或更改的用户名。 

    - **工作信息。** 添加任何工作相关信息，例如，用户的职位、部门或经理。

    - **设置。** 决定用户是否可以登录到 Azure Active Directory 租户。 还可以指定用户的全局位置。

    - **联系信息。** 为用户添加任何相关的联系信息。 例如，街道地址或移动电话号码。

    - **身份验证联系信息。** 请验证此信息，以确保用户具有处于活动状态的电话号码和电子邮件地址。 此信息由 Azure Active Directory 在登录期间用来确保用户确实是该用户。 只有全局管理员可以更新身份验证联系信息。

4. 选择“其他安全性验证” 。

    为用户保存所做的所有更改。

    >[!Note]
    >必须使用 Windows Server Active Directory 更新其授权来源为 Windows Server Active Directory 的用户的标识、联系信息或工作信息。 完成更新后，必须等待下一个同步周期完成才能看到更改。

## <a name="next-steps"></a>后续步骤
更新用户的个人资料后，可以执行以下基本流程：

- [添加或删除用户](add-users-azure-active-directory.md)

- [向用户分配角色](active-directory-users-assign-role-azure-portal.md)

- [创建基本组并添加成员](active-directory-groups-create-azure-portal.md)

另外，你可以执行其他用户管理任务，例如，分配委托、使用策略以及共享用户帐户。 有关其他可用操作的详细信息，请参阅 [Azure Active Directory 用户管理和文档](../users-groups-roles/index.yml)。

<!-- Update_Description: wording update -->