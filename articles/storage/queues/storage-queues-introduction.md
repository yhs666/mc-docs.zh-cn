---
title: Azure 队列简介 | Microsoft Docs
description: Azure 队列简介
services: storage
author: WenJason
ms.service: storage
ms.topic: article
origin.date: 02/06/2019
ms.date: 02/25/2019
ms.author: v-jay
ms.subservice: queues
ms.openlocfilehash: 1b40587234fd78fab40a84ac8fd400c263529ae9
ms.sourcegitcommit: 0fd74557936098811166d0e9148e66b350e5b5fa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/22/2019
ms.locfileid: "56665635"
---
# <a name="what-are-azure-queues"></a>什么是 Azure 队列？

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

* **存储帐户：** 对 Azure 存储进行的所有访问都要通过存储帐户完成。 有关存储帐户容量的详细信息，请参阅 [Azure 存储可伸缩性和性能目标](../common/storage-scalability-targets.md?toc=%2fstorage%2fqueues%2ftoc.json) 。

* **队列：** 一个队列包含一组消息。 所有消息必须位于相应的队列中。 请注意，队列名称必须全部小写。 有关命名队列的详细信息，请参阅 [命名队列和元数据](https://msdn.microsoft.com/library/azure/dd179349.aspx)。

* **消息：** 一条消息（不管采用何种格式）的最大大小为 64 KB。 消息可以保留在队列中的最长时间为 7 天。

## <a name="next-steps"></a>后续步骤

* [创建存储帐户](../storage-create-storage-account.md?toc=%2fstorage%2fqueues%2ftoc.json)
* [使用 .NET 的队列入门](storage-dotnet-how-to-use-queues.md)