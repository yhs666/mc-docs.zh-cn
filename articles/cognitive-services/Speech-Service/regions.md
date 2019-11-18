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
ms.openlocfilehash: f4cc7f71d678fea4126c85c764be37bbf69ecbbc
ms.sourcegitcommit: c863b31d8ead7e5023671cf9b58415542d9fec9c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2019
ms.locfileid: "74020841"
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
 中国东部 2 | `chinaeast2` | https://chinaeast2.cris.azure.cn/

<!-- intent -->

<!-- voice first/bot service not available -->

## <a name="rest-apis"></a>REST API

<!-- speech-to-text -->

### <a name="text-to-speech"></a>文本转语音

有关文本转语音的参考文档，请参阅[文本转语音 REST API](rest-text-to-speech.md)。

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-text-to-speech.md)]
