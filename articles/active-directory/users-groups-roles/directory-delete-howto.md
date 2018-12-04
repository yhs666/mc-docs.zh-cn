---
title: 删除 Azure Active Directory 租户目录 | Microsoft Docs
description: 介绍如何准备要删除的 Azure AD 租户目录
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
origin.date: 06/13/2018
ms.date: 07/31/2018
ms.author: v-junlch
ms.reviewer: elkuzmen
ms.custom: it-pro
ms.openlocfilehash: 2610957462d4344ecc11f0a0af4d345a917b17d3
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52647136"
---
# <a name="delete-an-azure-active-directory-tenant"></a>删除 Azure Active Directory 租户
删除租户时，也会删除包含在该租户中的所有资源。 必须对租户进行准备，尽量减少关联的资源，然后才能将其删除。 只有 Azure Active Directory (Azure AD) 全局管理员可以从门户中删除 Azure AD 租户。

## <a name="prepare-the-tenant-for-deletion"></a>准备要删除的租户

在删除 Azure AD 中的某个租户之前，必须对其进行多项检查。 这些检查可以降低删除租户后对用户访问造成负面影响（例如，影响用户登录 Office 365 或访问 Azure 中的资源）的风险。 例如，如果与某个订阅关联的租户被无意删除，则用户无法访问该订阅的 Azure 资源。 下面介绍了需要检查哪些情况：

- 租户中不能有用户，只能有一个负责删除该租户的全局管理员。 只有在删除所有其他用户后，才能删除该租户。 如果用户是从本地同步的，则必须关闭同步，并且必须使用 Azure 门户或 Azure PowerShell cmdlet 从云租户中删除这些用户。 
- 租户中不能有任何应用程序。 只有在删除所有应用程序后，才能删除租户。
- 不能有任何多重身份验证提供程序关联到该租户。
- 与租户关联的任何 Microsoft Online Services（例如 Azure 或 Office 365）不能存在任何订阅。 例如，如果在 Azure 中创建了一个默认租户，并且 Azure 订阅仍然依赖于此租户进行身份验证，则不能删除此租户。 类似地，如果其他用户已将订阅与某个租户相关联，则无法删除该租户。 

## <a name="delete-an-azure-ad-tenant"></a>删除 Azure AD 租户

1. 使用租户全局管理员帐户登录到 [Azure 门户](https://portal.azure.cn)。

2. 选择“Azure Active Directory” 。

3. 切换到要删除的租户。
  
    ![“删除目录”按钮](./media/directory-delete-howto/delete-directory-command.png)

4. 选择“删除目录”。
  
    ![“删除目录”按钮](./media/directory-delete-howto/delete-directory-list.png)

5. 如果租户没有通过一项或多项检查，则会出现一个详细说明如何通过的链接。 通过所有检查后，请选择“删除”，此过程结束。

## <a name="next-steps"></a>后续步骤
[Azure Active Directory 文档](/active-directory/)

