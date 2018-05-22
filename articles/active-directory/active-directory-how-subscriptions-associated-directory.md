---
title: 如何将现有的 Azure 订阅添加到 Azure AD 目录 | Microsoft Docs
description: 如何将现有的订阅添加到 Azure AD 目录
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.assetid: bc4773c2-bc4a-4d21-9264-2267065f0aea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
origin.date: 12/12/2017
ms.date: 05/07/2018
ms.author: v-junlch
ms.reviewer: jeffsta
ms.custom: oldportal;it-pro;
ms.openlocfilehash: e4797e6058928e2cd96ee8e1b6574fe2ba00a852
ms.sourcegitcommit: 6f08b9a457d8e23cf3141b7b80423df6347b6a88
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2018
---
# <a name="how-to-associate-or-add-an-azure-subscription-to-azure-active-directory"></a>如何将 Azure 订阅关联或添加到 Azure Active Directory

本文介绍 Azure 订阅与 Azure Active Directory (Azure AD) 之间的关系，以及如何向 Azure AD 目录添加现有的订阅。 Azure 订阅与 Azure AD 建立了信任关系，也即会委托该目录对用户、服务和设备进行身份验证。 多个订阅可以委托同一个目录，但每个订阅只能委托一个目录。 

订阅与目录之间的这种委托关系不同于订阅与 Azure 中其他资源（网站、数据库等）之间的委托关系。 如果某个订阅过期，则与该订阅关联的其他资源的访问权限也会终止。 但是，Azure AD 目录会保留在 Azure 中，你可以将其他订阅与该目录相关联，然后使用新订阅管理目录。

所有用户都只有一个用于验证其身份的主目录，但他们也可以是其他目录中的来宾。 在 Azure AD 中，可以看到用户帐户是其成员或来宾的目录。

## <a name="next-steps"></a>后续步骤

* 若要详细了解如何创建免费的全新 Azure AD 目录，请参阅[如何获取 Azure Active Directory 租户](develop/active-directory-howto-tenant.md)
* 若要了解有关如何在 Azure 中控制资源访问的详细信息，请参阅[了解 Azure 中的资源访问权限](../role-based-access-control/rbac-and-directory-admin-roles.md)
* 有关如何在 Azure AD 中分配角色的详细信息，请参阅[在 Azure Active Directory 中分配管理员角色](active-directory-assign-admin-roles-azure-portal.md)



<!--Update_Description: link update -->
