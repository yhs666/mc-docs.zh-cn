---
title: Azure 队列存储简介 | Azure
description: Azure 队列存储简介
services: storage
documentationcenter: ''
author: forester123
manager: digimobile
editor: tysonn
ms.assetid: ''
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 08/07/2017
ms.date: 10/30/2017
ms.author: v-johch
ms.openlocfilehash: 239d03dff09060ce2e6c299b32dcf7161f8422f6
ms.sourcegitcommit: 71c3744a54c69e7e322b41439da907c533faba39
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2017
ms.locfileid: "23481821"
---
# <a name="introduction-to-queues"></a>队列简介

Azure 队列存储是一项可存储大量消息的服务，用户可以通过经验证的呼叫，使用 HTTP 或 HTTPS 从世界任何地方访问这些消息。 一条队列消息的大小最多可为 64 KB，一个队列中可以包含数百万条消息，直至达到存储帐户的总容量限值。

## <a name="common-uses"></a>常见用途

队列存储的常见用途包括：

* 创建积压工作以进行异步处理
* 将消息从 Azure Web 角色传递到 Azure 辅助角色

## <a name="queue-service-concepts"></a>队列服务概念

队列服务包含以下组件：

![队列概念](./media/storage-queues-introduction/queue1.png)

* **URL 格式：** 可使用以下 URL 格式对队列进行寻址：   
    http://`<storage account>`.queue.core.chinacloudapi.cn/`<queue>` 

    可使用以下 URL 访问示意图中的某个队列：  

    `http://myaccount.queue.core.chinacloudapi.cn/images-to-download`

* **存储帐户：** 对 Azure 存储的所有访问都要通过存储帐户来完成。 有关存储帐户容量的详细信息，请参阅 [Azure 存储可伸缩性和性能目标](../common/storage-scalability-targets.md?toc=%2fstorage%2fqueues%2ftoc.json) 。

* **队列：** 一个队列包含一组消息。 所有消息必须位于相应的队列中。 请注意，队列名称必须全部小写。 有关命名队列的详细信息，请参阅 [命名队列和元数据](https://msdn.microsoft.com/library/azure/dd179349.aspx)。

* **消息：** 一条消息（不管采用何种格式）的最大大小为 64 KB。 消息可以保留在队列中的最长时间为 7 天。

## <a name="next-steps"></a>后续步骤

* [创建存储帐户](../storage-create-storage-account.md?toc=%2fstorage%2fqueues%2ftoc.json)
* [使用 .NET 的队列入门](storage-dotnet-how-to-use-queues.md)