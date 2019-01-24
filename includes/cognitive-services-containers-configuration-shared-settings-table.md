---
author: WenJason
ms.author: v-jay
ms.service: cognitive-services
ms.topic: include
origin.date: 01/02/2019
ms.date: 01/28/2019
ms.openlocfilehash: 97e2196c233d6f918e1b4db34c5beb13eeda4e0b
ms.sourcegitcommit: f248afb1039011d34579baed2980f0632061f5b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2019
ms.locfileid: "54858081"
---
此容器具有以下配置设置：

|必须|设置|目的|
|--|--|--|
|是|[ApiKey](#apikey-setting)|用于跟踪账单信息。|
|是|[计费](#billing-setting)|指定 Azure 上服务资源的终结点 URI。|
|是|[Eula](#eula-setting)| 表示已接受容器的许可条款。|
|否|[Fluentd](#fluentd-settings)|将日志和（可选）指标数据写入 Fluentd 服务器。|
|否|[日志记录](#logging-settings)|为容器提供 ASP.NET Core 日志记录支持。 |
|否|[Mounts](#mount-settings)|从主计算机读取数据并将其写入到容器，以及从容器读回数据并将其写回到主计算机。|
