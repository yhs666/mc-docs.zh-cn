---
title: Azure IoT 解决方案加速器和 Azure Active Directory | Microsoft Docs
description: 介绍了 Azure IoT 解决方案加速器如何使用 Azure Active Directory 来管理权限。
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
origin.date: 11/10/2017
ms.author: v-yiso
ms.date: 11/26/2018
ms.openlocfilehash: aba70eb07ea1de75f3c3414858534bd660558aad
ms.sourcegitcommit: 5f2849d5751cb634f1cdc04d581c32296e33ef1b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2018
ms.locfileid: "53028434"
---
# <a name="permissions-on-the-azureiotsuitecn-site"></a>azureiotsuite.cn 站点权限
## <a name="what-happens-when-you-sign-in"></a>登录时发生的情况
在 [azureiotsuite.cn][lnk-azureiotsolutions] 上首次登录时，站点会基于当前所选 Azure Active Directory (AAD) 租户和 Azure 订阅来确定所拥有的权限级别。

1. 站点首先从 Azure 查明用户所属的 AAD 租户以填充用户名旁显示的租户列表。 当前，站点一次只能获取一个租户的用户令牌。 因此，当使用右上角的下拉列表切换租户时，站点会使你登录到该租户，以获取该租户的令牌。

2. 接下来，站点从 Azure 查明你已与所选租户关联的订阅。 创建新的解决方案加速器时，会看到可用订阅。

3. 最后，站点检索标记为解决方案加速器的订阅和资源组中的所有资源，并填充主页上的磁贴。

下列各部分介绍了用于控制对解决方案加速器的访问的角色。

## <a name="aad-roles"></a>AAD 角色

AAD 角色控制预配解决方案加速器，以及在解决方案加速器中管理用户和一些 Azure 服务的能力。

有关 AAD 中的管理员角色的详细信息，可参阅[在 Azure AD 中分配管理员角色][lnk-aad-admin]。 本文重点介绍解决方案加速器使用的**全局管理员**和**用户**目录角色。

### <a name="global-administrator"></a>全局管理员

对于每个 AAD 租户，可以有多个全局管理员：

* 在创建某个 AAD 租户时，默认情况下会成为该租户的全局管理员。
* 全局管理员可以预配基本和标准解决方案加速器（一个基本部署使用单个 Azure 虚拟机）。

### <a name="domain-user"></a>域用户

每个 AAD 租户可以有多个域用户：

* 域用户可以通过 [azureiotsolutions.cn][lnk-azureiotsolutions] 站点预配基本解决方案加速器。
* 域用户可以使用 CLI 创建基本解决方案加速器。

### <a name="guest-user"></a>来宾用户

每个 AAD 租户可以有多个来宾用户。 来宾用户在 AAD 租户中拥有有限的权利集。 因此，来宾用户无法在 AAD 租户中预配解决方案加速器。

有关 AAD 中用户及角色的详细信息，请参阅以下资源：

* [在 Azure AD 中创建用户][lnk-create-edit-users]
* [将用户分配到应用][lnk-assign-app-roles]

## <a name="azure-subscription-administrator-roles"></a>Azure 订阅管理员角色

Azure 管理员角色可控制将 Azure 订阅映射到 AAD 租户的能力。



## <a name="faq"></a>常见问题解答


### <a name="i-want-to-change-a-service-administrator-or-co-administrator-when-logged-in-with-an-organizational-account"></a>我在使用组织帐户登录时要更改服务管理员或共同管理员

请参阅支持文章[使用组织帐户登录时更改服务管理员和共同管理员][lnk-service-admins]。

### <a name="why-am-i-seeing-this-error-your-account-does-not-have-the-proper-permissions-to-create-a-solution-please-check-with-your-account-administrator-or-try-with-a-different-account"></a>为何会出现以下错误？ “你的帐户没有创建解决方案的正确权限。 请咨询帐户管理员或使用其他帐户进行尝试。”
请查看以下指南示意图：

![][img-flowchart]

> [!NOTE]
> 如果在验证你是 AAD 租户的全局管理员和订阅的共同管理员后，仍看到此错误，请让帐户管理员删除该用户，并按以下顺序重新分配必要的权限。 首先，将用户添加为全局管理员，然后将用户添加为 Azure 订阅的共同管理员。 如果问题仍然存在，请联系[帮助和支持][lnk-help-support]。

### <a name="why-am-i-seeing-this-error-when-i-have-an-azure-subscription-an-azure-subscription-is-required-to-create-pre-configured-solutions-you-can-create-a-free-trial-account-in-just-a-couple-of-minutes"></a>为何在我具有 Azure 订阅时会出现以下错误？ “创建预配置解决方案需要 Azure 订阅。 只需几分钟即可创建一个免费试用帐户。”

如果确定具有 Azure 订阅，请验证订阅的租户映射，并确保在下拉列表中选择正确租户。 如果验证了所需租户是正确的，请按照上图，验证订阅和此 AAD 租户的映射。

## <a name="next-steps"></a>后续步骤
若要继续了解 IoT 解决方案加速器，请参阅如何[自定义解决方案加速器][lnk-customize]。

[img-flowchart]: media/iot-accelerators-permissions/flowchart.png

[lnk-azureiotsolutions]: https://www.azureiotsuite.cn/
[lnk-rm-github-repo]: https://github.com/Azure/remote-monitoring-services-dotnet
[lnk-pm-github-repo]: https://github.com/Azure/azure-iot-predictive-maintenance
[lnk-aad-admin]:../active-directory/users-groups-roles/directory-assign-admin-roles.md
[lnk-portal]: https://portal.azure.cn
[lnk-create-edit-users]:../active-directory/fundamentals/active-directory-users-profile-azure-portal.md
[lnk-assign-app-roles]:../active-directory/manage-apps/assign-user-or-group-access-portal.md
[lnk-service-admins]: https://www.azure.cn/support/changing-service-admin-and-co-admin/
[lnk-resource-cs]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/DeviceAdministration/Web/Security/RolePermissions.cs
[lnk-help-support]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[lnk-customize]: iot-accelerators-remote-monitoring-customize.md
