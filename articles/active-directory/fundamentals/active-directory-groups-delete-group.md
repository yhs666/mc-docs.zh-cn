---
title: 删除组 - Azure Active Directory | Microsoft Docs
description: 关于如何使用 Azure Active Directory 删除组的说明。
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
origin.date: 08/29/2018
ms.date: 01/21/2019
ms.author: v-junlch
ms.reviewer: krbain
ms.custom: it-pro, seodec18
ms.openlocfilehash: 2630262d0bee0d4fffe64431b319c388e57c5a40
ms.sourcegitcommit: 29a95e5d4667c5c1ea82477c0449a722aae90d96
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2019
ms.locfileid: "54440328"
---
# <a name="delete-a-group-using-azure-active-directory"></a>使用 Azure Active Directory 删除组
可以因为各种原因删除 Azure Active Directory (Azure AD) 组，但通常是因为：

- 错误地将“组类型”设为了错误的选项。

- 错误地创建错误或重复组。 

- 不再需要组。

## <a name="to-delete-a-group"></a>删除组
1. 使用目录的全局管理员帐户登录到 [Azure 门户](https://portal.azure.cn)。

2. 选择“Azure Active Directory”，然后选择“组”。

3. 在“组 - 所有组”页中，搜索并选择要删除的组。 对于这些步骤，我们将使用“MDM 策略 - 东部”。

    ![“组 - 所有组”页，其中突出显示了组名](./media/active-directory-groups-delete-group/group-all-groups-screen.png)

4. 在“MDM 策略 - 东部概述”页上，选择“删除”。

    该组将从 Azure Active Directory 租户中删除。

    ![突出显示了删除选项的“MDM 策略 - 东部概述”页](./media/active-directory-groups-delete-group/group-overview-blade.png)

## <a name="next-steps"></a>后续步骤

- 如果误删除了某个组，可以重新创建该组。 有关详细信息，请参阅[如何创建基本组并添加成员](active-directory-groups-create-azure-portal.md)。

- 如果误删除了 Office 365 组，则可以将其还原。 有关详细信息，请参阅[还原已删除的 Office 365 组](../users-groups-roles/groups-restore-deleted.md)。

<!-- Update_Description: wording update -->