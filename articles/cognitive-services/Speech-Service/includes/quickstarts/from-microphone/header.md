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
origin.date: 11/07/2019
ms.date: 11/25/2019
ms.author: v-tawe
ms.openlocfilehash: e84d1bcac1cda81dabf97caeaa5aedbb5c3f5bb1
ms.sourcegitcommit: 9e92bcf6aa02fc9e7b3a29abadf6b6d1a8ece8c4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2019
ms.locfileid: "74390042"
---
在本快速入门中，我们将使用[语音 SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) 以交互方式识别从麦克风捕获的音频数据中的语音。 满足几个先决条件后，通过麦克风识别语音只需四个步骤：

> [!div class="checklist"]
> * 通过订阅密钥和区域创建 `SpeechConfig` 对象。
> * 使用以上的 `SpeechConfig` 对象创建 `SpeechRecognizer` 对象。
> * 使用 `SpeechRecognizer` 对象，开始单个言语的识别过程。
> * 检查返回的 `SpeechRecognitionResult`。
