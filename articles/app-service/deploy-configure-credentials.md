---
title: 配置部署凭据 - Azure 应用服务
description: 了解如何使用 Azure 应用服务部署凭据。
services: app-service
documentationcenter: ''
author: cephalin
manager: jpconnoc
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
origin.date: 03/10/2019
ms.date: 08/12/2019
ms.author: v-johch
ms.custom: seodec18
ms.openlocfilehash: 0107f7f65d106dbd95b4e64026de48cd26202382
ms.sourcegitcommit: e9c62212a0d1df1f41c7f40eb58665f4f1eaffb3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/09/2019
ms.locfileid: "68878551"
---
# <a name="configure-deployment-credentials-for-azure-app-service"></a>为 Azure 应用服务配置部署凭据
[Azure 应用服务](app-service-web-overview.md)支持两种类型的凭据，这些凭据适用于[本地 GIT 部署](deploy-local-git.md)和 [FTP/S 部署](deploy-ftp.md)。 这些凭据与 Azure Active Directory 凭据不同。

* **用户级凭据**：一组适用于整个 Azure 帐户的凭据。 在任何订阅中，它均可用于为 Azure 帐户有权访问的任何应用部署应用服务。 这是在门户 GUI（例如应用的[资源页](https://docs.azure.cn/zh-cn/azure-resource-manager/manage-resources-portal#manage-resources)的“概览”和“属性”）中呈现的默认组。   当通过基于角色的访问控制 (RBAC) 或共同管理员权限授予用户应用访问权限时，该用户便可使用其自己的用户级别凭据直到被撤销访问权限。 请勿与其他 Azure 用户共享这些凭据。

* **应用级凭据**：用于每个应用的一组凭据。 只能使用它部署到该应用。 每个应用的凭据在其创建时自动生成。 这些凭据不能手动进行配置，但可随时进行重置。 如果要通过 (RBAC) 授予用户访问应用级别凭据的权限，该用户必须是应用的参与者或更高级别身份。 读者不可进行发布，因此无法访问这些凭据。

## <a name="userscope"></a>设置和重置用户级凭据

可以在任何应用的[资源页面](https://docs.azure.cn/zh-cn/azure-resource-manager/manage-resources-portal#manage-resources)中配置用户级凭据。 无论在哪个应用中配置这些凭据，这些凭据都适用于 Azure 帐户中的所有应用和所有订阅。 

若要配置用户级凭据，请执行以下操作：

1. 在 [Azure](https://portal.azure.cn) 门户中，单击左侧菜单中的“应用服务” > &lt;any_app> > “部署中心” > “部署凭据”     。

    在门户中，用户必须至少有一个应用，才能访问“部署凭据”页面。 但使用 [Azure CLI](/cli/azure/webapp/deployment/user?view=azure-cli-latest#az-webapp-deployment-user-set) 时，可以在没有应用的情况下配置用户级凭据。

2. 单击“用户凭据”，配置用户名和密码，然后单击“保存凭据”   。

    ![](./media/app-service-deployment-credentials/deployment_credentials_configure.png)

设置部署凭据后，可以在应用的 *概述* 中找到 **Git**部署用户名，

![](./media/app-service-deployment-credentials/deployment_credentials_overview.png)

以及在应用的“属性”中查找 FTP 部署用户名。  

![](./media/app-service-deployment-credentials/deployment_credentials_properties.png)

> [!NOTE]
> Azure 不会显示用户级部署密码。 如果忘记密码，可以按照本部分的步骤重置凭据。
>
>  

## <a name="use-user-level-credentials-with-ftpftps"></a>将用户级凭据用于 FTP/FTPS

使用用户级凭据向 FTP/FTPS 终结点进行身份验证时需要使用以下格式的用户名：`<app-name>\<user-name>`

由于用户级凭据链接到用户而不是特定资源，因此用户名必须采用此格式才能将登录操作定向到正确的应用终结点。

## <a name="appscope"></a>设置和重置应用级凭据
若要获取应用级凭据，请执行以下操作：

1. 在 [Azure](https://portal.azure.cn) 门户中，单击左侧菜单中的“应用服务” > &lt;any_app> > “部署中心” > “部署凭据”     。

2. 单击“应用凭据”，然后单击“复制”链接以复制用户名或密码   。

    ![](./media/app-service-deployment-credentials/deployment_credentials_app_level.png)

若要重置应用级别凭据，请单击相同对话框中的“重置凭据”  。

## <a name="next-steps"></a>后续步骤

了解如何使用这些凭据通过[本地 Git](deploy-local-git.md) 或 [FTP/S](deploy-ftp.md) 部署应用。
