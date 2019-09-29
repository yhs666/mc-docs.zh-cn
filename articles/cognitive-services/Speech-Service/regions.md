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
origin.date: 03/12/2019
ms.date: 09/23/2019
ms.author: v-tawe
ms.custom: seodec18
ms.openlocfilehash: b17224a138880b6a01dadcf5db1fe69ecfa2ce1f
ms.sourcegitcommit: b328fdef5f35155562f10817af44f2a4e975c3aa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2019
ms.locfileid: "71267049"
---
# <a name="speech-service-supported-regions"></a>语音服务支持的区域

通过语音服务，应用程序可将音频转换为文本以及将文本转换为语音。 多个区域中均提供该服务，这些区域为语音 SDK 和 REST API 使用唯一终结点。

请务必使用与订阅的区域匹配的终结点。

## <a name="speech-sdk"></a>语音 SDK

在[语音 SDK](speech-sdk.md) 中，区域指定为字符串（例如，在 C# 语音 SDK 中用作 `SpeechConfig.FromSubscription` 的参数）。

### <a name="text-to-speech"></a>文本转语音

语音 SDK 可在以下区域中用于**文本转语音**：

  区域 | 语音 SDK 参数 | 语音自定义门户
 ------|-------|--------
 中国东部 | `chinaeast` | https://chinaeast.cris.ai
 中国东部 2 | `chinaeast2` | https://chinaeast2.cris.ai
 中国北部 | `chinanorth` | https://chinanorth.cris.ai
 中国北部 2 | `chinanorth2` | https://chinanorth2.cris.ai

<!-- intent -->

<!-- voice first/bot service not available -->

## <a name="rest-apis"></a>REST API

<!-- speech-to-text -->

### <a name="text-to-speech"></a>文本转语音

有关文本转语音的参考文档，请参阅[文本转语音 REST API](rest-text-to-speech.md)。

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-text-to-speech.md)]
