---
author: diberry
ms.author: v-junlch
ms.service: cognitive-services
ms.topic: include
origin.date: 01/22/2019
ms.date: 02/21/2019
ms.openlocfilehash: fe53025702edb09121cdac896c4abb88342d7132
ms.sourcegitcommit: 3ae99942621d28a8439ca1e7a7905caa5a3a10f9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/21/2019
ms.locfileid: "56582782"
---
此容器具有以下配置设置：

|必须|设置|目的|
|--|--|--|
|是|[ApiKey](#apikey-setting)|用于跟踪账单信息。|
|是|[计费](#billing-setting)|指定 Azure 上服务资源的终结点 URI。|
|是|[Eula](#eula-setting)| 表示已接受容器的许可条款。|
|否|[Fluentd](#fluentd-settings)|将日志和（可选）指标数据写入 Fluentd 服务器。|
|否|[Http 代理](#http-proxy-credentials-settings)|配置 HTTP 代理以发出出站请求。|
|否|[日志记录](#logging-settings)|为容器提供 ASP.NET Core 日志记录支持。 |
|否|[Mounts](#mount-settings)|从主计算机读取数据并将其写入到容器，以及从容器读回数据并将其写回到主计算机。|

<!-- ms.date: 02/21/2019 -->