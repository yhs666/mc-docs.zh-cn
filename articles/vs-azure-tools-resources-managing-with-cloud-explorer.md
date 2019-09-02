---
title: 使用 Cloud Explorer 管理 Azure 资源 | Microsoft Docs
description: 了解如何使用云资源管理器来浏览和管理 Visual Studio 中的 Azure 资源。
services: visual-studio-online
author: ghogen
manager: douge
assetId: 6347dc53-f497-49d5-b29b-e8b9f0e939d7
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
origin.date: 03/25/2017
ms.date: 09/26/2018
ms.author: v-junlch
ms.openlocfilehash: c083020953534985b9120bf51cf452881d725872
ms.sourcegitcommit: b8fb6890caed87831b28c82738d6cecfe50674fd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/29/2019
ms.locfileid: "58625203"
---
# <a name="manage-the-resources-associated-with-your-azure-accounts-in-visual-studio-cloud-explorer"></a>在 Visual Studio Cloud Explorer 中管理与 Azure 帐户关联的资源
通过 Cloud Explorer，可在 Visual Studio 中查看 Azure 资源和资源组、检查其属性，以及执行重要的开发人员诊断操作。 

与 [Azure 门户](https://portal.azure.cn)一样，Cloud Explorer 基于 Azure Resource Manager 堆栈。 因此，Cloud Explorer 可以识别 Azure 资源组等资源，以及 API 应用等 Azure 服务，并支持[基于角色的访问控制](role-based-access-control/role-assignments-portal.md) (RBAC)。 

## <a name="prerequisites"></a>先决条件
- 已选择 **Azure 工作负荷**的 [Visual Studio 2017](https://www.visualstudio.com/downloads/)，或者包含[用于 .NET 2.9 的 Azure SDK](https://www.microsoft.com/en-us/download/details.aspx?id=51657) 的 Visual Studio 早期版本。
- Azure 帐户 - 如果没有帐户，可以[注册试用帐户](https://www.azure.cn/pricing/1rmb-trial)。

> [!NOTE]
> 若要查看 Cloud Explorer，请选择菜单栏上的“视图” > “Cloud Explorer”。   
> 
> 

## <a name="add-an-azure-account-to-cloud-explorer"></a>将 Azure 帐户添加到 Cloud Explorer
要查看与 Azure 帐户关联的资源，必须先将帐户添加到 Cloud Explorer。 

1. 在 **Cloud Explorer** 中，选择“Azure 帐户设置”。

    ![Cloud Explorer Azure 帐户设置图标](./media/vs-azure-tools-resources-managing-with-cloud-explorer/azure-account-settings.png)

1. 选择“管理帐户”。 

    ![Cloud Explorer 添加帐户链接](./media/vs-azure-tools-resources-managing-with-cloud-explorer/manage-accounts-link.png)

1. 登录到要浏览其资源的 Azure 帐户。 

1. 登录到 Azure 帐户以后，会显示与该帐户关联的订阅。 选中要浏览的帐户订阅的复选框，并选择“应用”。 
 
    ![Cloud Explorer：选择要显示的 Azure 订阅](./media/vs-azure-tools-resources-managing-with-cloud-explorer/select-subscriptions.png)

1. 选择要浏览其资源的订阅后，这些订阅和资源会显示在 Cloud Explorer 中。

    ![Azure 帐户的 Cloud Explorer 资源列表](./media/vs-azure-tools-resources-managing-with-cloud-explorer/resources-listed.png)

## <a name="remove-an-azure-account-from-cloud-explorer"></a>从 Cloud Explorer 删除 Azure 帐户 

1. 在 Cloud Explorer 中选择“帐户管理”。

    ![Cloud Explorer Azure 帐户设置图标](./media/vs-azure-tools-resources-managing-with-cloud-explorer/azure-account-settings.png)

1. 在要删除的帐户旁，选择“管理帐户”。

    ![Cloud Explorer Azure 帐户设置图标](./media/vs-azure-tools-resources-managing-with-cloud-explorer/remove-account.png)

1. 选择“删除”以删除该帐户。

    ![Cloud Explorer“管理帐户”对话框](./media/vs-azure-tools-resources-managing-with-cloud-explorer/accountmanage.PNG)

## <a name="view-resource-types-or-resource-groups"></a>查看资源类型或资源组
若要查看 Azure 资源，可以选择“资源类型”或“资源组”视图。

1. 在 **Cloud Explorer** 中，选择资源视图下拉列表。

    ![可选择所需资源视图的 Cloud Explorer 下拉列表](./media/vs-azure-tools-resources-managing-with-cloud-explorer/resources-view-dropdown.png)

2. 从上下文菜单中，选择所需视图： 

   - “资源类型”视图 - [Azure 门户](https://portal.azure.cn)中使用的常用视图，按类型来显示 Azure 资源，例如 Web 应用、存储帐户和虚拟机。 
   - “资源组”视图 - 按关联的 Azure 资源组将 Azure 资源分类。 资源组是通常由特定应用程序使用的 Azure 资源组合。 若要了解有关 Azure 资源组的详细信息，请参阅 [Azure Resource Manager 概述](./azure-resource-manager/resource-group-overview.md)。

     下图比较了两个资源视图：

     ![Cloud Explorer 资源视图比较](./media/vs-azure-tools-resources-managing-with-cloud-explorer/resource-views-comparison.png)

## <a name="view-and-navigate-resources-in-cloud-explorer"></a>在 Cloud Explorer 中查看和导航资源
如果要在 Cloud Explorer 中导航到 Azure 资源并查看其信息，请展开项的类型或关联的资源组，并选择该资源。 当选择资源时，信息会显示在 Cloud Explorer 底部的两个选项卡（“操作”和“属性”）中。 

- “操作”选项卡 - 列出了用户可以在 Cloud Explorer 中针对所选资源执行的操作。 还可以通过右键单击资源来查看其上下文菜单，从而查看这些选项。

- “属性”选项卡 - 显示资源的属性，例如其类型、区域设置和关联的资源组。

下图显示了关于在应用服务的每个选项卡上看到的内容的示例比较：

![](./media/vs-azure-tools-resources-managing-with-cloud-explorer/actions-and-properties.png)

每个资源都有“在门户中打开”操作。 选择此操作时，Cloud Explorer 会在 [Azure 门户](https://portal.azure.cn)中显示所选的资源。 “在门户中打开”功能适用于导航到深度嵌套的资源。

根据 Azure 资源，还可能会显示其他操作和属性值。 例如，除了“在门户中打开”操作以外，Web 应用还提供了“在浏览器中打开”和“附加调试器”操作。 当选择存储帐户 Blob、队列或表时，会显示用于打开编辑器的操作。 Azure 应用具有“URL”和“状态”属性，而存储资源具有键和连接字符串属性。

## <a name="find-resources-in-cloud-explorer"></a>在 Cloud Explorer 中查找资源
若要在 Azure 帐户订阅中查找具有特定名称的资源，请在 Cloud Explorer 的“搜索”框中输入该名称。

![在云资源管理器中查找资源](./media/vs-azure-tools-resources-managing-with-cloud-explorer/search-for-resources.png)

在“搜索”框中输入字符时，只有符合这些字符的资源才会显示在资源树中。

<!-- Update_Description: wording update -->
