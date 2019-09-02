---
title: 区域 - 语音服务
titlesuffix: Azure Cognitive Services
description: 语音服务区域的参考。
services: cognitive-services
author: mahilleb-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
origin.date: 01/14/2019
ms.date: 04/01/2019
ms.author: v-biyu
ms.custom: seodec18
ms.openlocfilehash: b0dad12cece51bf197dc090d31ebd4aeffff7f92
ms.sourcegitcommit: b8fb6890caed87831b28c82738d6cecfe50674fd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/29/2019
ms.locfileid: "58625633"
---
# <a name="speech-service-supported-regions"></a>语音服务支持的区域

通过语音服务，应用程序可将音频转换为文本、执行语音翻译以及将文本转换为语音。 多个区域中均提供该服务，这些区域为语音 SDK 和 REST API 使用唯一终结点。

请务必使用与订阅的区域匹配的终结点。

## <a name="speech-sdk"></a>语音 SDK

在[语音 SDK](speech-sdk.md) 中，区域指定为字符串（例如，在 C# 语音 SDK 中用作 `SpeechConfig.FromSubscription` 的参数）。

### <a name="speech-recognition-and-translation"></a>语音识别和翻译

以下区域提供语音 SDK，用于“语音识别”和“翻译”：


| 区域 | 语音 SDK 参数 | 语音自定义门户 |
|--------|----------------------|-----------------------------|
|        |                      |                             |

### <a name="intent-recognition"></a>意向识别

通过语音 SDK 实现**意向识别**的可用区域如下：


| 全球区域 | 区域 | 语音 SDK 参数 |
|---------------|--------|----------------------|
|               |        |                      |

## <a name="rest-apis"></a>REST API

语音服务还为语音转文本和文本转语音请求公开 REST 终结点。

### <a name="speech-to-text"></a>语音转文本

有关语音转文本的参考文档，请参阅 [REST API](https://docs.azure.cn/cognitive-services/speech-service/rest-apis)。

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-speech-to-text.md)]

### <a name="text-to-speech"></a>文本转语音

有关文本转语音的参考文档，请参阅 [REST API](https://docs.azure.cn/cognitive-services/speech-service/rest-apis)。

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-text-to-speech.md)]
