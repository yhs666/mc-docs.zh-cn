---
title: 有关访问和创建新租户的快速入门 - Azure Active Directory | Microsoft Docs
description: 介绍如何查找 Azure Active Directory，以及如何为组织创建新的租户。
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: quickstart
origin.date: 09/10/2018
ms.date: 11/28/2019
ms.author: v-junlch
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: ee24af4d2a3abeed6c0d7b2866d76bec598a5885
ms.sourcegitcommit: 9597d4da8af58009f9cef148a027ccb7b32ed8cf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/28/2019
ms.locfileid: "74655393"
---
# <a name="quickstart-create-a-new-tenant-in-azure-active-directory"></a>快速入门：在 Azure Active Directory 中创建一个新租户
可以使用 Azure Active Directory (Azure AD) 门户执行所有管理任务，包括为组织创建新的租户。 

在本快速入门中，你将学习如何到达 Azure 门户和 Azure Active Directory，以及如何为组织创建基本租户。

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial/)。

## <a name="create-a-new-tenant-for-your-organization"></a>为组织创建新的租户
登录到 Azure 门户后，即可为组织创建新的租户。 新的租户代表你的组织，可帮助你管理面向内部和外部用户的特定 Azure 云服务实例。

### <a name="to-create-a-new-tenant"></a>创建新的租户

1. 使用全局管理员帐户登录到组织的 [Azure 门户](https://portal.azure.cn/)。

1. 在 Azure 门户菜单中，选择“创建资源”  。  

    ![Azure Active Directory 的“创建资源”页](./media/active-directory-access-create-new-tenant/azure-ad-portal.png)

1. 选择“标识”，然后选择“Azure Active Directory”   。

    随即出现“创建目录”页面  。

    ![Azure Active Directory“创建”页面](./media/active-directory-access-create-new-tenant/azure-ad-create-new-tenant.png)

1.  在“创建目录”页面上，输入以下信息  ：
    
    - 在“组织名称”框中键入 _Contoso_。 

    - 在“初始域名”框中键入 _Contoso_。 

    - 保留“国家或地区”框中的“美国”选项   。

1. 选择“创建”  。

将使用域 contoso.partner.onmschina.cn 创建新租户。

## <a name="clean-up-resources"></a>清理资源
如果不打算继续使用此应用程序，可按以下步骤删除此租户：

- 选择 Azure Active Directory，然后在“Contoso - 概述”页面上，选择“删除目录”    。

    这会删除此租户及其关联的信息。

    ![突出显示“删除目录”按钮的概述页](./media/active-directory-access-create-new-tenant/azure-ad-delete-new-tenant.png)

## <a name="next-steps"></a>后续步骤
- 更改或添加其他域名，请参阅[如何向 Azure Active Directory 添加自定义域名](add-custom-domain.md)

- 添加用户，请参阅[添加新用户或删除用户](add-users-azure-active-directory.md)

- 添加组和成员，请参阅[创建基本组并添加成员](active-directory-groups-create-azure-portal.md)

- 了解[使用 Privileged Identity Management 基于角色进行访问](../../role-based-access-control/pim-azure-resource.md)，以帮助管理组织的应用程序和资源访问权限。

- 了解 Azure AD，包括[基本许可信息、术语和关联的功能](active-directory-whatis.md)。

