---
title: 语言支持 - 语音服务
titleSuffix: Azure Cognitive Services
description: 语音服务支持多种语言进行语音到文本转换。 本文提供按服务功能分类的语言支持的完整列表。
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
origin.date: 11/04/2019
ms.date: 11/25/2019
ms.author: v-tawe
ms.custom: seodec18
ms.openlocfilehash: d36faa2c3f912984321006b00d002f0590d27897
ms.sourcegitcommit: 9e92bcf6aa02fc9e7b3a29abadf6b6d1a8ece8c4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2019
ms.locfileid: "74389505"
---
# <a name="language-and-region-support-for-the-speech-services"></a>语音服务的语言和区域支持

不同的语音服务函数支持不同的语言。 下表汇总了语言支持。

## <a name="speech-to-text"></a>语音转文本

Microsoft 语音 SDK 和 REST API 都支持以下语言（区域设置）。 若要提高准确性，可通过上传音频和人为标记的听录内容或相关文本，为语言子集提供自定义：句子。  发音自定义目前仅适用于 en-US 和 de-DE。 在[此处](how-to-custom-speech.md)详细了解自定义。

  Locale | 语言 | 支持 | 可自定义
 ------|------------|-----------|--------------|
 ar-EG | 阿拉伯语(埃及)，现代标准 | ✔️ | ✔️
 ar-SA | 阿拉伯语(沙特阿拉伯) | ✔️ | ✔️
 ar-AE | 阿拉伯语(阿拉伯联合酋长国) | ✔️ | ✔️
 ar-KW | 阿拉伯语(科威特) | ✔️ | ✔️
 ar-QA | 阿拉伯语(卡塔尔) | ✔️ | ✔️
 ca-ES | 加泰罗尼亚语 | ✔️ | ❌
 da-DK | 丹麦语(丹麦) | ✔️ | ❌
 de-DE | 德语(德国) | ✔️ | ✔️
 en-AU | 英语(澳大利亚) | ✔️ | ✔️
 en-CA | 英语(加拿大) | ✔️ | ✔️
 en-GB | 英语(英国) | ✔️ | ✔️
 en-IN | 英语(印度) | ✔️ | ✔️
 en-NZ | 英语(新西兰) | ✔️ | ✔️
 en-US | 英语(美国) | ✔️ | ✔️
 es-ES | 西班牙语(西班牙) | ✔️ | ✔️
 es-MX | 西班牙语(墨西哥) | ✔️ | ✔️
 fi-FI | 芬兰语(芬兰) | ✔️ | ❌
 fr-CA | 法语(加拿大) | ✔️ | ✔️
 fr-FR | 法语(法国) | ✔️ | ✔️
 gu-IN | 古吉拉特语(印度) | ✔️ | ✔️
 hi-IN | 印地语(印度) | ✔️ | ✔️
 it-IT | 意大利语(意大利) | ✔️ | ✔️
 ja-JP | 日语(日本) | ✔️ | ✔️
 ko-KR | 韩语(韩国) | ✔️ | ✔️
 mr-IN | 马拉地语(印度) | ✔️ | ✔️
 nb-NO | 书面挪威语(挪威) | ✔️ | ❌
 nl-NL | 荷兰语(荷兰) | ✔️ | ✔️
 pl-PL | 波兰语(波兰) | ✔️ | ❌
 pt-BR | 葡萄牙语(巴西) | ✔️ | ✔️
 pt-PT | 葡萄牙语(葡萄牙) | ✔️ | ✔️
 ru-RU | 俄语(俄罗斯) | ✔️ | ✔️
 sv-SE | 瑞典语(瑞典) | ✔️ | ❌
 ta-IN | 泰米尔语(印度) | ✔️ | ✔️
 te-IN | 泰卢固语(印度) | ✔️ | ✔️
 zh-CN | 中文(普通话，简体) | ✔️ | ✔️
 zh-HK | 中文(粤语，繁体) | ✔️ | ✔️
 zh-TW | 中文(台湾普通话) | ✔️ | ✔️
 th-TH | 泰语(泰国) | ✔️ | ❌
 tr-TR | 土耳其 | ✔️ | ✔️

<!-- ## Text-to-speech -->

<!-- speech-translation -->

## <a name="next-steps"></a>后续步骤

* [获取语音服务试用订阅](https://www.azure.cn/home/features/cognitive-services/)
* [了解如何在 C# 中识别语音](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-chsarp)
