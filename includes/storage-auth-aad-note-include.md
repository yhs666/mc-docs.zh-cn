---
title: include 文件
description: include 文件
services: storage
author: WenJason
ms.service: storage
ms.topic: include
origin.date: 10/09/2018
ms.date: 03/25/2019
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: d6041350cb8464a997cce247ff17ae03e1807609
ms.sourcegitcommit: c70402dacd23ccded50ec6aea9f27f1cf0ec22ba
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2019
ms.locfileid: "58253942"
---
> [!NOTE]
> - 适用于 blob 和队列的 Azure AD 身份验证预览版仅可用于非生产用途。 生产服务级别协议 (SLA) 当前不可用。 如果你的方案尚不支持 Azure AD 身份验证，请继续使用应用程序中的共享密钥授权或 SAS 令牌。
>
> - 预览期间，RBAC 角色分配可能需要最多五分钟的时间进行传播。
>
> - 要使用 OAuth 令牌授权 blob 和队列操作，必须使用 HTTPS。
>
> - Azure 门户现支持使用 Azure AD 凭据来读取和写入 blob 和队列数据，这是预览版本中的一部分。 若要访问 Azure 门户中的 Blob 和队列数据，则除了适用于 Blob 或队列访问的预览角色，用户还必须获得 Azure 资源管理器读者 RBAC 角色。 有关详细信息，请参阅[在 Azure 门户中使用 RBAC 授予对 Azure 容器和队列的访问权限（预览）](../articles/storage/common/storage-auth-aad-rbac.md)。 
> 
> - [Azure 存储资源管理器](https://azure.microsoft.com/features/storage-explorer/)当前使用存储帐户密钥访问 blob 和队列数据。 Blob 支持 OAuth 访问。
