---
title: 快速入门：识别音频文件中的语音 - 语音服务
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
origin.date: 10/28/2019
ms.date: 11/25/2019
ms.author: v-tawe
ms.openlocfilehash: eb4111634abb4a71111910b50b9949b67f0830fd
ms.sourcegitcommit: 9e92bcf6aa02fc9e7b3a29abadf6b6d1a8ece8c4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2019
ms.locfileid: "74390067"
---
在本快速入门中，我们将使用[语音 SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) 从音频文件识别语音。 满足几个先决条件后，从文件识别语音只需五个步骤：
> [!div class="checklist"]
> * 通过订阅密钥和区域创建 ````SpeechConfig```` 对象。
> * 创建指定 .WAV 文件名的 ````AudioConfig```` 对象。
> * 使用以上的 ````SpeechConfig```` 和 ````AudioConfig```` 对象创建 ````SpeechRecognizer```` 对象。
> * 使用 ````SpeechRecognizer```` 对象，开始单个言语的识别过程。
> * 检查返回的 ````SpeechRecognitionResult````。
