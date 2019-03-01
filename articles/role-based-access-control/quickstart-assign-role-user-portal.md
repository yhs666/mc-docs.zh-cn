---
title: 教程 - 使用 RBAC 和 Azure 门户授予用户对 Azure 资源的访问权限 | Microsoft Docs
description: 了解如何使用 Azure 门户中的基于角色的访问控制 (RBAC) 授予用户对 Azure 资源的访问权限。
services: role-based-access-control
documentationCenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: tutorial
ms.tgt_pltfrm: ''
ms.workload: identity
origin.date: 11/30/2018
ms.date: 02/26/2019
ms.author: v-junlch
ms.openlocfilehash: 531ae99d7071b834dd054e3dffac36b04b7e5c4b
ms.sourcegitcommit: e9f088bee395a86c285993a3c6915749357c2548
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/26/2019
ms.locfileid: "56836888"
---
# <a name="tutorial-grant-a-user-access-to-azure-resources-using-rbac-and-the-azure-portal"></a>教程：使用 RBAC 和 Azure 门户授予用户对 Azure 资源的访问权限

可以通过[基于角色的访问控制 (RBAC)](overview.md) 方式管理对 Azure 资源的访问权限。 在本教程中，你将授权用户在某个资源组中创建和管理虚拟机。

本教程介绍如何执行下列操作：

> [!div class="checklist"]
> * 在资源组范围内为用户授予访问权限
> * 删除访问权限

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial/?WT.mc_id=A261C142F)。

## <a name="sign-in-to-azure"></a>登录 Azure

通过 http://portal.azure.cn 登录到 Azure 门户。

## <a name="create-a-resource-group"></a>创建资源组

1. 在导航列表中，选择“资源组”。

1. 选择“添加”以打开“资源组”边栏选项卡。

   ![添加新的资源组](./media/quickstart-assign-role-user-portal/resource-group.png)

1. 输入 **rbac-quickstart-resource-group** 作为**资源组名称**。

1. 选择订阅和位置。

1. 选择“创建”以创建资源组。

1. 选择“刷新”以刷新资源组列表。

   新资源组会显示在资源组列表中。

   ![资源组列表](./media/quickstart-assign-role-user-portal/resource-group-list.png)

## <a name="grant-access"></a>授予访问权限

在 RBAC 中，若要授予访问权限，请创建角色分配。

1. 在“资源组”列表中，选择这个新的 **rbac-quickstart-resource-group** 资源组。

1. 选择“访问控制(IAM)”。

1. 选择“角色分配”选项卡以查看当前的角色分配列表。

   ![资源组的“访问控制(IAM)”边栏选项卡](./media/quickstart-assign-role-user-portal/access-control.png)

1. 选择“添加角色分配”以打开“添加角色分配”窗格。

   如果没有分配角色的权限，则将禁用“添加角色分配”选项。

   ![“添加角色分配”窗格](./media/quickstart-assign-role-user-portal/add-role-assignment.png)

1. 在“角色”下拉列表中，选择“虚拟机参与者”。

1. 在“选择”列表中，选择你自己或其他用户。

1. 选择“保存”，创建角色分配。

   片刻之后，系统会在 rbac-quickstart-resource-group 资源组范围为该用户分配“虚拟机参与者”角色。

   ![“虚拟机参与者”角色的分配](./media/quickstart-assign-role-user-portal/vm-contributor-assignment.png)

## <a name="remove-access"></a>删除访问权限

在 RBAC 中，若要删除访问权限，请删除角色分配。

1. 在角色分配列表中，在具有“虚拟机参与者”角色的用户旁边添加复选标记。

1. 选择“删除”。

   ![“删除角色分配”消息](./media/quickstart-assign-role-user-portal/remove-role-assignment.png)

1. 在显示的“删除角色分配”消息中，选择“是”。

## <a name="clean-up"></a>清理

1. 在导航列表中，选择“资源组”。

1. 选择 **rbac-quickstart-resource-group**，打开资源组。

1. 选择“删除资源组”，删除资源组。

   ![删除资源组](./media/quickstart-assign-role-user-portal/delete-resource-group.png)

1. 在“是否确实要删除”边栏选项卡上，键入资源组名称：**rbac-quickstart-resource-group**。

1. 选择“删除”，删除资源组。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [教程：使用 RBAC 和 Azure PowerShell 授予用户对 Azure 资源的访问权限](tutorial-role-assignments-user-powershell.md)


<!-- Update_Description: wording update -->