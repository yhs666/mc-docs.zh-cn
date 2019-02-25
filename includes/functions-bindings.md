---
author: ggailey777
ms.service: azure-functions
ms.topic: include
origin.date: 09/04/2018
ms.date: 02/22/2019
ms.author: v-junlch
ms.openlocfilehash: 650085b9de723235346e83919eaab9da006969bd
ms.sourcegitcommit: 0fd74557936098811166d0e9148e66b350e5b5fa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/22/2019
ms.locfileid: "56665744"
---
下表显示了 Azure Functions 运行时的两个主要版本支持的绑定。

| 类型 | 1.x | 2.x<sup>1</sup> | 触发器 | 输入 | 输出 |  
| ---- | :-: | :-: | :------: | :---: | :----: |
| [Blob 存储](../articles/azure-functions/functions-bindings-storage-blob.md)          |✔|✔|✔|✔|✔|  
| [Cosmos DB](../articles/azure-functions/functions-bindings-documentdb.md)               |✔|✔|✔|✔|✔|  
| [事件中心](../articles/azure-functions/functions-bindings-event-hubs.md)              |✔|✔|✔| |✔|  
| [HTTP](../articles/azure-functions/functions-bindings-http-webhook.md)             |✔|✔|✔| |✔|
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
| [Webhook](../articles/azure-functions/functions-bindings-http-webhook.md)             |✔||✔| |✔|
  
<sup>1</sup> 在 2.x 中，除了 HTTP 和 Timer 以外，所有绑定都必须注册。 请参阅[注册绑定扩展](../articles/azure-functions/functions-triggers-bindings.md#register-binding-extensions)。

<sup>2</sup> 试验性 &mdash; 不受支持，将来可能被弃用。

<!-- ms.date: 02/22/2018 -->