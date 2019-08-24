---
title: 创建动态组并检查状态 - Azure Active Directory | Microsoft Docs
description: 如何在 Azure 门户中创建组成员资格规则并检查状态。
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
origin.date: 03/18/2019
ms.date: 08/12/2019
ms.author: v-junlch
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: b2c836d446a9994ddfd10fdb3588353273fa49ea
ms.sourcegitcommit: 44548f2ebec1246f6ac799f5b2640ad1b5d7c8a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/13/2019
ms.locfileid: "68973123"
---
# <a name="create-a-dynamic-group-and-check-status"></a>创建动态组并检查状态

在 Azure Active Directory (Azure AD) 中，可以使用规则根据用户或设备属性确定组成员资格。 本文介绍如何为 Azure 门户中的动态组设置一项规则。
支持为安全组或 Office 365 组启用动态成员身份。 应用组成员身份规则时，将会对用户和设备属性进行评估，确定其是否与成员身份规则匹配。 当用户或设备的任何属性发生更改时，将处理组织中的所有动态组规则以进行成员身份更改。 如果用户和设备符合组的条件，则会对其执行添加或删除操作。

如需成员身份规则的语法、支持的属性、运算符和值的示例，请参阅 [Azure Active Directory 中的组的动态成员资格规则](groups-dynamic-membership.md)。

## <a name="to-create-a-group-membership-rule"></a>要创建组成员资格规则，请执行以下操作：

1. 在租户中使用全局管理员、Intune 管理员或用户管理员角色中的帐户登录 [Azure 门户](https://portal.azure.cn)。
2. 选择“组”。 
3. 选择“所有组”  ，然后选择“新组”  。

   ![选择用于添加新组的命令](./media/groups-create-rule/new-group-creation.png)

4. 在“组”页面上，输入新组的名称和说明。  为用户或设备选择“成员身份类型”，然后选择“添加动态查询”。   可使用规则生成器生成一个简单的规则，或[自己编写一个成员身份规则](groups-dynamic-membership.md)。

   ![为动态组添加成员身份规则](./media/groups-create-rule/add-dynamic-group-rule.png)

5. 查看适用于成员身份查询的自定义扩展属性
   1. 选择“获取自定义扩展属性” 
   2. 输入应用程序 ID，然后选择“刷新属性”  。 
6. 创建规则之后，选择边栏选项卡底部的“添加查询”  。
7. 在“组”边栏选项卡中，选择“创建”以创建该组。  

如果输入的规则无效，则会在门户右上角显示一个说明，指出为何系统无法处理规则。 请仔细阅读，了解如何修复规则。

## <a name="turn-on-or-off-welcome-email"></a>打开或关闭欢迎电子邮件

创建新的 Office 365 组时，会向添加到该组的用户发送欢迎通知。 以后，如果用户或设备的任何属性发生更改时，将处理组织中的所有动态组规则以进行成员身份更改。 添加的用户也会收到欢迎通知。 可以在 [Exchange PowerShell](https://docs.microsoft.com/powershell/module/exchange/users-and-groups/Set-UnifiedGroup?view=exchange-ps) 中关闭此行为。 

## <a name="check-processing-status-for-a-rule"></a>检查规则的处理状态

可在组的“概述”页上查看成员资格处理状态和上次更新日期  。
  
  ![显示动态组状态](./media/groups-create-rule/group-status.png)

“成员资格处理”状态会显示以下几种状态消息  ：

* **正在评估**：已收到组更改，正在评估更新。
* **正在处理**：正在处理更新。
* **更新完成**：处理已完成，且已完成所有适用更新。
* **处理错误**：无法完成处理是因为评估成员身份规则时遇到错误。
* **更新已暂停**：管理员暂停了动态成员资格规则更新。 MembershipRuleProcessingState 设置为“已暂停”。

“上次更新的成员资格”状态会显示以下几种状态消息  ：

* &lt;**日期和时间**&gt;：上次更新成员资格的时间。
* **正在进行**：目前正在进行更新。
* **未知**：无法检索上次更新时间。 该组可能是新的。

如果在处理特定组的成员资格规则时出错误，则该组的“概述”页顶部会显示警报  。 如果无法在 24 小时之后处理租户中所有组的挂起动态成员资格更新，则会在所有组的顶部显示警报  。

![正在处理错误消息警报](./media/groups-create-rule/processing-error.png)

以下文章提供了有关 Azure Active Directory 中的组的更多信息。

* [查看现有组](../fundamentals/active-directory-groups-view-azure-portal.md)
* [创建新组并添加成员](../fundamentals/active-directory-groups-create-azure-portal.md)
* [管理组的设置](../fundamentals/active-directory-groups-settings-azure-portal.md)
* [管理组的成员身份](../fundamentals/active-directory-groups-membership-azure-portal.md)
* [管理组中用户的动态规则](groups-dynamic-membership.md)

