---
title: 快速入门：从麦克风中识别语音 - 语音服务
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
origin.date: 11/20/2019
ms.date: 12/16/2019
ms.author: v-tawe
ms.openlocfilehash: bd222a6185fbb9d2db1ec5afae36c7ae1f66964a
ms.sourcegitcommit: 4a09701b1cbc1d9ccee46d282e592aec26998bff
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75469574"
---
在本快速入门中，我们将使用[语音 SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) 以交互方式识别从麦克风捕获的音频数据中的语音。 可轻松地将此功能集成到应用或设备中，用于执行对话听录等常见识别任务。 它还可用于更加复杂的集成，例如配合使用 Bot Framework 和语音 SDK 来生成语音助手。

满足几个先决条件后，通过麦克风识别语音只需四个步骤：

> [!div class="checklist"]
> * 通过订阅密钥和区域创建 `SpeechConfig` 对象。
> * 使用以上的 `SpeechConfig` 对象创建 `SpeechRecognizer` 对象。
> * 使用 `SpeechRecognizer` 对象，开始单个言语的识别过程。
> * 检查返回的 `SpeechRecognitionResult`。
