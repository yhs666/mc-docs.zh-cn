---
title: 创建认知服务文本分析资源
titleSuffix: Azure Cognitive Services
description: 了解如何创建认知服务文本分析资源。
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
origin.date: 06/26/2019
ms.date: 07/24/2019
ms.author: v-junlch
ms.openlocfilehash: 9707b67c5fcf2dee1bbf3627f6057bac38d1148d
ms.sourcegitcommit: 9a330fa5ee7445b98e4e157997e592a0d0f63f4c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/24/2019
ms.locfileid: "68440253"
---
## <a name="create-a-cognitive-services-text-analytics-resource"></a>创建认知服务文本分析资源

1. 登录到 [Azure 门户](https://portal.azure.cn)。
1. 选择“创建资源”  ，然后转到  “AI + 机器学习” > “文本分析”  。
   或者，转到 [创建文本分析](https://portal.azure.cn/#create/Microsoft.CognitiveServicesTextAnalytics)。
1. 输入所有必需的设置：

    |设置|值|
    |--|--|
    |Name|输入名称（2-64 个字符）|
    |订阅|选择适当的订阅|
    |Location|选择附近的位置|
    |定价层| 输入 **S**（即标准定价层）|
    |资源组|选择可用的资源组|

1. 选择“创建”  并等待创建资源。 浏览器会自动重定向到新创建的资源页。
1. 收集配置的 `endpoint` 和 API 密钥：

    |门户中的“资源”选项卡|设置|Value|
    |--|--|--|
    |**概述**|终结点|复制终结点。 其外观类似于 `https://chinaeast2.api.cognitive.azure.cn/text/analytics/v2.0`|
    |密钥 |API 密钥|复制两个密钥中的一个。 它是一个 32 个字符的字母数字字符串（不包含空格或短划线）：<`xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`>。|

<!-- Update_Description: wording update -->