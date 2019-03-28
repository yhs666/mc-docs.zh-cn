---
title: include 文件
description: include 文件
services: storage
author: WenJason
ms.service: storage
ms.topic: include
origin.date: 02/19/2019
ms.date: 03/25/2019
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: bafe6544eed1655c458a10bd479c4a7ffc2e434e
ms.sourcegitcommit: c70402dacd23ccded50ec6aea9f27f1cf0ec22ba
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2019
ms.locfileid: "58253941"
---
Azure 提供以下内置的 RBAC 角色，用于访问存储数据：

- [存储 Blob 数据所有者（预览版）](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-owner-preview)：用来为 Azure Data Lake Storage Gen2（预览版）设置所有权和管理 POSIX 访问控制。 有关详细信息，请参阅 [Azure Data Lake Storage Gen2 中的访问控制](../articles/storage/blobs/data-lake-storage-access-control.md)。
- [存储 Blob 数据参与者（预览版）](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-contributor-preview)：用来授予对 Blob 存储资源的读取/写入/删除权限。
- [存储 Blob 数据读者（预览版）](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-reader-preview)：用来授予对 Blob 存储资源的只读权限。
- [存储队列数据参与者（预览版）](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-contributor-preview)：用来授予对 Azure 队列的读取/写入/删除权限。
- [存储队列数据读者（预览版）](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-reader-preview)：用来授予对 Azure 队列的只读权限。

有关如何为 Azure 存储定义内置角色的详细信息，请参阅[了解角色定义](../articles/role-based-access-control/role-definitions.md#management-and-data-operations-preview)。 若要了解如何创建自定义 RBAC 角色，请参阅[针对 Azure 基于角色的访问控制创建自定义角色](../articles/role-based-access-control/custom-roles.md)。 
