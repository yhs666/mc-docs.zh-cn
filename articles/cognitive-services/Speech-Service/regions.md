---
title: 区域 - 语音服务
titleSuffix: Azure Cognitive Services
description: 语音服务区域的参考。
services: cognitive-services
author: mahilleb-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
origin.date: 08/26/2019
ms.date: 03/12/2019
ms.author: v-lingwu
ms.custom: seodec18
ms.openlocfilehash: dcd766872233616ce28e6cb6425eaee6e6d3a992
ms.sourcegitcommit: 13642a99cc524a416b40635f48676bbf5cdcdf3d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2019
ms.locfileid: "70103745"
---
# <a name="speech-service-supported-regions"></a>语音服务支持的区域

通过语音服务，应用程序可将音频转换为文本、执行语音翻译以及将文本转换为语音。 多个区域中均提供该服务，这些区域为语音 SDK 和 REST API 使用唯一终结点。

请务必使用与订阅的区域匹配的终结点。

## <a name="speech-sdk"></a>语音 SDK

在[语音 SDK](speech-sdk.md) 中，区域指定为字符串（例如，在 C# 语音 SDK 中用作 `SpeechConfig.FromSubscription` 的参数）。

### <a name="speech-to-text-text-to-speech-and-translation"></a>语音转文本、文本转语音和翻译

可以在以下区域使用语音 SDK，以进行**语音识别**、**文本转语音**和**翻译**：


  区域 | 语音 SDK 参数 | 语音自定义门户
 ------|-------|--------
 美国西部 | `westus` | https://westus.cris.ai

### <a name="intent-recognition"></a>意向识别

通过语音 SDK 实现**意向识别**的可用区域如下：

 全球区域 | 区域 | 语音 SDK 参数
 ------|-------|--------
 中国 | 中国北部 | `chinanorth`

这是[语言理解服务 (LUIS)](/azure/cognitive-services/luis/luis-reference-regions) 支持的发布区域的子集。

### 语音优先虚拟助手 <a name="standard-and-neural-voices"></a>

[语音 SDK](speech-sdk.md) 在以下区域支持**语音优先虚拟助理**功能：

区域 | 语音 SDK 参数
-------|---------------------
中国北部 | `chinanorth`


## <a name="rest-apis"></a>REST API

语音服务还为语音转文本和文本转语音请求公开 REST 终结点。

### <a name="speech-to-text"></a>语音转文本

有关语音转文本的参考文档，请参阅 [REST API](https://docs.azure.cn/cognitive-services/speech-service/rest-apis)。

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-speech-to-text.md)]

### <a name="text-to-speech"></a>文本转语音

有关文本转语音的参考文档，请参阅 [REST API](https://docs.azure.cn/cognitive-services/speech-service/rest-apis)。

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-text-to-speech.md)]
