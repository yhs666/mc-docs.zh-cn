---
title: 容器支持
titleSuffix: Azure Cognitive Services
description: 了解如何创建认知服务文本分析资源。
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
origin.date: 06/26/2019
ms.date: 07/09/2019
ms.author: v-junlch
ms.openlocfilehash: 3e19ae499ea80bbc0d56e638cda962fdbba76734
ms.sourcegitcommit: 8f49da0084910bc97e4590fc1a8fe48dd4028e34
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/12/2019
ms.locfileid: "67844999"
---
## <a name="create-a-cognitive-services-text-analytics-resource"></a>创建认知服务文本分析资源

1. 登录到 [Azure 门户](https://portal.azure.cn)
1. 选择“+ 创建资源”  ，导航到“AI + 机器学习”>“文本分析”  ，或单击[“创建文本分析”  ](https://portal.azure.cn/#create/Microsoft.CognitiveServicesTextAnalytics)
1. 输入所有必需的设置：

    |设置|值|
    |--|--|
    |Name|所需名称（2-64 个字符）|
    |订阅|选择相应的订阅|
    |位置|选择附近的位置|
    |定价层|`S` - 标准定价层|
    |资源组|选择可用的资源组|

1. 单击“创建”  并等待创建资源。 创建后，浏览器会自动重定向到新创建的资源页
1. 收集配置的 `endpoint` 和 API 密钥：

    |门户中的“资源”选项卡|设置|Value|
    |--|--|--|
    |**概述**|终结点|复制终结点。 它看起来类似于 `https://chinanorth.api.cognitive.azure.cn/text/analytics/v2.0`|
    |密钥 |API 密钥|复制两个密钥中的 1 个。 它是一个由 32 个字母数字组成的字符串（不包含空格或短划线），即 `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`。|

