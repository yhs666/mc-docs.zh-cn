---
title: 如何将用户和组分配到应用程序 | Microsoft Docs
description: 将用户分配到应用程序以授予访问权限
services: active-directory
documentationcenter: ''
author: yunan2016
manager: digimobile
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 07/11/2017
ms.date: 3/5/2018
ms.author: v-nany
ms.openlocfilehash: ea46ae3a95eb491a4b329126d790088ef4e020f1
ms.sourcegitcommit: ba39acbdf4f7c9829d1b0595f4f7abbedaa7de7d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2018
---
# <a name="how-to-assign-users-and-groups-to-an-application"></a>如何将用户和组分配到应用程序

在用户可以对特定应用程序执行下列任何操作之前，需要先**将用户分配给应用程序**以授予其访问权限：

-   通过**直接导航到应用程序的 URL**（也称为 SP 发起的登录）访问应用程序。

-   通过使用应用程序的“属性”页上的“用户访问 URL”（也称为 IDP 发起的登录）访问应用程序。


-   查看显示在其 [Office 365 应用程序启动器](https://support.office.com/article/Meet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a)中的应用程序。

## <a name="methods-to-assign-applications-with-azure-active-directory"></a>使用 Azure Active Directory 分配应用程序的方法 

在 Azure 中国，可以使用 Azure Active Directory 分配应用程序：

-   [以管理员身份直接将用户分配到应用程序](#assign-a-user-directly-as-an-administrator)



## <a name="assign-a-user-directly-as-an-administrator"></a>以管理员身份直接将用户分配到应用程序

要直接将一个或多个用户分配到应用程序，请按照以下步骤操作：

1.  打开 [**Azure 门户**](https://portal.azure.cn/)，并以“全局管理员”身份登录。

2.  在左侧主导航菜单顶部单击“所有服务”，打开“Azure Active Directory 扩展”。

3.  在筛选器搜索框中键入“Azure Active Directory”，选择“Azure Active Directory”项。

4.  在 Azure Active Directory 的左侧导航菜单中，单击“企业应用程序”。

5.  单击“所有应用程序”，查看所有应用程序的列表。

  * 如果未看到要在此处显示的应用程序，请使用“所有应用程序列表”顶部的“筛选器”控件，并将“显示”选项设置为“所有应用程序”。

6.  从列表中选择要向其分配用户的应用程序。

7.  在应用程序加载后，在应用程序的左侧导航菜单中单击“用户和组”。

8.  单击“用户和组”列表顶部的“添加”按钮，以打开“添加分配”窗格。

9.  在“添加分配”窗格中，单击“用户和组”选择器。

10. 在“按名称或电子邮件地址搜索”搜索框中，键入要分配的用户的**全名**或**电子邮件地址**。

11. 将鼠标悬停在列表中的“用户”上方以显示“复选框”。 单击用户个人资料头像或徽标旁边的复选框，将用户添加到“已选择”列表。

12. **可选：**如果想要**添加多个用户**，请在“按名称或电子邮件地址搜索”搜索框中，键入其他**全名**或**电子邮件地址**，然后单击复选框以将此用户添加到“已选择”列表。

13. 在完成用户的选择后，单击“选择”按钮将他们添加到要分配给应用程序的用户和组列表。

14. **可选：**单击“添加分配”窗格中的“选择角色”选择器，选择一个角色来分配给所选用户。

15. 单击“分配”按钮，将应用程序分配给选定用户。

在一段很短的时间后，所选用户能够使用解决方案描述部分中所述的方法启动这些应用程序。



