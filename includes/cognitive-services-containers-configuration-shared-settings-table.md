---
author: diberry
ms.author: v-junlch
ms.service: cognitive-services
ms.topic: include
origin.date: 05/15/2019
ms.date: 06/11/2019
ms.openlocfilehash: 8e7a915fdaf2590d0346083ce4f426760790aa85
ms.sourcegitcommit: 259c97c9322da7add9de9f955eac275d743c9424
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/11/2019
ms.locfileid: "66830151"
---
此容器具有以下配置设置：

|必须|设置|目的|
|--|--|--|
|是|[ApiKey](#apikey-configuration-setting)|用于跟踪账单信息。|
|是|[Billing](#billing-configuration-setting)|指定 Azure 上服务资源的终结点 URI。|
|是|[Eula](#eula-setting)| 表示已接受容器的许可条款。|
|否|[Fluentd](#fluentd-settings)|将日志和（可选）指标数据写入 Fluentd 服务器。|
|否|Http Proxy|配置 HTTP 代理以发出出站请求。|
|否|[Logging](#logging-settings)|为容器提供 ASP.NET Core 日志记录支持。 |
|否|[Mounts](#mount-settings)|从主计算机读取数据并将其写入到容器，以及从容器读回数据并将其写回到主计算机。|

