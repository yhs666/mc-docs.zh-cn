---
title: 语音转文本 - 语音服务
titleSuffix: Azure Cognitive Services
description: 使用语音转文本功能，可将音频流实时听录为可由应用程序、工具或设备根据命令输入使用、显示和处理的文本。
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
origin.date: 07/05/2019
ms.date: 11/25/2019
ms.author: v-tawe
ms.openlocfilehash: 0432d2feb1168b7ddb2becdf8ba3f3edbc9351d8
ms.sourcegitcommit: 9e92bcf6aa02fc9e7b3a29abadf6b6d1a8ece8c4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2019
ms.locfileid: "74389274"
---
# <a name="what-is-speech-to-text"></a>什么是语音转文本？

使用 Azure 语音服务中的语音转文本功能，可将音频流实时听录成可由应用程序、工具或设备根据命令输入使用、显示和处理的文本。 此服务由 Microsoft 对 Cortana 和 Office 产品使用的同一识别技术提供支持。  有关可用语音转文本语言的完整列表，请参阅[支持的语言](/cognitive-services/speech-service/language-support#speech-to-text)。

默认情况下，语音转文本服务使用通用语言模型。 此模型已使用 Microsoft 自有的数据训练，部署在云中。 它非常适合用于对话和听写方案。 如果使用语音转文本在独特的环境中进行识别和听录，则可以创建并训练自定义的声学、语言和发音模型，以解决环境干扰或行业特定的词汇。

可以轻松通过麦克风捕获音频，读取流中的数据，或使用语音 SDK 和 REST API 访问存储中的音频文件。 语音 SDK 支持在 WAV/PCM 16 位 16 kHz/8 kHz 单声道音频中进行语音识别。 可以使用[语音转文本 REST 终结点](/cognitive-services/speech-service/rest-apis)或[批量听录服务](/cognitive-services/speech-service/batch-transcription#supported-formats)支持其他音频格式。

## <a name="core-features"></a>核心功能

下面是可以通过语音 SDK 和 REST API 获得的功能：

| 使用案例 | SDK | REST |
|--------- | --- | ---- |
| 听录简短言语（15 秒以下）。 仅支持最终听录结果。 | 是 | 是 |
| 持续听录较长言语和流音频（15 秒以上）。 支持临时和最终听录结果。 | 是 | 否 |
| 使用 [LUIS](/cognitive-services/luis/what-is-luis) 从识别结果中派生意向。 | 是 | 否\* |
| 以异步方式批量听录音频文件。 | 否  | 是\*\* |
| 创建和管理语音模型。 | 否 | 是\*\* |
| 创建和管理自定义模型部署。 | 否  | 是\*\* |
| 创建准确度测试，以测量基线模型与自定义模型的准确度。 | 否  | 是\*\* |
| 管理订阅。 | 否  | 是\*\* |

\*_可以使用单独的 LUIS 订阅获取 LUIS 意向和实体。通过此订阅，SDK 可为你调用 LUIS 并提供实体和意向结果。使用 REST API，可以自己调用 LUIS 以使用 LUIS 订阅获取意向和实体。_

\*\*_可以通过 cris.azure.cn 终结点使用这些服务。请参阅 [Swagger 参考](https://chinaeast2.cris.azure.cn/swagger/ui/index)。_

## <a name="get-started-with-speech-to-text"></a>语音转文本入门

我们提供了适用于大多数流行编程语言的快速入门，旨在帮助你在 10 分钟以内运行代码。 [此表](/cognitive-services/speech-service/#5-minute-quickstarts)包含按平台和语言组织的语音 SDK 快速入门完整列表。  还可以在[此处](/cognitive-services/speech-service/#reference)找到 API 参考。

如果你偏向于使用语音转文本 REST 服务，请参阅 [REST API](https://docs.azure.cn/cognitive-services/speech-service/rest-apis)。

## <a name="sample-code"></a>示例代码

<!-- After you've had a chance to use the Speech Services, try our tutorial that teaches you how to recognize intents from speech using the Speech SDK and LUIS. -->

<!-- - [Tutorial: Recognize intents from speech with the Speech SDK and LUIS, C#](how-to-recognize-intents-from-speech-csharp.md) -->

GitHub 上提供了语音 SDK 的示例代码。 这些示例涵盖了常见方案，例如，从文件或流中读取音频、连续和单次识别，以及使用自定义模型。

- [语音转文本示例 (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)
- [批量听录示例 (REST)](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/batch)

## <a name="customization"></a>自定义

除了语音服务使用的标准基线模型外，还可以使用可用的数据根据需要自定义模型，以克服语音识别障碍，如讲话风格、词汇和背景噪声，请参阅[自定义语音](how-to-custom-speech.md)

> [!NOTE]
> 自定义选项因语言/区域设置而异（请参阅[支持的语言](supported-languages.md)）。

## <a name="migration-guides"></a>迁移指南

<!-- > [!WARNING] -->
<!-- > Bing Speech was decommissioned on October 15, 2019. -->

<!-- If your applications, tools, or products are using the Bing Speech APIs or Custom Speech, we've created guides to help you migrate to Speech Services. -->

<!-- - [Migrate from Bing Speech to the Speech Services](https://docs.azure.cn/cognitive-services/speech-service/how-to-migrate-from-bing-speech) -->

- [从自定义语音迁移到语音服务](/cognitive-services/speech-service/how-to-migrate-from-custom-speech-service)

## <a name="reference-docs"></a>参考文档

- [语音 SDK](/cognitive-services/speech-service/)
- [语音设备 SDK](speech-devices-sdk.md)
- [REST API：语音转文本](rest-speech-to-text.md)
- [REST API：批量听录和自定义](https://chinaeast2.cris.azure.cn/swagger/ui/index)

## <a name="next-steps"></a>后续步骤

- [获取语音服务订阅密钥以进行试用](get-started.md)
- [获取语音 SDK](speech-sdk.md)
