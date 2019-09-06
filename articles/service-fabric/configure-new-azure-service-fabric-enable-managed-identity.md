---
title: Azure Service Fabric - 部署支持托管标识的新 Azure Service Fabric 群集 | Azure
description: 本文介绍如何创建启用了托管标识的新 Service Fabric 群集
services: service-fabric
author: rockboyfor
ms.service: service-fabric
ms.topic: article
origin.date: 07/25/2019
ms.date: 08/26/2019
ms.author: v-yeche
ms.openlocfilehash: 62828993b0e429361e048aec054b5420a60e29c2
ms.sourcegitcommit: ba87706b611c3fa338bf531ae56b5e68f1dd0cde
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/30/2019
ms.locfileid: "70174539"
---
# <a name="create-a-new-azure-service-fabric-cluster-with-managed-identity-support-preview"></a>创建支持托管标识的新 Azure Service Fabric 群集（预览）

若要访问 Azure Service Fabric 应用程序的托管标识功能，必须先在群集上启用托管标识令牌服务。 此服务负责使用 Service Fabric 应用程序的托管标识对这些应用程序进行身份验证，以及代表它们获取访问令牌。 启用此服务以后，即可在 Service Fabric Explorer 中左侧窗格的“系统”部分  下看到它，它在其他系统服务旁边以 **fabric:/System/ManagedIdentityTokenService** 名称运行。

> [!NOTE]
> 若要启用**托管标识令牌服务**，必须使用 Service Fabric 运行时 6.5.658.9590 或更高版本。  

## <a name="enable-the-managed-identity-token-service"></a>启用托管标识令牌服务 
若要在创建群集时启用托管标识令牌服务，可以在 Azure 资源管理器模板中使用以下代码片段：

```json
"fabricSettings": [
    {
        "name": "ManagedIdentityTokenService",
        "parameters": [
            {
                "name": "IsEnabled",
                "value": "true"
            }
        ]
    }
]
```

## <a name="errors"></a>错误

如果部署失败并显示此消息，则表示群集不在所需的 Service Fabric 版本上（支持的最低运行时为 6.5 CU2）：

```json
{
    "code": "ParameterNotAllowed",
    "message": "Section 'ManagedIdentityTokenService' and Parameter 'IsEnabled' is not allowed."
}
```

## <a name="next-steps"></a>后续步骤
* [使用系统分配的托管标识部署 Azure Service Fabric 应用程序](./how-to-deploy-service-fabric-application-system-assigned-managed-identity.md)
* [使用用户分配的托管标识部署 Azure Service Fabric 应用程序](./how-to-deploy-service-fabric-application-user-assigned-managed-identity.md)
* [从服务代码中利用 Service Fabric 应用程序的托管标识](./how-to-managed-identity-service-fabric-app-code.md)
* [向 Azure Service Fabric 应用程序授予对其他 Azure 资源的访问权限](./how-to-grant-access-other-resources.md)

## <a name="related-articles"></a>相关文章
* 查看 Azure Service Fabric 中的[托管标识支持](./concepts-managed-identity.md)

* [在现有 Azure Service Fabric 群集中启用托管标识支持](./configure-existing-cluster-enable-managed-identity-token-service.md)

<!--Update_Description: new articles on configure new azure service fabric enable managed identity -->
<!--ms.date: 09/02/2019-->