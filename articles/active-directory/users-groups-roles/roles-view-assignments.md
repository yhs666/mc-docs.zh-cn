---
title: 在管理中心查看管理员角色权限 - Azure Active Directory | Microsoft Docs
description: 现在，你可以在 Azure AD 管理中心查看和管理 Azure AD 管理员角色的成员。
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
origin.date: 07/31/2019
ms.date: 08/20/2019
ms.author: v-junlch
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: d795f182e15bb9f7a5fdd879f02534fa77303096
ms.sourcegitcommit: 599d651afb83026938d1cfe828e9679a9a0fb69f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/23/2019
ms.locfileid: "69993670"
---
# <a name="view-custom-role-assignments-in-azure-active-directory"></a>在 Azure Active Directory 中查看自定义角色分配

本文介绍如何在 Azure Active Directory (Azure AD) 中查看已分配的自定义角色。 在 Azure Active Directory (Azure AD) 中，角色可以在目录级别分配，也可以在单个应用程序的范围内分配。 在目录范围的角色分配添加到单个应用程序角色分配的列表中，但在单个应用程序范围的角色分配不添加到目录级别分配的列表中。

## <a name="view-the-assignments-of-a-role-with-directory-scope-using-the-azure-portal"></a>使用 Azure 门户查看目录范围角色的分配

1. 在 Azure AD 组织中使用特权角色管理员或全局管理员权限登录 [Azure 门户](https://portal.azure.cn)。
1. 选择“Azure Active Directory”，接着选择“角色和管理员”，然后选择一个角色，将其打开并查看其属性。 ****  ****
1. 选择“分配”，查看角色的分配  。

    ![从列表中打开一个角色时，查看角色分配和权限](./media/roles-view-assignments/role-assignments.png)

## <a name="view-the-assignments-of-a-role-with-directory-scope-using-azure-ad-powershell"></a>使用 Azure AD PowerShell 查看目录范围的角色的分配

可以自动使用 Azure PowerShell 将 Azure AD 管理员角色分配给用户。 本文使用 [Azure Active Directory PowerShell 版本 2](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0#directory_roles) 模块。

### <a name="prepare-powershell"></a>准备 PowerShell

首先，必须[下载 Azure AD 预览版 PowerShell 模块](https://www.powershellgallery.com/packages/AzureAD/)。

若要安装 Azure AD PowerShell 模块，请使用以下命令：

``` PowerShell
install-module azureadpreview
import-module azureadpreview
```

若要验证模块是否可供使用，请运行下面的命令：

``` PowerShell
get-module azuread
  ModuleType Version      Name                         ExportedCommands
  ---------- ---------    ----                         ----------------
  Binary     2.0.0.115    azuread                      {Add-AzureADAdministrati...}
```

### <a name="view-the-assignments-of-a-role"></a>查看角色的分配

示例：查看角色分配。

``` PowerShell
# Fetch list of all directory roles with object ID
Get-AzureADDirectoryRole

# Fetch a specific directory role by ID
$role = Get-AzureADDirectoryRole -ObjectId "5b3fe201-fa8b-4144-b6f1-875829ff7543"

# Fetch role membership for a role
Get-AzureADDirectoryRoleMember -ObjectId $role.ObjectId | Get-AzureADUser
```

## <a name="view-the-assignments-of-a-role-with-directory-scope-using-microsoft-graph-api"></a>使用 Microsoft Graph API 查看目录范围的角色的分配

HTTP 请求，用于获取给定角色定义的角色分配。

GET

``` HTTP
https://graph.chinacloudapi.cn/<tenantDomain-or-tenantId>/roleAssignments?api-version=1.61-internal&$filter=roleDefinitionId eq ‘<object-id-or-template-id-of-role-definition>’
```

响应

``` HTTP
HTTP/1.1 200 OK
{
    "id":"CtRxNqwabEKgwaOCHr2CGJIiSDKQoTVJrLE9etXyrY0-1"
    "principalId":"ab2e1023-bddc-4038-9ac1-ad4843e7e539",
    "roleDefinitionId":"3671d40a-1aac-426c-a0c1-a3821ebd8218",
    "resourceScopes":["/"]
}
```

## <a name="view-the-assignments-of-a-role-with-single-application-scope-using-the-azure-portal-preview"></a>使用 Azure 门户查看单个应用程序范围角色的分配（预览）

1. 在 Azure AD 组织中使用特权角色管理员或全局管理员权限登录 [Azure 门户](https://portal.azure.cn)。
1. 选择“Azure Active Directory”，接着选择“应用注册”，然后选择要查看其属性的应用注册。  可能必须选择“所有应用程序”，以便在 Azure AD 组织中查看应用注册的完整列表。 

    ![在“应用注册”页中创建或编辑应用注册](./media/roles-create-custom/appreg-all-apps.png)

1. 选择“角色和管理员”，然后选择一个角色，查看其属性。 ****

    ![在“应用注册”页中查看应用注册角色分配](./media/roles-view-assignments/appreg-assignments.png)

1. 选择“分配”，查看角色的分配  。

    ![在应用注册的属性中查看应用注册角色分配](./media/roles-view-assignments/appreg-assignments-2.png)

## <a name="next-steps"></a>后续步骤

* 欢迎在 [Azure AD 管理角色论坛](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032)上与我们分享知识。
* 有关角色以及管理员角色分配的详细信息，请参阅[分配管理员角色](directory-assign-admin-roles.md)。
* 有关默认用户权限，请参阅[默认来宾和成员用户权限的比较](../fundamentals/users-default-permissions.md)。

