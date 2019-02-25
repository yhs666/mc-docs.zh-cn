---
title: include 文件
description: include 文件
services: storage
author: WenJason
ms.service: storage
ms.topic: include
origin.date: 10/09/2018
ms.date: 02/25/2019
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: a31cdb7bc3ee6daaa6667a9c58713905f54b3baf
ms.sourcegitcommit: 0fd74557936098811166d0e9148e66b350e5b5fa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/22/2019
ms.locfileid: "56665756"
---
> [!NOTE]
> - 适用于 blob 和队列的 Azure AD 身份验证预览版仅可用于非生产用途。 生产服务级别协议 (SLA) 当前不可用。 如果你的方案尚不支持 Azure AD 身份验证，请继续使用应用程序中的共享密钥授权或 SAS 令牌。
>
> - 预览期间，RBAC 角色分配可能需要最多五分钟的时间进行传播。
>
> - 要使用 OAuth 令牌授权 blob 和队列操作，必须使用 HTTPS。
>
> - Azure 门户现支持使用 Azure AD 凭据来读取和写入 blob 和队列数据，这是预览版本中的一部分。
> 
> - [Azure 存储资源管理器](https://azure.microsoft.com/features/storage-explorer/)当前使用存储帐户密钥访问 blob 和队列数据。 Blob 支持 OAuth 访问。
