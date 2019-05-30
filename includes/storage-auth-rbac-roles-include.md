---
title: include 文件
description: include 文件
services: storage
author: WenJason
ms.service: storage
ms.topic: include
origin.date: 03/21/2019
ms.date: 05/27/2019
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: b53ea96650dad7118e0e1d3cc58fc9e6ffbf58a7
ms.sourcegitcommit: bf4afcef846cc82005f06e6dfe8dd3b00f9d49f3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2019
ms.locfileid: "66004043"
---
Azure 提供了以下内置的 RBAC 角色，用于授权访问 blob 和队列数据：

- [存储 Blob 数据所有者](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-owner)：用来为 Azure Data Lake Storage Gen2（预览版）设置所有权和管理 POSIX 访问控制。 有关详细信息，请参阅 [Azure Data Lake Storage Gen2 中的访问控制](../articles/storage/blobs/data-lake-storage-access-control.md)。
- [存储 Blob 数据参与者](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-contributor)：用来授予对 Blob 存储资源的读取/写入/删除权限。
- [存储 Blob 数据读取者](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-reader)：用来授予对 Blob 存储资源的只读权限。
- [存储队列数据参与者](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-contributor)：用来授予对 Azure 队列的读取/写入/删除权限。
- [存储队列数据读取者](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-reader)：用来授予对 Azure 队列的只读权限。
- [存储队列数据消息处理者](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-message-processor)：用来对 Azure 存储队列中的消息授予扫视、检索和删除权限。
- [存储队列数据消息发送者](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-message-sender)：用来对 Azure 存储队列中的消息授予添加权限。

> [!NOTE]
> 请记住，RBAC 角色分配可能需要最多五分钟的时间进行传播。
