---
title: 快速入门：访问 Azure Active Directory 并创建新租户 | Microsoft Docs
description: 快速入门：有关如何找到 Azure Active Directory 以及如何为组织创建新租户的步骤。
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.topic: quickstart
origin.date: 09/10/2018
ms.date: 12/10/2018
ms.author: v-junlch
custom: it-pro
ms.openlocfilehash: 980681698905da5751b8a8f44c5f33b800362556
ms.sourcegitcommit: 833865e1f1e99b3acd10781451eed636cc7cc810
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/10/2018
ms.locfileid: "53157407"
---
# <a name="quickstart-access-azure-active-directory-to-create-a-new-tenant"></a>快速入门：访问 Azure Active Directory 以创建新租户
可以使用 Azure Active Directory (Azure AD) 门户执行所有管理任务，包括为组织创建新的租户。 

在本快速入门中，你将学习如何到达 Azure 门户和 Azure Active Directory，以及如何为组织创建基本租户。

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial/)。

## <a name="sign-in-to-the-azure-portal"></a>登录到 Azure 门户
使用全局管理员帐户登录到组织的 [Azure 门户](https://portal.azure.cn/)。

![Azure 门户屏幕](./media/active-directory-access-create-new-tenant/azure-ad-portal.png)

## <a name="create-a-new-tenant-for-your-organization"></a>为组织创建新的租户
登录到 Azure 门户后，即可为组织创建新的租户。 新的租户代表你的组织，可帮助你管理面向内部和外部用户的特定 Azure 云服务实例。

### <a name="to-create-a-new-tenant"></a>创建新的租户
1. 依次选择“Azure Active Directory”、“创建资源”和“安全性 + 标识”，然后选择“Azure Active Directory”。

    “创建”页面随即显示。

    ![Azure Active Directory“创建”页面](./media/active-directory-access-create-new-tenant/azure-ad-create-new-tenant.png)

2.  在“创建目录”页面上，输入以下信息：
    
    - 在“组织名称”框中键入 _Contoso_。

    - 在“初始域名”框中键入 _Contoso_。

    - 保留“国家或地区”框中的“美国”选项。

3. 选择“创建” 。

将使用域 contoso.partner.onmschina.cn 创建新租户。

## <a name="clean-up-resources"></a>清理资源
如果不打算继续使用此应用程序，可按以下步骤删除此租户：

- 选择 Azure Active Directory，然后在“Contoso - 概述”页面上，选择“删除目录”。

    这会删除此租户及其关联信息。

    ![“创建目录”页面](./media/active-directory-access-create-new-tenant/azure-ad-delete-new-tenant.png)

## <a name="next-steps"></a>后续步骤
- 更改或添加其他域名，请参阅[如何向 Azure Active Directory 添加自定义域名](add-custom-domain.md)

- 添加用户，请参阅[添加新用户或删除用户](add-users-azure-active-directory.md)

- 添加组和成员，请参阅[创建基本组并添加成员](active-directory-groups-create-azure-portal.md)

- 了解 Azure AD，包括[基本许可信息、术语和关联的功能](active-directory-whatis.md)。

<!-- Update_Description: wording update -->