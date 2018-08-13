---
title: 在 Azure AD 中创建用户组 | Microsoft Docs
description: 如何在 Azure Active Directory 中创建组并向该组添加成员
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: quickstart
origin.date: 08/04/2017
ms.date: 08/07/2018
ms.author: v-junlch
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: 8bd01b10acd197a9e785288d4e47378ba9127ae1
ms.sourcegitcommit: 7cdf4633aea04e524cb48cb1990b750ae8be841c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/07/2018
ms.locfileid: "39584235"
---
# <a name="create-a-group-and-add-members-in-azure-active-directory"></a>在 Azure Active Directory 中创建组并添加成员
> [!div class="op_single_selector"]
> * [Azure 门户](active-directory-groups-create-azure-portal.md)
> * [PowerShell](../users-groups-roles/groups-settings-v2-cmdlets.md)

本文介绍如何在 Azure Active Directory 中创建和填充新组。 使用组来执行管理任务，例如一次向多个用户或设备分配许可证或权限。

## <a name="how-do-i-create-a-group"></a>如何创建组？
1. 使用目录全局管理员的帐户登录到 [Azure 门户](https://portal.azure.cn) 。
2. 选择“所有服务”，在文本框中输入“组”，然后按 **Enter**。

3. 在“组”边栏选项卡中，选择“所有组”。

   ![打开“组”边栏选项卡](./media/active-directory-groups-create-azure-portal/view-groups-blade.png)
4. 在“组 - 所有组”边栏选项卡中，选择“添加”命令。

   ![选择“添加”命令](./media/active-directory-groups-create-azure-portal/add-group-command.png)
5. 在“组”边栏选项卡中，为组添加名称和描述。
6. 要选择要添加到组的成员，请在“成员身份类型”框中选择“已分配”，并选择“成员”。 

   ![选择要添加的成员](./media/active-directory-groups-create-azure-portal/select-members.png)
7. 在“成员”边栏选项卡中，选择一个或多个要添加到组的用户或设备，然后选择边栏选项卡底部的“选择”按钮以将它们添加到组。 “用户”框会通过将输入内容与用户或设备名称的任何部分进行匹配来筛选显示内容。 该框中不允许使用任何通配符。
8. 完成向组添加成员后，请在“组”边栏选项卡中选择“创建”。    

   ![创建组确认](./media/active-directory-groups-create-azure-portal/create-group-confirmation.png)


## <a name="next-steps"></a>后续步骤
这些文章提供了有关 Azure Active Directory 的更多信息。

- [查看现有组](active-directory-groups-view-azure-portal.md)
- [管理组的设置](active-directory-groups-settings-azure-portal.md)
- [管理组的成员](active-directory-groups-members-azure-portal.md)
- [管理组的成员身份](active-directory-groups-membership-azure-portal.md)

<!-- Update_Description: link update -->