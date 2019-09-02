---
title: Azure AD 权利管理（预览版）中的常见方案 - Azure Active Directory
description: 了解在 Azure Active Directory 权利管理（预览版）中实施常见方案时应该执行的概略性步骤。
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
origin.date: 04/23/2019
ms.date: 08/09/2019
ms.author: v-junlch
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: bf0dbdfb308948941533f83505818b06a0714d0c
ms.sourcegitcommit: 44548f2ebec1246f6ac799f5b2640ad1b5d7c8a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/13/2019
ms.locfileid: "68972821"
---
# <a name="common-scenarios-in-azure-ad-entitlement-management-preview"></a>Azure AD 权利管理（预览版）中的常见方案

> [!IMPORTANT]
> Azure Active Directory (Azure AD) 权利管理目前以公共预览版提供。
> 此预览版在提供时没有附带服务级别协议，不建议将其用于生产工作负荷。 某些功能可能不受支持或者受限。
> 有关详细信息，请参阅[适用于 Azure 预览版的补充使用条款](https://www.azure.cn/support/legal/)。

可以通过多种方式配置组织的权利管理。 但是，如果只是初学，则需了解管理员、审批者和请求者面对的常见方案。

## <a name="administrators"></a>管理员

### <a name="im-new-to-entitlement-management-and-i-want-help-with-getting-started"></a>我不了解权利管理，需要入门方面的帮助

> [!div class="mx-tableFixed"]
> | 步骤 | 示例 |
> | --- | --- |
> | [按教程创建第一个访问包](entitlement-management-access-package-first.md) | [![Azure 门户图标](./media/entitlement-management-scenarios/azure-portal.png)](./media/entitlement-management-scenarios/azure-portal-expanded.png#lightbox) |

### <a name="i-want-to-allow-users-in-my-directory-to-request-access-to-groups-applications-or-sharepoint-sites"></a>我想要允许我目录中的用户请求对组、应用程序或 SharePoint 站点的访问权限

> [!div class="mx-tableFixed"]
> | 步骤 | 示例 |
> | --- | --- |
> | **1.** [在目录中创建新的访问包](entitlement-management-access-package-create.md#start-new-access-package) | ![创建访问包](./media/entitlement-management-scenarios/access-package.png) |
> | **2.** [向访问包添加资源角色](entitlement-management-access-package-edit.md#add-resource-roles)<ul><li>组</li><li>应用程序</li><li>SharePoint 站点</li></ul> | ![添加资源角色](./media/entitlement-management-scenarios/resource-roles.png) |
> | **3.** [添加策略](entitlement-management-access-package-edit.md#policy-for-users-in-your-directory)<ul><li>适用于目录中的用户</li><li>需要审批</li><li>过期设置</li></ul> | ![添加策略](./media/entitlement-management-scenarios/policy.png) |

### <a name="i-want-to-allow-users-from-my-business-partners-directory-including-users-not-yet-in-my-directory-to-request-access-to-groups-applications-or-sharepoint-sites"></a>我想要允许业务合作伙伴目录中的用户（包括尚不在我目录中的用户）请求对组、应用程序或 SharePoint 站点的访问权限

> [!div class="mx-tableFixed"]
> | 步骤 | 示例 |
> | --- | --- |
> | **1.** [在目录中创建新的访问包](entitlement-management-access-package-create.md#start-new-access-package) | ![创建访问包](./media/entitlement-management-scenarios/access-package.png) |
> | **2.** [向访问包添加资源角色](entitlement-management-access-package-edit.md#add-resource-roles) | ![添加资源角色](./media/entitlement-management-scenarios/resource-roles.png) |
> | **3.** [为外部用户添加策略](entitlement-management-access-package-edit.md#policy-for-users-not-in-your-directory)<ul><li>适用于不在目录中的用户</li><li>需要审批</li><li>过期设置</li></ul> | ![为外部用户添加策略](./media/entitlement-management-scenarios/policy-external.png) |
> | **4.** [将用于请求访问包的“我的访问权限”门户链接发送给业务合作伙伴](entitlement-management-access-package-edit.md#copy-my-access-portal-link)<ul><li>业务合作伙伴可以与其用户共享链接</li></ul> |  |

### <a name="i-want-to-change-the-groups-applications-or-sharepoint-sites-in-an-access-package"></a>我想要更改访问包中的组、应用程序或 SharePoint 站点

> [!div class="mx-tableFixed"]
> | 步骤 | 示例 |
> | --- | --- |
> | **1.** 打开访问包 | ![添加资源角色](./media/entitlement-management-scenarios/resource-roles.png) |
> | **2.** [添加或删除资源角色](entitlement-management-access-package-edit.md#add-resource-roles) | ![添加资源角色](./media/entitlement-management-scenarios/resource-roles-add.png) |

### <a name="i-want-to-view-who-has-an-assignment-to-groups-applications-or-sharepoint-sites"></a>我想要查看谁对组、应用程序或 SharePoint 站点进行了分配

> [!div class="mx-tableFixed"]
> | 步骤 | 示例 |
> | --- | --- |
> | **1.** 打开访问包 | ![添加资源角色](./media/entitlement-management-scenarios/resource-roles.png) |
> | **2.** [查看分配](entitlement-management-access-package-edit.md#view-who-has-an-assignment)<ul><li>查看哪些用户可以访问某个访问包</li><li>查看哪位用户的访问权限已过期</li></ul> |  |

### <a name="i-want-to-view-groups-applications-or-sharepoint-sites-a-user-has-access-to"></a>我想要查看用户有权访问的组、应用程序或 SharePoint 站点

> [!div class="mx-tableFixed"]
> | 步骤 | 示例 |
> | --- | --- |
> | [查看用户分配报表](entitlement-management-reports.md)<ul><li>查看他们请求的时间以及谁进行的审批</li></ul> |  |

## <a name="approvers"></a>审批者

### <a name="i-want-to-approve-requests-to-access-groups-applications-or-sharepoint-sites"></a>我想要审批对组、应用程序或 SharePoint 站点的访问请求

> [!div class="mx-tableFixed"]
> | 步骤 | 示例 |
> | --- | --- |
> | **1.** [在“我的访问权限”门户中打开请求](entitlement-management-request-approve.md#open-request) | [![“我的访问权限”门户图标](./media/entitlement-management-scenarios/my-access-portal.png)](./media/entitlement-management-scenarios/my-access-portal-expanded.png#lightbox) |
> | **2.** [审批访问请求](entitlement-management-request-approve.md#approve-or-deny-request) | ![批准访问](./media/entitlement-management-scenarios/approve-access.png) |

## <a name="requestors"></a>请求者

### <a name="i-want-to-view-the-groups-applications-or-sharepoint-sites-available-to-me-and-request-access"></a>我想要查看可供我访问的组、应用程序或 SharePoint 站点并请求访问权限

> [!div class="mx-tableFixed"]
> | 步骤 | 示例 |
> | --- | --- |
> | **1.** [登录到“我的访问权限”门户](entitlement-management-request-access.md#sign-in-to-the-my-access-portal) | [![“我的访问权限”门户图标](./media/entitlement-management-scenarios/my-access-portal.png)](./media/entitlement-management-scenarios/my-access-portal-expanded.png#lightbox) |
> | **2.** 查找访问包 |  |
> | **3.** [请求访问权限](entitlement-management-request-access.md#request-an-access-package) | ![请求访问权限](./media/entitlement-management-scenarios/request-access.png) |

### <a name="im-an-external-user-and-i-want-to-request-access-to-groups-applications-or-sharepoint-sites-with-a-direct-link"></a>我是外部用户，我想要请求通过直接链接对组、应用程序或 SharePoint 站点进行访问

> [!div class="mx-tableFixed"]
> | 步骤 | 示例 |
> | --- | --- |
> | **1.** [查找你接收的“我的访问权限”门户链接](entitlement-management-access-package-edit.md#copy-my-access-portal-link) |  |
> | **2.** [登录到“我的访问权限”门户](entitlement-management-request-access.md#sign-in-to-the-my-access-portal) | [![“我的访问权限”门户图标](./media/entitlement-management-scenarios/my-access-portal.png)](./media/entitlement-management-scenarios/my-access-portal-expanded.png#lightbox) |
> | **3.** [请求访问权限](entitlement-management-request-access.md#request-an-access-package) | ![请求访问外部用户](./media/entitlement-management-scenarios/request-access-external.png) |

### <a name="i-want-to-view-the-groups-applications-or-sharepoint-sites-i-already-have-access-to"></a>我想要查看我已经有权访问的组、应用程序或 SharePoint 站点

> [!div class="mx-tableFixed"]
> | 步骤 | 示例 |
> | --- | --- |
> | **1.** [登录到“我的访问权限”门户](entitlement-management-request-access.md#sign-in-to-the-my-access-portal) | [![“我的访问权限”门户图标](./media/entitlement-management-scenarios/my-access-portal.png)](./media/entitlement-management-scenarios/my-access-portal-expanded.png#lightbox) |
> | **2.** 查看活动访问包 |  |

## <a name="next-steps"></a>后续步骤

- [教程：创建第一个访问包](entitlement-management-access-package-first.md)
- [委托任务](entitlement-management-delegate.md)

