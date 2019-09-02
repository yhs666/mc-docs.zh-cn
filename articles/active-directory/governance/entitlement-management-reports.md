---
title: 在 Azure AD 权利管理（预览版）中查看报表和日志 - Azure Active Directory
description: 了解如何在 Azure Active Directory 权利管理（预览版）中查看用户分配报表和审核日志。
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: jocastel-MSFT
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
origin.date: 04/19/2019
ms.date: 08/22/2019
ms.author: v-junlch
ms.reviewer: jocastel
ms.collection: M365-identity-device-management
ms.openlocfilehash: c12a81b6e6c3ac083185a002e51caaca77bd9c2d
ms.sourcegitcommit: 599d651afb83026938d1cfe828e9679a9a0fb69f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/23/2019
ms.locfileid: "69993353"
---
# <a name="view-reports-and-logs-in-azure-ad-entitlement-management-preview"></a>在 Azure AD 权利管理（预览版）中查看报表和日志

> [!IMPORTANT]
> Azure Active Directory (Azure AD) 权利管理目前以公共预览版提供。
> 此预览版在提供时没有附带服务级别协议，不建议将其用于生产工作负荷。 某些功能可能不受支持或者受限。
> 有关详细信息，请参阅[适用于 Azure 预览版的补充使用条款](https://www.azure.cn/support/legal/)。

用户分配报告和 Azure Active Directory 审核日志提供了有关目录中用户的其他详细信息。 作为管理员，你可以查看用户有权访问的资源，并查看请求日志，以便进行审核或确定用户请求的状态。 本文介绍如何使用用户分配报告和 Azure AD 审核日志。

## <a name="view-resources-a-user-has-access-to"></a>查看用户可以访问的资源

1. 依次单击“Azure Active Directory”、“标识监管”。  

1. 在左侧菜单中单击“用户分配报表”。 

1. 单击“选择用户”，打开“选择用户”窗格  。

1. 在列表中查找特定用户，你需要查看该用户有权访问的资源。

1. 单击该用户，然后单击“选择”  。

    此时会显示一个列表，其中包含该用户可以访问的资源。 该列表包含访问包、策略和日期。

    ![用户分配报表](./media/entitlement-management-reports/user-assignments-report.png)

## <a name="determine-the-status-of-a-users-request"></a>确定用户的请求的状态

若要更详细地了解用户如何请求访问某个访问包，以及如何接受对该包的访问，可以使用 Azure AD 审核日志。 具体说来，可以使用`EntitlementManagement` 和 `UserManagement` 类别中的日志记录来更详细地了解每个请求的处理步骤。  

1. 单击“Azure Active Directory”，然后单击“审核日志”。  

1. 在顶部将“类别”更改为  `EntitlementManagement` 或 `UserManagement`，具体取决于要查找的审核记录。  

1. 单击“应用”  。

1. 若要下载日志，请单击“下载”。 

Azure AD 在收到新请求时会写入一条审核记录，其中的“类别”为  `EntitlementManagement`，“活动”通常为  `User requests access package assignment`。  如果是在 Azure 门户中创建的直接分配，则审核记录的“活动”字段  为 `Administrator directly assigns user to access package`，执行分配的用户通过 **ActorUserPrincipalName** 来标识。

Azure AD 会在请求正在进行时写入更多的审核记录，包括：

| Category | 活动 | 请求状态 |
| :---- | :------------ | :------------ |
| `EntitlementManagement` | `Auto approve access package assignment request` | 请求不需要审批 |
| `UserManagement` | `Create request approval` | 请求需要审批 |
| `UserManagement` | `Add approver to request approval` | 请求需要审批 |
| `EntitlementManagement` | `Approve access package assignment request` | 已批准请求 |
| `EntitlementManagement` | `Ready to fulfill access package assignment request` |请求已获得批准，或者不需要审批 |

为用户分配访问权限以后，Azure AD 会写入一条审核记录，其类别为 `EntitlementManagement`，其“活动”为  `Fulfill access package assignment`。  收到访问权限的用户按 **ActorUserPrincipalName** 字段进行标识。

如果未分配访问权限，则 Azure AD 会写入一条审核记录，其类别为 `EntitlementManagement`，其“活动”为  `Deny access package assignment request`（如果请求被审批者拒绝）或 `Access package assignment request timed out (no approver action taken)`（如果请求已在审批者进行审批之前超时）。

如果用户的访问包分配过期、被用户取消，或者被管理员删除，则 Azure AD 会写入一条审核记录，其类别为 `EntitlementManagement`，其“活动”为  `Remove access package assignment`。

## <a name="next-steps"></a>后续步骤

- [排查 Azure AD 权利管理的问题](entitlement-management-troubleshoot.md)
- [常见方案](entitlement-management-scenarios.md)

<!-- Update_Description: wording update -->