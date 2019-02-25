---
title: 使用 RBAC 管理对容器和队列的访问权限（预览版） - Azure 存储 | Microsoft Docs
description: 使用基于角色的访问控制 (RBAC) 将用于访问 blob 和队列数据的角色分配给用户、组、应用程序服务主体或托管服务标识。 Azure 存储支持通过内置角色和自定义角色来访问容器和队列。
services: storage
author: WenJason
ms.service: storage
ms.topic: article
origin.date: 12/12/2018
ms.date: 02/25/2019
ms.author: v-jay
ms.subservice: common
ms.openlocfilehash: de2283d40b64816f935cbbb7495fa735b14ad8c2
ms.sourcegitcommit: 0fd74557936098811166d0e9148e66b350e5b5fa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/22/2019
ms.locfileid: "56665781"
---
# <a name="manage-access-rights-to-azure-blob-and-queue-data-with-rbac-preview"></a>使用 RBAC 管理对 Azure Blob 和队列数据的访问权限（预览版）

Azure Active Directory (Azure AD) 通过[基于角色的访问控制 (RBAC)](/role-based-access-control/overview) 授权访问受保护的资源。 Azure 存储定义了一组内置的 RBAC 角色，它们包含用于访问容器或队列的通用权限集。 在向 Azure AD 标识分配 RBAC 角色时，系统会根据指定的作用域，向该标识授予对这些资源的访问权限。 可以将访问权限限定于订阅、资源组、存储帐户、单个容器或队列级别。 可以使用 Azure 门户、Azure 命令行工具及 Azure 管理 API 分配对 Azure 存储资源的访问权限。 

[!INCLUDE [storage-auth-aad-note-include](../../../includes/storage-auth-aad-note-include.md)]

## <a name="rbac-roles-for-blobs-and-queues"></a>Blob 和队列的 RBAC 角色

Azure 存储同时支持内置的和自定义 RBAC 角色。 Azure 存储提供以下内置 RBAC 角色用于 Azure AD：

- [存储 Blob 数据所有者（预览版）](/role-based-access-control/built-in-roles#storage-blob-data-owner-preview)：用来为 Azure Data Lake Storage Gen2（预览版）设置所有权和管理 POSIX 访问控制。 有关详细信息，请参阅 [Azure Data Lake Storage Gen2 中的访问控制](../blobs/data-lake-storage-access-control.md)。
- [存储 Blob 数据参与者（预览版）](/role-based-access-control/built-in-roles#storage-blob-data-contributor-preview)：用来授予对 Blob 存储资源的读取/写入/删除权限。
- [存储 Blob 数据读者（预览版）](/role-based-access-control/built-in-roles#storage-blob-data-reader-preview)：用来授予对 Blob 存储资源的只读权限。
- [存储队列数据参与者（预览版）](/role-based-access-control/built-in-roles#storage-queue-data-contributor-preview)：用来授予对 Azure 队列的读取/写入/删除权限。
- [存储队列数据读者（预览版）](/role-based-access-control/built-in-roles#storage-queue-data-reader-preview)：用来授予对 Azure 队列的只读权限。

有关如何为 Azure 存储定义内置角色的详细信息，请参阅[了解角色定义](/role-based-access-control/role-definitions#management-and-data-operations-preview)。

还可以定义用于容器和队列的自定义角色。 有关详细信息，请参阅[针对 Azure 基于角色的访问控制创建自定义角色](/role-based-access-control/custom-roles)。 

## <a name="assign-a-role-to-a-security-principal"></a>向安全主体分配角色

向 Azure 标识分配 RBAC 角色，以授予对存储帐户中容器或队列的权限。 可以将角色分配范围限定于存储帐户或特定的容器或队列。 下表总结了由内置角色授予的访问权限，具体取决于作用域：

|作用域|Blob 数据所有者|Blob 数据参与者|Blob 数据读取器|队列数据参与者|队列数据读取器|
|---|---|---|---|---|---|
|订阅级别|对订阅中的所有容器和 blob 具有读/写访问权限和 POSIX 访问控制管理权限|对订阅中的所有容器和 blob 具有读/写访问权限| 对订阅中的所有容器和 blob 具有读取访问权限|对订阅中的所有队列具有读/写访问权限|对订阅中的所有队列具有读取访问权限|
|资源组级别|对资源组中的所有容器和 blob 具有读/写访问权限和 POSIX 访问控制管理权限|对资源组中的所有容器和 blob 具有读/写访问权限|对资源组中的所有容器和 blob 具有读取访问权限|对资源组中的所有队列具有读/写访问权限|对资源组中的所有队列具有读取访问权限|
|存储帐户级别|对存储帐户中的所有容器和 blob 具有读/写访问权限和 POSIX 访问控制管理权限|对存储帐户中的所有容器和 blob 具有读/写访问权限|对存储帐户中的所有容器和 blob 具有读取访问权限|对存储帐户中的所有队列具有读/写访问权限|对存储帐户中的所有队列具有读取访问权限|
|容器/队列级别|对指定容器及其 blob 具有读/写访问权限和 POSIX 访问控制管理权限。|对指定容器及其 blob 具有读/写访问权限|对指定容器及其 blob 具有读取访问权限|对指定队列具有读/写访问权限|对指定队列具有读取访问权限|

> [!NOTE]
> 作为 Azure 存储帐户的所有者，系统不会自动向你分配数据访问权限。 你必须为自己显式分配一个用于 Azure 存储的 RBAC 角色。 可以在订阅、资源组、存储帐户、容器或队列级别分配它。

有关调用 Azure 存储操作所需的权限的详细信息，请参阅[用于调用 REST 操作的权限](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-rest-operations)。

以下部分介绍如何分配限定于存储帐户或限定于单个容器的角色。

### <a name="assign-a-role-scoped-to-the-storage-account-in-the-azure-portal"></a>在 Azure 门户中分配限定于存储帐户的角色

若要在 Azure 门户中分配内置角色，以授予对存储帐户中所有容器或队列的访问权限，请执行以下操作：

1. 在 [Azure 门户](https://portal.azure.cn)中导航到存储帐户。
1. 选择存储帐户，然后选择“访问控制(IAM)”以显示该帐户的访问控制设置。 选择“角色分配”选项卡以查看角色分配列表。

    ![显示存储访问控制设置的屏幕截图](media/storage-auth-aad-rbac/portal-access-control.png)

1. 单击“添加角色分配”按钮以添加一个新角色。
1. 在“添加角色分配”窗口中，选择要分配给 Azure AD 标识的角色。 然后搜索以找到要为其分配该角色的标识。 例如，下图显示分配给用户的“存储 Blob 数据读取器(预览)”角色。

    ![显示如何分配 RBAC 角色的屏幕截图](media/storage-auth-aad-rbac/add-rbac-role.png)

1. 单击“保存” 。 分配有该角色的标识列出在该角色下。 例如，下图显示所添加的用户现在对存储帐户中的所有 blob 数据具有读取权限。

    ![显示分配有角色的用户列表的屏幕截图](media/storage-auth-aad-rbac/account-scoped-role.png)

## <a name="next-steps"></a>后续步骤

- 若要详细了解 RBAC，请参阅[什么是基于角色的访问控制 (RBAC)？](../../role-based-access-control/overview.md)
- 若要了解如何使用 Azure PowerShell、Azure CLI 或 REST API 分配和管理 RBAC 角色分配，请参阅以下文章：
    - [使用 Azure PowerShell 管理基于角色的访问控制 (RBAC)](../../role-based-access-control/role-assignments-powershell.md)
    - [使用 Azure CLI 管理基于角色的访问控制 (RBAC)](../../role-based-access-control/role-assignments-cli.md)
    - [使用 REST API 管理基于角色的访问控制 (RBAC)](../../role-based-access-control/role-assignments-rest.md)
