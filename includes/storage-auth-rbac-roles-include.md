---
title: include 文件
description: include 文件
services: storage
author: WenJason
ms.service: storage
ms.topic: include
origin.date: 07/25/2019
ms.date: 10/28/2019
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: d3a30fc89e4eda4f9a358888f715f51400f880c2
ms.sourcegitcommit: 73f07c008336204bd69b1e0ee188286d0962c1d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2019
ms.locfileid: "72914405"
---
Azure 提供以下内置 RBAC 角色，用于授权使用 Azure AD 和 OAuth 访问 blob 和队列数据：

- [存储 Blob 数据所有者](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-owner)：用于为 Azure Data Lake Storage Gen2 设置所有权和管理 POSIX 访问控制。 有关详细信息，请参阅 [Azure Data Lake Storage Gen2 中的访问控制](../articles/storage/blobs/data-lake-storage-access-control.md)。
- [存储 Blob 数据参与者](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-contributor)：用来授予对 Blob 存储资源的读取/写入/删除权限。
- [存储 Blob 数据读取者](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-reader)：用来授予对 Blob 存储资源的只读权限。
- [存储队列数据参与者](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-contributor)：用来授予对 Azure 队列的读取/写入/删除权限。
- [存储队列数据读取者](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-reader)：用来授予对 Azure 队列的只读权限。
- [存储队列数据消息处理者](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-message-processor)：用来对 Azure 存储队列中的消息授予扫视、检索和删除权限。
- [存储队列数据消息发送者](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-message-sender)：用来对 Azure 存储队列中的消息授予添加权限。

> [!NOTE]
> RBAC 角色分配可能需要最多五分钟的时间进行传播。
>
> 只有为数据访问显式定义的角色才允许安全主体访问 blob 或队列数据。 **所有者**、**参与者**和**存储帐户参与者**等角色允许安全主体管理存储帐户，但不提供对该帐户中 blob 或队列数据的访问。
