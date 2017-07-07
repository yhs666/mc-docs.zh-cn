---
title: "Azure 中继端口设置 | Microsoft Docs"
description: "有关 Azure 中继端口值的详细信息。"
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 03/24/2017
ms.date: 07/07/2017
ms.author: v-yiso
ms.translationtype: Human Translation
ms.sourcegitcommit: 457fc748a9a2d66d7a2906b988e127b09ee11e18
ms.openlocfilehash: a23b53f3e7259493fb2f88e9423a79efc4e1ec47
ms.contentlocale: zh-cn
ms.lasthandoff: 05/05/2017


---

# <a name="azure-relay-port-settings"></a>Azure 中继端口设置

下表描述 Azure 中继端口值的所需配置。

## <a name="hybrid-connections"></a>混合连接
混合连接将 WebSocket 用作仅使用 **HTTPS** 的基础传输机制。 

## <a name="wcf-relays"></a>WCF 中继
  
|绑定|传输安全|端口|  
|-------------|------------------------|----------|  
|[BasicHttpRelayBinding 类](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.basichttprelaybinding)（客户端）|是|HTTPS| 
| |" |否|HTTP|  
|[BasicHttpRelayBinding 类](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.basichttprelaybinding)（服务）|任一个|9351/HTTP|  
|[NetEventRelayBinding 类](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.neteventrelaybinding)（客户端）|是|9351/HTTPS|  
||" |否|9350/HTTP|  
|[NetEventRelayBinding 类](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.neteventrelaybinding)（服务）|任一个|9351/HTTP|  
|[NetTcpRelayBinding 类](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.nettcprelaybinding)（客户端/服务）|任一个|5671/9352/HTTP（9352/9353，如果使用混合）|  
|[NetOnewayRelayBinding 类](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.netonewayrelaybinding)（客户端）|是|9351/HTTPS|  
||" |否|9350/HTTP|  
|[NetOnewayRelayBinding 类](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.netonewayrelaybinding)（服务）|任一个|9351/HTTP|  
|[WebHttpRelayBinding 类](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.webhttprelaybinding)（客户端）|是|HTTPS|  
||" |否|HTTP|  
|[WebHttpRelayBinding 类](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.webhttprelaybinding)（服务）|任一个|9351/HTTP|  
|[WS2007HttpRelayBinding 类](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.ws2007httprelaybinding)（客户端）|是|HTTPS|  
||" |否|HTTP|  
|[WS2007HttpRelayBinding 类](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.ws2007httprelaybinding)（服务）|任一个|9351/HTTP|

## <a name="next-steps"></a>后续步骤
若要了解有关 Azure 中继的详细信息，请访问以下链接：
* [什么是 Azure 中继？](./relay-what-is-it.md)
* [中继常见问题](./relay-faq.md)
