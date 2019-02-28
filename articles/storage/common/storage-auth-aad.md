---
title: 使用 Azure Active Directory 验证对 Azure blob 和队列的访问权限（预览版） | Microsoft Docs
description: 使用 Azure Active Directory 验证对 Azure blob 和队列的访问权限（预览版）。
services: storage
author: WenJason
ms.service: storage
ms.topic: article
origin.date: 10/15/2018
ms.date: 02/25/2019
ms.author: v-jay
ms.subservice: common
ms.openlocfilehash: 3bec7258dc81bcac57d45de7df2b786f85c08f04
ms.sourcegitcommit: 0fd74557936098811166d0e9148e66b350e5b5fa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/22/2019
ms.locfileid: "56665775"
---
# <a name="authenticate-access-to-azure-blobs-and-queues-using-azure-active-directory-preview"></a>使用 Azure Active Directory 验证对 Azure blob 和队列的访问权限（预览版）

Azure 存储支持为 Blob 和队列服务使用 Azure Active Directory (AD) 进行身份验证和授权。 通过 Azure AD，可以使用基于角色的访问控制 (RBAC) 向用户、组或应用程序服务主体授予访问权限。 

与其他授权方式相比，使用 Azure AD 凭据对用户或应用程序进行身份验证可以提供优越的安全性和易用性。 虽然可以继续为应用程序使用共享密钥授权，但是，使用 Azure AD 不需要将帐户访问密钥与代码存储在一起。 也可以继续使用共享访问签名 (SAS) 授予对存储帐户中的资源的精细访问权限，但 Azure AD 提供了类似的功能，并且不需要管理 SAS 令牌，也不需要担心吊销已泄露的 SAS。 Azure 建议尽量对 Azure 存储应用程序使用 Azure AD 身份验证。

[!INCLUDE [storage-auth-aad-note-include](../../../includes/storage-auth-aad-note-include.md)]

## <a name="about-the-preview"></a>关于此预览版

对于此预览版，请注意以下几点：

- 在此预览版中，Azure AD 集成仅可用于 Blob 和队列服务。
- Azure AD 集成可用于所有公共区域中的 GPv1、GPv2 和 Blob 存储帐户。 
- 仅支持使用资源管理器部署模型创建的存储帐户。 
- 不久将支持在 Azure 存储分析日志中记录调用方标识信息。
- 当前支持通过 Azure AD 授权访问标准存储帐户中的资源。 不久将支持授权访问高级存储帐户中的页 blob。
- Azure 存储同时支持内置的和自定义 RBAC 角色。 可以分配作用域为订阅、资源组、存储帐户或单个容器或队列的角色。
- 目前支持 Azure AD 集成的 Azure 存储客户端库包括：
    - [.NET](https://www.nuget.org/packages/WindowsAzure.Storage)
    - [Java](http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage)
    - Python
        - [Blob、队列和表](https://github.com/Azure/azure-storage-python)
    - [Node.js](https://www.npmjs.com/package/azure-storage)
    - [JavaScript](https://aka.ms/downloadazurestoragejs)

## <a name="get-started-with-azure-ad-for-storage"></a>开始将 Azure AD 用于存储

将 Azure AD 与 Azure 存储集成的第一步是为服务主体（用户、组或应用程序服务主体）或 Azure 资源的托管标识分配对存储数据的 RBAC 角色。 RBAC 角色包含针对容器和队列的共有权限集。 若要详细了解 Azure 存储中的 RBAC 角色，请参阅[通过 RBAC 管理对存储数据的访问权限（预览版）](storage-auth-aad-rbac.md)。

Azure CLI 和 PowerShell 现在支持使用 Azure AD 标识创建日志记录。 使用 Azure AD 标识登录后，会话将以该标识的名义运行。 若要了解详细信息，请参阅[使用 Azure AD 标识通过 CLI 或 PowerShell 访问 Azure 存储（预览版）](storage-auth-aad-script.md)。

