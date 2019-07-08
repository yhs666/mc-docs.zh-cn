---
title: Azure 数据工厂中的 Webhook 活动 | Microsoft Docs
description: Webhook 活动在使用用户指定的某些条件验证附加的数据集之前，不会继续执行管道。
services: data-factory
documentationcenter: ''
author: WenJason
manager: digimobile
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
origin.date: 03/25/2019
ms.date: 07/08/2019
ms.author: v-jay
ms.openlocfilehash: 1d44be744aeb78f4574857f4e04fdbe6445c7bed
ms.sourcegitcommit: 5191c30e72cbbfc65a27af7b6251f7e076ba9c88
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/05/2019
ms.locfileid: "67570544"
---
# <a name="webhook-activity-in-azure-data-factory"></a>Azure 数据工厂中的 Webhook 活动
可以使用 Webhook 活动通过自定义代码控制管道的执行。 使用 Webhook 活动，客户可以调用终结点并传递回调 URL。 管道运行在继续下一个活动之前，等待调用回调。

## <a name="syntax"></a>语法

```json

{
    "name": "MyWebHookActivity",
    "type": "WebHook",
    "typeProperties": {
        "method": "POST",
        "url": "<URLEndpoint>",
        "headers": {
            "Content-Type": "application/json"
        },
        "body": {
            "key": "value"
        },
        "timeout": "00:03:00",
        "authentication": {
            "type": "ClientCertificate",
            "pfx": "****",
            "password": "****"
        }
    }
}

```


## <a name="type-properties"></a>Type 属性



属性 | 说明 | 允许的值 | 必须
-------- | ----------- | -------------- | --------
name | Webhook 活动的名称 | String | 是 |
type | 必须设置为 **WebHook**。 | String | 是 |
method | 目标终结点的 Rest API 方法。 | 字符串。 支持的类型：“POST” | 是 |
url | 目标终结点和路径 | 字符串（或带有 resultType 字符串的表达式）。 | 是 |
headers | 发送到请求的标头。 例如，若要在请求中设置语言和类型："headers" : { "Accept-Language": "en-us", "Content-Type": "application/json" }。 | 字符串（或带有 resultType 字符串的表达式） | 是，需要内容类型标头。 "headers":{ "Content-Type":"application/json"} |
body | 表示要发送到终结点的有效负载。 | 传递回回调 URI 的主体应该是有效的 JSON。 请参阅[请求有效负载架构](/data-factory/control-flow-web-activity#request-payload-schema)部分中的请求有效负载架构。 | 是 |
authentication | 用于调用该终结点的身份验证方法。 支持的类型为“Basic”或“ClientCertificate”。 有关详细信息，请参阅[身份验证](/data-factory/control-flow-web-activity#authentication)部分。 如果不需要身份验证，则排除此属性。 | 字符串（或带有 resultType 字符串的表达式） | 否 |
timeout | 活动将等待多长时间才能调用 &#39;callBackUri&#39;。 活动将等待多长时间才能调用“callBackUri”。 默认值为 10 分钟 (“00:10:00”)。 格式为 Timespan，即 d.hh:mm:ss | String | 否 |

## <a name="additional-notes"></a>附加说明

Azure 数据工厂会将正文中的附加属性“callBackUri”传递给 url 终结点，并且需要在达到指定超时值之前调用此 uri。 如果未调用此 uri，则活动将失败，其状态为“超时”。

仅当对自定义终结点的调用失败时，Webhook 活动本身才会失败。 可以将任何错误消息添加到回调正文中，并在后续活动中使用。

## <a name="next-steps"></a>后续步骤
查看数据工厂支持的其他控制流活动：

- [If Condition 活动](control-flow-if-condition-activity.md)
- [Execute Pipeline 活动](control-flow-execute-pipeline-activity.md)
- [For Each 活动](control-flow-for-each-activity.md)
- [Get Metadata 活动](control-flow-get-metadata-activity.md)
- [Lookup 活动](control-flow-lookup-activity.md)
- [Web 活动](control-flow-web-activity.md)
- [Until 活动](control-flow-until-activity.md)
