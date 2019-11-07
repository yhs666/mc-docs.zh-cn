---
author: WenJason
ms.service: storage
ms.topic: include
origin.date: 10/26/2018
ms.date: 10/28/2019
ms.author: v-jay
ms.custom: seo-python-october2019
ms.openlocfilehash: 73e021a8dcf9c0803b4e5fdd286c6e40d2cb97ce
ms.sourcegitcommit: 73f07c008336204bd69b1e0ee188286d0962c1d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2019
ms.locfileid: "72914384"
---
## <a name="what-is-queue-storage"></a>什么是队列存储？

Azure 队列存储是一项可存储大量消息的服务，用户可以通过经验证的呼叫，使用 HTTP 或 HTTPS 从世界任何地方访问这些消息。 一条队列消息的大小最多可为 64 KB，一个队列中可以包含数百万条消息，直至达到存储帐户的总容量限值。 队列存储通常用于创建要异步处理的积压工作 (backlog)。

## <a name="queue-service-concepts"></a>队列服务概念

Azure 队列服务包含以下组件：

![Azure 队列服务组件](./media/storage-queue-concepts-include/azure-queue-service-components.png)

* **URL 格式：** 可使用以下 URL 格式对队列进行寻址：   
    http://`<storage account>`.queue.core.chinacloudapi.cn/`<queue>` 
  
    可使用以下 URL 访问示意图中的某个队列：  
  
    `http://myaccount.queue.core.chinacloudapi.cn/images-to-download`

* **存储帐户：** 对 Azure 存储进行的所有访问都要通过存储帐户完成。 有关存储帐户容量的详细信息，请参阅 [Azure 存储可伸缩性和性能目标](../articles/storage/common/storage-scalability-targets.md) 。
* **队列：** 一个队列包含一组消息。 所有消息必须位于相应的队列中。 请注意，队列名称必须全部小写。 有关命名队列的详细信息，请参阅 [命名队列和元数据](https://msdn.microsoft.com/library/azure/dd179349.aspx)。
* **消息：** 一条消息（不管采用何种格式）的最大大小为 64 KB。 消息可以保留在队列中的最长时间为 7 天。

