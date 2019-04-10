---
title: 使用 Azure Active Directory 验证对 Azure 存储的访问权限 | Microsoft Docs
description: 使用 Azure Active Directory 验证对 Azure 存储的访问权限。
services: storage
author: WenJason
ms.service: storage
ms.topic: article
origin.date: 03/21/2019
ms.date: 04/08/2019
ms.author: v-jay
ms.subservice: common
ms.openlocfilehash: 153496fa39a215ae027e5db262f40c953f748026
ms.sourcegitcommit: b7cefb6ad34a995579a42b082dcd250eb79068a2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/03/2019
ms.locfileid: "58890100"
---
# <a name="authenticate-access-to-azure-storage-using-azure-active-directory"></a>使用 Azure Active Directory 验证对 Azure 存储的访问权限

Azure 存储支持为存储服务使用 Azure Active Directory (AD) 进行身份验证和授权。 通过 Azure AD，可以使用基于角色的访问控制 (RBAC) 向用户、组或应用程序服务主体授予访问权限。 

与其他授权方式相比，使用 Azure AD 凭据对用户或应用程序进行身份验证可以提供优越的安全性和易用性。 虽然可以继续为应用程序使用共享密钥授权，但是，使用 Azure AD 不需要将帐户访问密钥与代码存储在一起。 也可以继续使用共享访问签名 (SAS) 授予对存储帐户中的资源的精细访问权限，但 Azure AD 提供了类似的功能，并且不需要管理 SAS 令牌，也不需要担心吊销已泄露的 SAS。 Azure 建议尽量对 Azure 存储应用程序使用 Azure AD 身份验证。

可以针对所有公共区域的所有常规用途 v2 帐户、常规用途 v1 帐户和 Blob 存储帐户使用 Azure AD 凭据进行身份验证和授权。 仅通过 Azure 资源管理器部署模型创建的存储帐户支持 Azure AD 授权。

[!INCLUDE [storage-auth-aad-note-include](../../../includes/storage-auth-aad-note-include.md)]

## <a name="overview-of-azure-ad-for-azure-storage"></a>用于 Azure 存储的 Azure AD 概述

将 Azure AD 与 Azure 存储集成的第一步是为服务主体（用户、组或应用程序服务主体）或 Azure 资源的托管标识分配对存储数据的 RBAC 角色。 RBAC 角色包含针对存储服务的共有权限集。 若要详细了解如何为 Azure 存储分配 RBAC 角色，请参阅[通过 RBAC 管理存储数据访问权限](storage-auth-aad-rbac.md)。

Azure CLI 和 PowerShell 现在支持使用 Azure AD 标识创建日志记录。 使用 Azure AD 标识登录后，会话将以该标识的名义运行。 若要了解详细信息，请参阅[使用 Azure AD 标识通过 CLI 或 PowerShell 访问 Azure 存储](storage-auth-aad-script.md)。

## <a name="rbac-roles-for-azure-storage"></a>Azure 存储的 RBAC 角色

Azure Active Directory (Azure AD) 通过[基于角色的访问控制 (RBAC)](../../role-based-access-control/overview.md) 授权访问受保护的资源。 Azure 存储定义了一组内置的 RBAC 角色，它们包含用于访问存储帐户的通用权限集。 

将 RBAC 角色分配到 Azure AD 安全主体后，Azure 会向该安全主体授予对这些资源的访问权限。 访问权限可以局限到订阅、资源组或存储帐户级别。 Azure AD 安全主体可以是用户、组或应用程序服务主体。 

[!INCLUDE [storage-auth-rbac-roles-include](../../../includes/storage-auth-rbac-roles-include.md)]

若要了解如何在 Azure 门户中分配内置的角色，请参阅[在 Azure 门户中使用 RBAC 授予对 Azure 容器和队列的访问权限](storage-auth-aad-rbac.md)。

### <a name="access-permissions-granted-by-rbac-roles"></a>通过 RBAC 角色授予的访问权限 

有关调用 Azure 存储操作所需的权限的详细信息，请参阅[用于调用 REST 操作的权限](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-rest-operations)。

## <a name="next-steps"></a>后续步骤

- [在 Azure 门户中使用 RBAC 授予对 Azure 容器和队列的访问权限](storage-auth-aad-rbac.md)
- [使用 Azure AD 标识通过 CLI 或 PowerShell 访问 Azure 存储](storage-auth-aad-script.md)