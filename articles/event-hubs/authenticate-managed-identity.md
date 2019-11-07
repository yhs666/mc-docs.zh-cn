---
title: 使用 Azure Active Directory 对托管标识进行身份验证
description: 本文提供有关对使用 Azure Active Directory 访问 Azure 事件中心资源的托管标识进行身份验证的信息
services: event-hubs
ms.service: event-hubs
documentationcenter: ''
author: spelluru
manager: ''
ms.topic: conceptual
origin.date: 08/22/2019
ms.date: 10/23/2019
ms.author: v-tawe
ms.openlocfilehash: b2b431a70ec75b29c1385ed9fa2f757376ae850e
ms.sourcegitcommit: a1575acb8d0047fae425deb8196e3c89bd3dac57
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2019
ms.locfileid: "72887676"
---
# <a name="authenticate-a-managed-identity-with-azure-active-directory-to-access-event-hubs-resources"></a>使用 Azure Active Directory 对托管标识的事件中心资源访问进行身份验证
Azure 事件中心支持使用 [Azure 资源的托管标识](../active-directory/managed-identities-azure-resources/overview.md)进行 Azure Active Directory (Azure AD) 身份验证。 Azure 资源的托管标识可以从 Azure 虚拟机 (VM)、函数应用、虚拟机规模集和其他服务中运行的应用程序使用 Azure AD 凭据授权对事件中心资源的访问权限。 将 Azure 资源的托管标识与 Azure AD 身份验证结合使用，可避免将凭据随在云中运行的应用程序一起存储。

本文介绍如何在 Azure VM 中使用托管标识授予对事件中心的访问权限。

## <a name="enable-managed-identities-on-a-vm"></a>在 VM 上启用托管标识
在使用 Azure 资源的托管标识对 VM 中的事件中心资源授权之前，必须首先在 VM 上启用 Azure 资源的托管标识。 若要了解如何为 Azure 资源启用托管标识，请参阅下述文章之一：

- [Azure 门户](../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md)
- [Azure PowerShell](../active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm.md)
- [Azure CLI](../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md)
- [Azure Resource Manager 模板](../active-directory/managed-identities-azure-resources/qs-configure-template-windows-vm.md)
- [Azure 资源管理器客户端库](../active-directory/managed-identities-azure-resources/qs-configure-sdk-windows-vm.md)

## <a name="grant-permissions-to-a-managed-identity-in-azure-ad"></a>向 Azure AD 中的托管标识授予权限
若要通过应用程序中的托管标识授权对事件中心服务的请求，请先为该托管标识配置基于角色的访问控制 (RBAC) 设置。 Azure 事件中心定义 RBAC 角色，这些角色涵盖了从事件中心进行发送和读取操作所需的权限。 将 RBAC 角色分配到某个托管标识后，将在适当的范围授予该托管标识访问事件中心数据的权限。

有关如何分配 RBAC 角色的详细信息，请参阅[使用 Azure Active Directory 进行身份验证，以便访问事件中心资源](authorize-access-azure-active-directory.md)。

## <a name="use-event-hubs-with-managed-identities"></a>将事件中心与托管标识结合使用
若要将事件中心与托管标识配合使用，需为标识分配角色和相应的范围。 此部分的过程使用一个简单的应用程序，该应用程序在托管标识下运行并访问事件中心资源。

在这里，我们将使用一个在 [Azure 应用服务](https://www.azure.cn/home/features/app-service/)中托管的示例 Web 应用程序。 有关如何创建 Web 应用程序的分步说明，请参阅[在 Azure 中创建 ASP.NET Core Web 应用](../app-service/app-service-web-get-started-dotnet.md)

创建应用程序后，请执行以下步骤： 

1. 转到“设置”，然后选择“标识”  。  
1. 选择“状态”  ，将其切换到“启用”  。 
1. 选择“保存”  ，保存设置。 

    ![Web 应用的托管标识](./media/authenticate-managed-identity/identity-web-app.png)

启用此设置后，会在 Azure Active Directory (Azure AD) 中创建一个新的服务标识并将其配置到应用服务主机中。

现在，请将此服务标识分配给事件中心资源中所需范围中的某个角色。

### <a name="to-assign-rbac-roles-using-the-azure-portal"></a>使用 Azure 门户分配 RBAC 角色
若要为事件中心资源分配角色，请导航到 Azure 门户中的该资源。 显示资源的“访问控制(标识和访问管理)”设置，并按以下说明管理角色分配：

> [!NOTE]
> 以下步骤为事件中心命名空间分配服务标识角色。 可以遵循相同的步骤来分配限定为事件中心资源范围的角色。 

1. 在 Azure 门户中导航到事件中心命名空间，显示该命名空间的“概览”。  
1. 选择左侧菜单上的“访问控制(标识和访问管理)”，显示事件中心的访问控制设置  。
1.  选择“角色分配”  选项卡以查看角色分配列表。
3.  选择“添加”以添加新角色。 
4.  在“添加角色分配”页上，选择要分配的事件中心角色  。 然后通过搜索找到已注册的服务标识，以便分配该角色。
    
    ![“添加角色分配”页](./media/authenticate-managed-identity/add-role-assignment-page.png)
5.  选择“保存”  。 分配有该角色的标识列出在该角色下。 例如，下图显示服务标识有事件中心数据所有者。
    
    ![分配给角色的标识](./media/authenticate-managed-identity/role-assigned.png)

分配此角色后，Web 应用程序即可访问已定义范围内的事件中心资源。 

### <a name="test-the-web-application"></a>测试 Web 应用程序
现在可以启动 Web 应用程序并将浏览器指向示例 aspx 页面了。 可以在 [GitHub 存储库](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/Rbac/ManagedIdentityWebApp)中找到用于通过事件中心资源发送和接收数据的示例 Web 应用程序。

从 [Nuget](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) 安装最新包，并开始使用 EventHubClient 通过事件中心发送和接收数据，如以下代码所示： 

```csharp
var ehClient = EventHubClient.CreateWithManagedIdentity(new Uri($"sb://{EventHubNamespace}/"), EventHubName);
```

## <a name="next-steps"></a>后续步骤
- 从 GitHub 下载[示例](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/Rbac/ManagedIdentityWebApp)。
- 请参阅下文，了解 Azure 资源的托管标识：[什么是 Azure 资源的托管标识？](../active-directory/managed-identities-azure-resources/overview.md)
- 请参阅以下相关文章：
    - [使用 Azure Active Directory 对应用程序的 Azure 事件中心请求进行身份验证](authenticate-application.md)
    - [使用共享访问签名对 Azure 事件中心请求进行身份验证](authenticate-shared-access-signature.md)
    - [使用 Azure Active Directory 授权访问事件中心资源](authorize-access-azure-active-directory.md)
    - [使用共享访问签名授权访问事件中心资源](authorize-access-shared-access-signature.md)