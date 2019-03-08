---
author: diberry
ms.author: v-junlch
ms.service: cognitive-services
ms.topic: include
origin.date: 01/22/2019
ms.date: 03/01/2019
ms.openlocfilehash: 34265b3fa0ba8767d3db07b83b57d6acc9c43c0b
ms.sourcegitcommit: ea33f8dbf7f9e6ac90d328dcd8fb796241f23ff7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/01/2019
ms.locfileid: "57204205"
---
此容器具有以下配置设置：

|必须|设置|目的|
|--|--|--|
|是|[ApiKey](#apikey-configuration-setting)|用于跟踪账单信息。|
|是|[计费](#billing-configuration-setting)|指定 Azure 上服务资源的终结点 URI。|
|是|[Eula](#eula-setting)| 表示已接受容器的许可条款。|
|否|[Fluentd](#fluentd-settings)|将日志和（可选）指标数据写入 Fluentd 服务器。|
|否|[Http 代理](#http-proxy-credentials-settings)|配置 HTTP 代理以发出出站请求。|
|否|[日志记录](#logging-settings)|为容器提供 ASP.NET Core 日志记录支持。 |
|否|[Mounts](#mount-settings)|从主计算机读取数据并将其写入到容器，以及从容器读回数据并将其写回到主计算机。|

<!-- ms.date: 03/01/2019 -->