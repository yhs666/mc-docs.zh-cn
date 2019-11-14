---
author: ggailey777
ms.service: azure-functions
ms.topic: include
origin.date: 09/04/2018
ms.date: 10/28/2019
ms.author: v-junlch
ms.openlocfilehash: 3f3568ce4423dc7733409145b37566d3634d7913
ms.sourcegitcommit: 7d2ea8a08ee329913015bc5d2f375fc2620578ba
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2019
ms.locfileid: "73034405"
---
下表显示了 Azure Functions 运行时的两个主版本支持的绑定：

| 类型 | 1.x | 2.x<sup>1</sup> | 触发器 | 输入 | 输出 |
| ---- | :-: | :-: | :------: | :---: | :----: |
| [Blob 存储](../articles/azure-functions/functions-bindings-storage-blob.md)          |✔|✔|✔|✔|✔|
| [Cosmos DB](../articles/azure-functions/functions-bindings-documentdb.md)               |✔|✔|✔|✔|✔|
| [事件网格](../articles/azure-functions/functions-bindings-event-grid.md)              |✔|✔|✔| | |
| [事件中心](../articles/azure-functions/functions-bindings-event-hubs.md)              |✔|✔|✔| |✔|
| [HTTP 和 Webhook](../articles/azure-functions/functions-bindings-http-webhook.md)             |✔|✔|✔| |✔|
| [Microsoft Graph<br/>Excel 表](../articles/azure-functions/functions-bindings-microsoft-graph.md)   ||✔| |✔|✔|
| [Microsoft Graph<br/>OneDrive 文件](../articles/azure-functions/functions-bindings-microsoft-graph.md) ||✔| |✔|✔|
| [Microsoft Graph<br/>Outlook 电子邮件](../articles/azure-functions/functions-bindings-microsoft-graph.md)  ||✔| | |✔|
| [Microsoft Graph<br/>事件](../articles/azure-functions/functions-bindings-microsoft-graph.md)         ||✔|✔|✔|✔|
| [Microsoft Graph<br/>身份验证令牌](../articles/azure-functions/functions-bindings-microsoft-graph.md)    ||✔| |✔| |
| [移动应用](../articles/azure-functions/functions-bindings-mobile-apps.md)             |✔| | |✔|✔|
| [通知中心](../articles/azure-functions/functions-bindings-notification-hubs.md) |✔|| | |✔|
| [队列存储](../articles/azure-functions/functions-bindings-storage-queue.md)         |✔|✔|✔| |✔|
| [SendGrid](../articles/azure-functions/functions-bindings-sendgrid.md)                   |✔|✔| | |✔|
| [服务总线](../articles/azure-functions/functions-bindings-service-bus.md)             |✔|✔|✔| |✔|
| [表存储](../articles/azure-functions/functions-bindings-storage-table.md)         |✔|✔| |✔|✔|
| [计时器](../articles/azure-functions/functions-bindings-timer.md)                         |✔|✔|✔| | |

<sup>1</sup> 在版本 2.x 运行时中，除了 HTTP 和 Timer 以外，所有绑定都必须注册。 请参阅[注册绑定扩展](../articles/azure-functions/functions-bindings-register.md)。 除非另有说明，否则版本 3.x 也支持所有受支持的 2.x 绑定。   

