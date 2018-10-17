---
title: 如何使用 Azure Active Directory 删除组 | Microsoft Docs
description: 了解如何使用 Azure Active Directory 删除组。
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
origin.date: 08/29/2018
ms.date: 10/09/2018
ms.author: v-junlch
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: 20cb9adda4886c0a40fc4bfa8289fc663778213b
ms.sourcegitcommit: d8b4e1fbda8720bb92cc28631c314fa56fa374ed
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/10/2018
ms.locfileid: "48914092"
---
# <a name="how-to-delete-a-group-using-azure-active-directory"></a>如何：使用 Azure Active Directory 删除组
可以出于多种原因删除组，但通常是因为：

- 将“组类型”错误地设置为了错误选项

- 错误地创建了错误或重复的组 

- 不再需要该组

## <a name="to-delete-a-group"></a>删除组
1. 使用目录的全局管理员帐户登录到 [Azure 门户](https://portal.azure.cn)。

2. 依次选择“Azure Active Directory”、“组”。

3. 在“组 - 所有组”页中，搜索并选择要删除的组。 对于这些步骤，我们将使用“MDM 策略 - 东部”。

    ![“组 - 所有组”页，其中突出显示了组名](./media/active-directory-groups-delete-group/group-all-groups-screen.png)

4. 在“MDM 策略 - 东部概述”页上，选择“删除”。

    该组将从 Azure Active Directory 租户中删除。

    ![突出显示了删除选项的“MDM 策略 - 东部概述”页](./media/active-directory-groups-delete-group/group-overview-blade.png)

## <a name="next-steps"></a>后续步骤

- 如果误删除了某个组，可以重新创建该组。 有关详细信息，请参阅[如何创建基本组并添加成员](active-directory-groups-create-azure-portal.md)。

- 如果误删除了 Office 365 组，则可以将其还原。 有关详细信息，请参阅[还原已删除的 Office 365 组](../users-groups-roles/groups-restore-deleted.md)。

