---
title: "Azure 服务总线管理库 | Azure"
description: "通过 .NET 管理服务总线命名空间和消息实体"
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
origin.date: 10/18/2017
ms.date: 12/11/2017
ms.author: v-yiso
ms.openlocfilehash: 3fe55029d5c290966ba4938aa6cb9cc51c865703
ms.sourcegitcommit: 2291ca1f5cf86b1402c7466d037a610d132dbc34
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2017
---
# <a name="service-bus-management-libraries"></a>服务总线管理库

Azure 服务总线管理库可以动态预配服务总线命名空间和实体。 这样可以实现复杂的部署和消息方案，并能以编程方式确定要预配的实体。 这些库目前可用于 .NET。

## <a name="supported-functionality"></a>受支持的功能

* 创建、更新、删除命名空间
* 创建、更新、删除队列
* 创建、更新、删除主题
* 创建、更新、删除订阅

## <a name="prerequisites"></a>先决条件

若要开始使用服务总线管理库，必须使用 Azure Active Directory (AAD) 服务进行身份验证。 AAD 要求身份验证为服务主体，且该主体提供对 Azure 资源的访问权限。 有关创建服务主体的信息，请参阅以下文章之一：  

* [使用 Azure 门户创建可访问资源的 Active Directory 应用程序和服务主体](../azure-resource-manager/resource-group-create-service-principal-portal.md)
* [使用 Azure PowerShell 创建服务主体来访问资源](../azure-resource-manager/resource-group-authenticate-service-principal.md)
* [使用 Azure CLI 创建服务主体来访问资源](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md)

这些教程提供 `AppId`（客户端 ID）、`TenantId` 和 `ClientSecret`（身份验证密钥），这些都将用于管理库进行的身份验证。 若要对资源组运行命令，必须拥有“所有者”权限。

## <a name="programming-pattern"></a>编程模式

所有服务总线资源的操纵模式都遵循常用协议：

1. 使用 **Microsoft.IdentityModel.Clients.ActiveDirectory** 库从 Azure Active Directory 获取令牌。
    ```csharp
    var context = new AuthenticationContext($"https://login.chinacloudapi.cn/{tenantId}");

    var result = await context.AcquireTokenAsync(
        "https://management.core.chinacloudapi.cn/",
        new ClientCredential(clientId, clientSecret)
    );
    ```

1. 创建 `ServiceBusManagementClient` 对象。
    ```csharp
    var creds = new TokenCredentials(token);
    var sbClient = new ServiceBusManagementClient(creds)
    {
        SubscriptionId = SettingsCache["SubscriptionId"]
    };
    ```

1. 将 CreateOrUpdate 参数设置为指定的值。
    ```csharp
    var queueParams = new QueueCreateOrUpdateParameters()
    {
        Location = SettingsCache["DataCenterLocation"],
        EnablePartitioning = true
    };
    ```

1. 执行调用。
    ```csharp
    await sbClient.Queues.CreateOrUpdateAsync(resourceGroupName, namespaceName, QueueName, queueParams);
    ```

## <a name="next-steps"></a>后续步骤
* [.NET 管理示例](https://github.com/Azure-Samples/service-bus-dotnet-management/)
* [Microsoft.Azure.Management.ServiceBus API 参考](https://docs.microsoft.com/en-us/dotnet/api/Microsoft.Azure.Management.ServiceBus)