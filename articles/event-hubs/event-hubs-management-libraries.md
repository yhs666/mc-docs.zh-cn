---
title: "Azure 事件中心管理库 | Azure"
description: "通过 .NET 管理事件中心命名空间和实体"
services: event-hubs
cloud: na
documentationcenter: na
author: rockboyfor
manager: digimobile
ms.assetid: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
origin.date: 08/15/2017
ms.date: 09/04/2017
ms.author: v-yeche
ms.openlocfilehash: e39941887d03e66665b6ca4a118334478b84d7e6
ms.sourcegitcommit: 095c229b538d9d2fc51e007abe5fde8e46296b4f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/04/2017
---
# <a name="event-hubs-management-libraries"></a>事件中心管理库

事件中心管理库可以动态设置事件中心命名空间和实体。 这样可以实现复杂的部署和消息传递方案，以便能够以编程方式确定要设置的实体。 这些库目前可用于 .NET。

## <a name="supported-functionality"></a>受支持的功能

* 创建、更新、删除命名空间
* 事件中心创建、更新、删除
* 资源组创建、更新、删除

## <a name="prerequisites"></a>先决条件

若要开始使用事件中心管理库，必须使用 Azure Active Directory (AAD) 进行身份验证。 AAD 要求以提供 Azure 资源访问权限的服务主体身份进行身份验证。 有关创建服务主体的信息，请参阅以下文章之一：  

* [使用 Azure 门户创建可访问资源的 Active Directory 应用程序和服务主体](../azure-resource-manager/resource-group-create-service-principal-portal.md)
* [使用 Azure PowerShell 创建服务主体来访问资源](../azure-resource-manager/resource-group-authenticate-service-principal.md)
* [使用 Azure CLI 创建服务主体来访问资源](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md)

这些教程提供 `AppId`（客户端 ID）、`TenantId` 和 `ClientSecret`（身份验证密钥），这些都将用于管理库进行的身份验证。 若要对资源组运行命令，必须拥有“所有者”权限。

## <a name="programming-pattern"></a>编程模式

所有事件中心资源的操纵模式都遵循常用协议：

1. 使用 `Microsoft.IdentityModel.Clients.ActiveDirectory` 库从 AAD 获取令牌。
    ```csharp
    var context = new AuthenticationContext($"https://login.chinacloudapi.cn/{tenantId}");

    var result = await context.AcquireTokenAsync(
        "https://management.core.chinacloudapi.cn/",
        new ClientCredential(clientId, clientSecret)
    );
    ```

2. 创建 `EventHubManagementClient` 对象。

    ```csharp
    var creds = new TokenCredentials(token);
    var ehClient = new EventHubManagementClient(creds)
    {
        SubscriptionId = SettingsCache["SubscriptionId"]
    };
    ```

3. 将 `CreateOrUpdate` 参数设置为指定值。

    ```csharp
    var ehParams = new EventHubCreateOrUpdateParameters()
    {
        Location = SettingsCache["DataCenterLocation"]
    };
    ```

4. 执行调用。

    ```csharp
    await ehClient.EventHubs.CreateOrUpdateAsync(resourceGroupName, namespaceName, EventHubName, ehParams);
    ```

## <a name="next-steps"></a>后续步骤
* [.NET 管理示例](https://github.com/Azure-Samples/event-hubs-dotnet-management/)
* [Microsoft.Azure.Management.EventHub 引用](https://docs.microsoft.com/dotnet/api/Microsoft.Azure.Management.EventHub)

<!--Update_Description: update meta properties, wording update-->