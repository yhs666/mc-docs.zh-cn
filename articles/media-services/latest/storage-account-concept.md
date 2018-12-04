---
title: Azure 媒体服务中的存储帐户 | Azure
description: 本文讨论媒体服务如何使用存储帐户。
services: media-services
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
origin.date: 03/19/2018
ms.date: 05/28/2018
ms.author: v-nany
ms.openlocfilehash: d5f915e70eb6643239bfff02c8a4fc25cbf20cdc
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52644943"
---
# <a name="storage-accounts"></a>存储帐户

创建媒体服务帐户时，需要提供 Azure 存储帐户资源的名称。 指定存储帐户会附加到媒体服务帐户。 

必须具有一个主存储帐户，并且可以拥有任意数量的与媒体服务帐户关联的辅助存储帐户。 媒体服务支持常规用途 v2 (GPv2) 或常规用途 v1 (GPv1) 帐户。 

>[!NOTE]
> 不允许将仅限 Blob 的帐户作为主帐户。 

我们建议使用 GPv2，以便在热存储层和冷存储层之间进行选择。 若要了解存储帐户的详细信息，请参阅 [Azure 存储帐户选项](../../storage/common/storage-account-options.md)。 

## <a name="assets-in-a-storage-account"></a>存储帐户中的资产

在媒体服务 v3 中，存储 API 用于上传文件。 若要了解如何结合使用存储 API 与媒体服务来上传输入文件，请查看[从本地文件创建作业输入](job-input-from-local-file-how-to.md)。 

> [!Note]
> 在不使用媒体服务 API 的情况下，不应该试更改媒体服务 SDK 生成的 BLOB 容器内容。

