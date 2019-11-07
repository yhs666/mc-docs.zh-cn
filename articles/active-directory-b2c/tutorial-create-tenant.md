---
title: 教程 - 创建 Azure Active Directory B2C 租户
description: 了解如何通过使用 Azure 门户创建 Azure Active Directory B2C 租户来准备注册应用程序。
services: B2C
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
origin.date: 09/28/2019
ms.date: 10/24/2019
ms.author: v-junlch
ms.subservice: B2C
ms.openlocfilehash: 05f9a95b81b1043bba9a35f4a02acd334dab5273
ms.sourcegitcommit: 817faf4e8d15ca212a2f802593d92c4952516ef4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2019
ms.locfileid: "72847136"
---
# <a name="tutorial-create-an-azure-active-directory-b2c-tenant"></a>教程：创建 Azure Active Directory B2C 租户

必须在你管理的租户中注册应用程序，然后这些应用程序才能与 Azure Active Directory B2C (Azure AD B2C) 交互。

在本文中，学习如何：

> [!div class="checklist"]
> * 创建 Azure AD B2C 租户
> * 将租户链接到订阅
> * 切换到包含你的 Azure AD B2C 租户的目录
> * 在 Azure 门户中将 Azure AD B2C 资源添加到收藏夹 

在下一个教程中，学习如何注册应用程序。

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。

## <a name="create-an-azure-ad-b2c-tenant"></a>创建 Azure AD B2C 租户

1. 登录到 [Azure 门户](https://portal.azure.cn/)。
1. 请确保使用的是包含订阅的目录。

    在顶部菜单中选择“目录 + 订阅”筛选器，然后选择包含你的订阅的目录。  此目录不同于将包含 Azure AD B2C 租户的其他目录。

    ![“目录 + 订阅”筛选器，其中已选择订阅租户](./media/tutorial-create-tenant/portal-01-select-directory.png)

1. 在 Azure 门户的左上角，选择“创建资源”  。
1. 搜索并选择“Active Directory B2C”  ，然后选择“创建”  。
1. 选择“创建新的 Azure AD B2C 租户”。 

    ![已在 Azure 门户中选择“创建新的 Azure AD B2C 租户”](./media/tutorial-create-tenant/portal-02-create-tenant.png)

1. 输入**组织名称**和**初始域名**。 选择**国家或地区**（以后不能更改），然后选择“创建”。 

    域名将用作完整租户域名的一部分。 在此示例中，租户名称是 *contosob2c.partner.onmschina.cn*：

    ![Azure 门户中包含示例值的“创建租户”窗体](./media/tutorial-create-tenant/portal-03-tenant-naming.png)

1. 租户创建过程完成后，请在租户创建页面的顶部选择“创建新的 B2C 租户或链接到现有租户”链接。 

    ![Azure 门户中突出显示的“链接租户”痕迹导航链接](./media/tutorial-create-tenant/portal-04-select-link-sub-link.png)

1. 选择“将现有 Azure AD B2C 租户链接到我的 Azure 订阅”。 

   ![Azure 门户中“链接现有订阅”的选项](./media/tutorial-create-tenant/portal-05-link-subscription.png)

1. 选择创建的 **Azure AD B2C 租户**，然后选择你的**订阅**。

    对于“资源组”  ，选择“新建”  。 输入要包含该租户的资源组的**名称**，选择**资源组位置**，然后选择“创建”。 

    ![Azure 门户中的“链接订阅”设置窗体](./media/tutorial-create-tenant/portal-06-link-subscription-settings.png)

## <a name="select-your-b2c-tenant-directory"></a>选择 B2C 租户目录

若要开始使用新的 Azure AD B2C 租户，需要切换到包含该租户的目录。

在 Azure 门户的顶部菜单中选择“目录 + 订阅”筛选器，然后选择包含你的 Azure AD B2C 租户的目录。 

如果最初在列表中未看到新的 Azure B2C 租户，请刷新浏览器窗口，然后再次在顶部菜单中选择“目录+订阅”筛选器  。

![已在 Azure 门户中选择包含 B2C 租户的目录](./media/tutorial-create-tenant/portal-07-select-tenant-directory.png)

## <a name="add-azure-ad-b2c-as-a-favorite-optional"></a>将 Azure AD B2C 添加到收藏夹（可选）

使用此可选步骤可以更轻松地在所有后续教程中选择 Azure AD B2C 租户。

如果你不希望每次使用租户时都要在“所有服务”中搜索“Azure AD B2C”，可将该资源加入收藏夹。  以后，可以从左侧的“收藏夹”菜单中选择该资源，以快速浏览到你的 Azure AD B2C 租户。 

只需执行此操作一次。 在执行这些步骤之前，请确保已根据上一部分[选择 B2C 租户目录](#select-your-b2c-tenant-directory)中所述切换到包含你的 Azure AD B2C 租户的目录。

1. 在 [Azure 门户](https://portal.azure.cn)的左侧菜单中选择“所有服务”。 
1. 在搜索文本框中输入 *Azure AD B2C*
1. 选择**星形**图标将 Azure AD B2C 添加到收藏夹
1. “Azure AD B2C”现在会显示在左侧的“收藏夹”菜单中。   如果需要，可以在列表中选择该资源并将其拖到更靠上的位置，如下图所示：

![在 Azure 门户中将 Azure AD B2C 添加到收藏夹的步骤](./media/tutorial-create-tenant/portal-08-favorite-b2c.png)

## <a name="next-steps"></a>后续步骤

本文介绍了如何执行以下操作：

> [!div class="checklist"]
> * 创建 Azure AD B2C 租户
> * 将租户链接到订阅
> * 切换到包含你的 Azure AD B2C 租户的目录
> * 在 Azure 门户中将 Azure AD B2C 资源添加到收藏夹 

接下来，了解如何在新租户中注册 Web 应用程序。

> [!div class="nextstepaction"]
> [注册应用程序 >](tutorial-register-applications.md)

<!-- Update_Description: wording update -->
